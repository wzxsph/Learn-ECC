---
name: cost-aware-llm-pipeline
description: LLM API 使用成本优化模式 — 按任务复杂度路由模型、预算跟踪、重试逻辑和提示缓存。
origin: ECC
---

# 成本感知 LLM 管道

在保持质量的同时控制 LLM API 成本的模式。将模型路由、预算跟踪、重试逻辑和提示缓存组合成可组合的管道。

## 何时激活

- 构建调用 LLM API 的应用（Claude、GPT 等）
- 处理具有不同复杂度的批量项目
- 需要控制 API 支出预算
- 在不牺牲复杂任务质量的前提下优化成本

## 核心概念

### 1. 按任务复杂度路由模型

自动为简单任务选择更便宜的模型，为复杂任务保留昂贵的模型。

```python
MODEL_SONNET = "claude-sonnet-4-6"
MODEL_HAIKU = "claude-haiku-4-5-20251001"

_SONNET_TEXT_THRESHOLD = 10_000  # 字符数
_SONNET_ITEM_THRESHOLD = 30     # 项目数

def select_model(
    text_length: int,
    item_count: int,
    force_model: str | None = None,
) -> str:
    """根据任务复杂度选择模型。"""
    if force_model is not None:
        return force_model
    if text_length >= _SONNET_TEXT_THRESHOLD or item_count >= _SONNET_ITEM_THRESHOLD:
        return MODEL_SONNET  # 复杂任务
    return MODEL_HAIKU  # 简单任务（便宜 3-4 倍）
```

### 2. 不可变成本跟踪

使用冻结数据类跟踪累计支出。每次 API 调用返回新的跟踪器 — 绝不修改状态。

```python
from dataclasses import dataclass

@dataclass(frozen=True, slots=True)
class CostRecord:
    model: str
    input_tokens: int
    output_tokens: int
    cost_usd: float

@dataclass(frozen=True, slots=True)
class CostTracker:
    budget_limit: float = 1.00
    records: tuple[CostRecord, ...] = ()

    def add(self, record: CostRecord) -> "CostTracker":
        """返回添加记录后的新跟踪器（绝不修改 self）。"""
        return CostTracker(
            budget_limit=self.budget_limit,
            records=(*self.records, record),
        )

    @property
    def total_cost(self) -> float:
        return sum(r.cost_usd for r in self.records)

    @property
    def over_budget(self) -> bool:
        return self.total_cost > self.budget_limit
```

### 3. 精准重试逻辑

仅对临时性错误重试。对认证错误或错误请求快速失败。

```python
from anthropic import (
    APIConnectionError,
    InternalServerError,
    RateLimitError,
)

_RETRYABLE_ERRORS = (APIConnectionError, RateLimitError, InternalServerError)
_MAX_RETRIES = 3

def call_with_retry(func, *, max_retries: int = _MAX_RETRIES):
    """仅对临时性错误重试，其他错误快速失败。"""
    for attempt in range(max_retries):
        try:
            return func()
        except _RETRYABLE_ERRORS:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)  # 指数退避
    # AuthenticationError、BadRequestError 等 → 立即抛出
```

### 4. 提示缓存

缓存长的系统提示以避免每次请求都重新发送。

```python
messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": system_prompt,
                "cache_control": {"type": "ephemeral"},  # 缓存此部分
            },
            {
                "type": "text",
                "text": user_input,  # 变化部分
            },
        ],
    }
]
```

## 组合使用

在单一管道函数中组合所有四种技术：

```python
def process(text: str, config: Config, tracker: CostTracker) -> tuple[Result, CostTracker]:
    # 1. 路由模型
    model = select_model(len(text), estimated_items, config.force_model)

    # 2. 检查预算
    if tracker.over_budget:
        raise BudgetExceededError(tracker.total_cost, tracker.budget_limit)

    # 3. 调用（重试 + 缓存）
    response = call_with_retry(lambda: client.messages.create(
        model=model,
        messages=build_cached_messages(system_prompt, text),
    ))

    # 4. 跟踪成本（不可变）
    record = CostRecord(model=model, input_tokens=..., output_tokens=..., cost_usd=...)
    tracker = tracker.add(record)

    return parse_result(response), tracker
```

## 价格参考（2025-2026）

| 模型 | 输入（$/1M tokens） | 输出（$/1M tokens） | 相对成本 |
|-------|---------------------|----------------------|---------------|
| Haiku 4.5 | $0.80 | $4.00 | 1x |
| Sonnet 4.6 | $3.00 | $15.00 | 约 4x |
| Opus 4.5 | $15.00 | $75.00 | 约 19x |

## 最佳实践

- **从最便宜的模型开始**，仅在达到复杂度阈值时才路由到昂贵模型
- **处理批量前设置明确的预算限制** — 及早失败而不是超支
- **记录模型选择决策**，以便根据真实数据调整阈值
- **系统提示超过 1024 tokens 时使用提示缓存** — 节省成本和延迟
- **绝不重试认证或验证错误** — 仅重试临时性失败（网络、限流、服务器错误）

## 应避免的反模式

- 对所有请求使用最贵的模型，不管复杂度如何
- 对所有错误都重试（浪费预算在永久性失败上）
- 修改成本跟踪状态（使调试和审计困难）
- 在整个代码库中硬编码模型名称（使用常量或配置）
- 对重复的系统提示忽略提示缓存

## 何时使用

- 任何调用 Claude、OpenAI 或类似 LLM API 的应用
- 成本快速累积的批量处理管道
- 需要智能路由的多模型架构
- 需要预算保护机制的生产系统

## 相关

- 智能体: `planner` — 实现规划
- 技能: `backend-patterns` — API 和服务层模式
- 技能: `context-budget` — 上下文预算分析