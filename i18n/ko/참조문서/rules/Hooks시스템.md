# Hooks 시스템

## Hook 유형

- **PreToolUse**: 도구실행前（검증、参数修改）
- **PostToolUse**: 도구실행后（자동형식化、검사）
- **Stop**: 세션종료时（最终검증）

## 자동수용权限

谨慎사용：
- 为可信的、定义明确的계획활성화
- 为探索性작업비활성화
- 切勿사용 dangerously-skip-permissions 标志
- 在 `~/.claude.json` 中설정 `allowedTools`

## TodoWrite 모범 사례

사용 TodoWrite 도구：
- 추적多단계태스크的进度
- 검증对指令的理解
- 활성화실시간指导
- 표시粒度实施단계

Todo 列表揭示：
- 乱序단계
- 缺失프로젝트
- 多余的필요 없음要프로젝트
- 오류的粒度
- 误解的需求
