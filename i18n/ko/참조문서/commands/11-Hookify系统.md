# Hookify 시스템명령

## 개요

Hookify 시스템用于생성和管理 hooks，防止不良行为和자동化워크플로우。

## 명령列表

### /hookify

**용도**: 생성 hooks 防止不良行为

**描述**: 생성自定义 hooks 来防止不希望发生的行为。

**사용场景**:
- 防止意外삭제操作
- 阻止危险명령
- 자동형식化
- 품질검사

**Hook 유형**:
- **PreToolUse**: 도구실행前
- **PostToolUse**: 도구실행后
- **Stop**: 세션종료时

---

### /hookify-list

**용도**: 列出모든설정的 hookify 규칙

**描述**: 표시当前모든已설정的 hookify 규칙。

---

### /hookify-configure

**용도**: 交互式활성화/비활성화 hookify 규칙

**描述**: 交互式설정 hookify 규칙的활성화和비활성화状态。

---

### /hookify-help

**용도**: Hookify 시스템帮助

**描述**: 가져오기 Hookify 시스템的详细帮助정보。

---

## Hook 설정예제

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## 相关명령

- `/hookify` - 생성 hook
- `/hookify-list` - 列出규칙
- `/hookify-configure` - 설정규칙
- `/hookify-help` - 가져오기帮助
