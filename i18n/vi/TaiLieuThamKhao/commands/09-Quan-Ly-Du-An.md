# Lệnh Quản Lý Dự Án

## Tổng quan

Lệnh quản lý dự án dùng để cấu hình project, thiết lập infrastructure và quản lý toolchain.

## Danh sách lệnh

### /projects

**Mục đích**: Liệt kê các project đã biết + instinct statistics

**Mô tả**: Liệt kê tất cả project của user cùng với instinct statistics liên quan.

---

### /harness-audit

**Mục đích**: Audit agent harness configuration

**Mô tả**: Kiểm tra và audit Claude Code harness configuration.

**Kiểm tra nội dung**:
- Toolchain configuration
- Agent settings
- MCP server status
- Rules configuration

---

### /model-route

**Mục đích**: Route task đến model phù hợp

**Mô tả**: Chọn model phù hợp nhất theo task type (Haiku/Sonnet/Opus).

**Model selection guide**:
- **Haiku**: Task nhẹ, code generation, frequent invocation
- **Sonnet**: Main development work, complex coding task
- **Opus**: Architectural decision, deep reasoning, research analysis

---

### /pm2

**Mục đích**: PM2 process manager initialization

**Mô tả**: Initialize PM2 process manager configuration.

---

### /setup-pm

**Mục đích**: Configure package manager

**Mô tả**: Configure package manager mà project sử dụng.

**Supported package manager**:
- npm
- pnpm
- yarn
- bun

**Có thể configure qua** `CLAUDE_PACKAGE_MANAGER` **environment variable**

---

### /project-init

**Mục đích**: Project initialization

**Mô tả**: Initialize standard structure và configuration cho project mới.

---

## Các lệnh liên quan

- `/projects` - Liệt kê project
- `/harness-audit` - Audit configuration
- `/model-route` - Model routing