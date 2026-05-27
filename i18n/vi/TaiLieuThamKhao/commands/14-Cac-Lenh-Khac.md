# Các Lệnh Khác

## Tổng quan

Các lệnh miscellaneous khác, bao gồm Jira integration, GAN operation, security scan và các chức năng khác.

## Danh sách lệnh

### /jira

**Mục đích**: Tương tác với Jira ticket - get, analyze, comment, transition status, search

**Mô tả**: Tích hợp với Jira project management tool, hỗ trợ làm việc với Jira ticket trực tiếp trong workflow.

**Cách dùng**:

| Lệnh | Mô tả |
|---|---|
| `/jira get <TICKET-KEY>` | Get và analyze Jira ticket |
| `/jira comment <TICKET-KEY>` | Thêm progress comment |
| `/jira transition <TICKET-KEY>` | Thay đổi ticket status |
| `/jira search <JQL>` | Search issue dùng JQL |

**`/jira get` workflow**:
1. Get ticket từ Jira (qua MCP hoặc REST API)
2. Extract all field: summary, description, acceptance criteria, priority, label, linked issue
3. Optional get comment để lấy additional context
4. Generate structured analysis

**Prerequisite** - Jira credential configuration (chọn một):

**Option A — MCP Server (Khuyến khích)**:
Thêm `jira` vào `mcpServers` configuration.

**Option B — Environment variable**:
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

**Tích hợp với các lệnh khác**:
- Sau khi analyze ticket, dùng `/plan` để tạo implementation plan
- Dùng `tdd-workflow` skill cho test-driven development
- Dùng `/code-review` để review implementation
- Dùng `/jira comment` hoặc `/jira transition` để update ticket status

---

### /gan-build (Cần hoàn thiện)

**Mục đích**: GAN build operation

**Mô tả**: GAN (Generative Agent Network) build operation.

---

### /gan-design (Cần hoàn thiện)

**Mục đích**: GAN design operation

**Mô tả**: GAN design related operation.

---

### /prune

**Mục đích**: Xóa stale pending instinct quá 30 ngày chưa promote

**Mô tả**: Cleanup stale instinct quá 30 ngày, những instinct này được auto-generate nhưng chưa bao giờ được review hoặc promote.

**Cách dùng**:
```
/prune                    # Xóa instinct quá 30 ngày
/prune --max-age 60      # Tùy chỉnh ngày threshold
/prune --dry-run         # Preview mà không xóa
```

**Implementation**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

Hoặc khi manual install (`CLAUDE_PLUGIN_ROOT` not set):
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**Tham số**:

| Tham số | Mô tả |
|---|---|
| (không có) | Xóa instinct quá 30 ngày |
| `--max-age <days>` | Tùy chỉnh threshold (ngày) |
| `--dry-run` | Preview nội dung sẽ xóa, không thực sự xóa |

**Best practice**:
- Dùng `--dry-run` trước để preview nội dung sẽ xóa
- Chạy prune định kỳ để giữ instinct inventory sạch
- Lưu ý: chỉ xóa instinct ở pending status, không xóa instinct đã promote

---

### /security-scan

**Mục đích**: Security scan

**Mô tả**: Security vulnerability scan cho codebase.

**Scan nội dung**:
- Hardcoded credential
- SQL injection risk
- XSS vulnerability
- Dependency vulnerability

---

### /feature-dev (Cần hoàn thiện)

**Mục đích**: Feature development assistant

**Mô tả**: Cung cấp workflow hỗ trợ cho feature development.

---

### /cost-report

**Mục đích**: Model cost report

**Mô tả**: Generate AI model usage cost report.

---

## Các lệnh liên quan

- `/security-scan` - Security scan
- `/jira` - Jira integration
- `/prune` - Cleanup instinct