# Hooks 系统

## Hook Türleri

- **PreToolUse**: 工具执行前（Doğrulama、参数修改）
- **PostToolUse**: 工具执行后（自动格式化、检查）
- **Stop**: 会话结束时（最终Doğrulama）

## 自动接受权限

谨慎使用：
- 为可信的、定义明确的计划启用
- 为探索性工作禁用
- 切勿使用 dangerously-skip-permissions 标志
- 在 `~/.claude.json` 中配置 `allowedTools`

## TodoWrite En İyi Uygulamalar

使用 TodoWrite 工具：
- 跟踪多AdımlarGörev的进度
- Doğrulama对指令的理解
- 启用实时指导
- 显示粒度实施Adımlar

Todo 列表揭示：
- 乱序Adımlar
- 缺失项目
- 多余的不必要项目
- 错误的粒度
- 误解的需求
