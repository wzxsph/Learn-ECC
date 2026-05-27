# Script công cụ

Tài liệu này giới thiệu các script công cụ chính trong dự án ECC (Everything Claude Code).

## Mục lục

- [ecc.js](#eccjs) - Điểm vào CLI chính
- [install-apply.js](#install-applyjs) - Trình thực thi cài đặt
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Kiểm toán toolchain
- [catalog.js](#catalogjs) - Trình xem catalog component
- [build-opencode.js](#build-opencodejs) - Xây dựng OpenCode

---

## ecc.js

**Đường dẫn**: `scripts/ecc.js`

Điểm vào CLI chính để cài đặt có chọn lọc ECC, điều phối tất cả subcommand.

### Danh sách lệnh

| Lệnh | Script | Mô tả |
|------|------|----------|
| `install` | install-apply.js | Cài đặt nội dung ECC vào target |
| `plan` | install-plan.js | Kiểm tra manifest và phân tích kế hoạch cài đặt |
| `catalog` | catalog.js | Khám phá file cấu hình và component ID đã cài đặt |
| `consult` | consult.js | Đề xuất component dựa trên truy vấn ngôn ngữ tự nhiên |
| `list-installed` | list-installed.js | Kiểm tra trạng thái cài đặt trong context hiện tại |
| `doctor` | doctor.js | Chẩn đoán file quản lý ECC bị thiếu hoặc drift |
| `repair` | repair.js | Khôi phục file quản lý ECC bị drift hoặc thiếu |
| `auto-update` | auto-update.js | Kéo các thay đổi ECC mới nhất và cài đặt lại |
| `status` | status.js | Truy vấn tóm tắt lưu trữ SQLite |
| `platform-audit` | platform-audit.js | Kiểm toán GitHub queue, thảo luận, roadmap |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Quét IOC chuỗi cung ứng cho dependencies và AI tools |
| `sessions` | sessions-cli.js | Liệt kê hoặc kiểm tra phiên trong lưu trữ SQLite |
| `work-items` | work-items.js | Theo dõi Linear, GitHub work items |
| `session-inspect` | session-inspect.js | Snapshot phiên chuẩn từ dmux hoặc Claude history target |
| `loop-status` | loop-status.js | Kiểm tra loop wake và pending tool results cũ trong Claude scripts |
| `uninstall` | uninstall.js | Xóa file quản lý ECC khỏi install-state |

### Ví dụ sử dụng

```bash
# Cài đặt ngôn ngữ trực tiếp
ecc typescript

# Cài đặt với file cấu hình
ecc install --profile developer --target claude

# Xem kế hoạch cài đặt
ecc plan --profile core --target cursor

# Liệt kê các component khả dụng
ecc catalog profiles
ecc catalog components --family language

# Hiển thị chi tiết component
ecc catalog show framework:nextjs

# Tư vấn đề xuất
ecc consult "security reviews"

# Kiểm tra đã cài đặt
ecc list-installed --json

# Chẩn đoán vấn đề
ecc doctor --target cursor
ecc repair --dry-run

# Tự động cập nhật
ecc auto-update --dry-run

# Truy vấn trạng thái
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Kiểm toán platform
ecc platform-audit --json --allow-untracked docs/drafts/

# Quét bảo mật
ecc security-ioc-scan --home

# Quản lý phiên
ecc sessions
ecc sessions session-active --json

# Theo dõi work items
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Kiểm tra trạng thái loop
ecc loop-status --json

# Gỡ cài đặt
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Đường dẫn**: `scripts/install-apply.js`

Runtime cài đặt ECC, duy trì các entry point cài đặt ngôn ngữ legacy đồng thời di chuyển logic thay đổi target-specific vào Node code có thể kiểm thử.

### Các target được hỗ trợ

| Target | Mô tả |
|------|----------|
| `claude` | Cài đặt vào ~/.claude/, rules/skills vào rules/ecc và skills/ecc |
| `claude-project` | Cài đặt vào ./.claude/ (project-level) |
| `cursor` | Cài đặt rules, hooks, cursor config vào ./.cursor/ |
| `antigravity` | Cài đặt rules, workflow, skills, agents vào ./.agent/ |
| `codex` | Cài đặt shared agents/config vào ~/.codex/ |
| `gemini` | Cài đặt project-level Gemini config vào ./.gemini/ |
| `opencode` | Cài đặt shared commands/hooks/config vào ~/.opencode/ |
| `codebuddy` | Cài đặt commands, agents, skills vào ./.codebuddy/ |
| `joycode` | Cài đặt commands, agents, skills vào ./.joycode/ |
| `qwen` | Cài đặt vào ~/.qwen/ |
| `zed` | Cài đặt vào ./.zed/ |

### Các phương thức cài đặt

1. **Legacy Language Mode**: `install.sh <language>` (ví dụ `ecc-install typescript`)
2. **Profile Mode**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Module Mode**: `install.sh --modules <id,id,...>`
4. **Skill Mode**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Localization Mode**: `install.sh --target claude|claude-project --locale <locale-code>`

### Tùy chọn

- `--dry-run` - Hiển thị kế hoạch cài đặt nhưng không sao chép file
- `--json` - Xuất định dạng JSON có thể đọc bằng máy
- `--config <path>` - Tải ý định cài đặt từ file

---

## claw.js

**Đường dẫn**: `scripts/claw.js`

NanoClaw v2 - REPL không dependencies bên ngoài, có nhận thức phiên cho Everything Claude Code, được xây dựng trên `claude -p`.

### Chức năng

- Persistence lưu trữ lịch sử phiên trong `~/.claude/claw/`
- Hỗ trợ dynamic loading skills như context
- Compact phiên để quản lý độ dài context
- Branch và export phiên
- Tìm kiếm cross-session

### Lệnh REPL

| Lệnh | Mô tả |
|------|----------|
| `/help` | Hiển thị trợ giúp |
| `/clear` | Xóa lịch sử phiên hiện tại |
| `/history` | In toàn bộ lịch sử hội thoại |
| `/sessions` | Liệt kê các phiên đã lưu |
| `/model [name]` | Hiển thị/đặt model |
| `/load <skill-name>` | Load skill vào context hiện tại |
| `/branch <session-name>` | Branch phiên hiện tại sang phiên mới |
| `/search <query>` | Tìm kiếm cross-session |
| `/compact` | Giữ lại các vòng gần đây, compact context cũ |
| `/export <md\|json\|txt> [path]` | Export phiên |
| `/metrics` | Hiển thị metrics phiên |
| `exit` | Thoát REPL |

### Biến môi trường

| Biến | Mặc định | Mô tả |
|------|---------|----------|
| `CLAW_SESSION` | `default` | Tên phiên ban đầu |
| `CLAW_MODEL` | `sonnet` | Model mặc định |
| `CLAW_SKILLS` | rỗng | Danh sách skills được load (phân cách bằng dấu phẩy) |

### Ví dụ sử dụng

```bash
# Khởi động với phiên mặc định
node scripts/claw.js

# Khởi động với phiên được chỉ định
CLAW_SESSION=my-session node scripts/claw.js

# Khởi động với skills
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Đường dẫn**: `scripts/harness-audit.js`

Công cụ kiểm toán toolchain deterministic, dựa trên kiểm tra file/rule rõ ràng. Tự động phát hiện ECC repo mode vs consumer project mode.

### Các danh mục kiểm toán

| Danh mục | Mô tả |
|------|----------|
| Tool Coverage | Hoàn thiện tool coverage |
| Context Efficiency | Hiệu quả context |
| Quality Gates | Quality gates |
| Memory Persistence | Persistence bộ nhớ |
| Eval Coverage | Eval coverage |
| Security Guardrails | Security guardrails |
| Cost Efficiency | Hiệu quả chi phí |
| GitHub Integration | Tích hợp GitHub |
| Vercel/Netlify/Cloudflare/Fly Integration | Tích hợp các platform |

### Ví dụ sử dụng

```bash
# Kiểm toán thư mục hiện tại
node scripts/harness-audit.js

# Chỉ định scope
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Chỉ định root directory
node scripts/harness-audit.js --root /path/to/project

# Định dạng JSON output
node scripts/harness-audit.js --format json

# Kết hợp sử dụng
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Đường dẫn**: `scripts/catalog.js`

Công cụ khám phá các component và file cấu hình đã cài đặt của ECC.

### Lệnh

#### profiles

Liệt kê tất cả file cấu hình cài đặt.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

Liệt kê các component đã cài đặt, có thể lọc theo family và target.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Hiển thị chi tiết của một component cụ thể.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Component families

- `baseline` - Component cơ sở
- `language` - Component ngôn ngữ (javascript, typescript, python, etc.)
- `framework` - Component framework (nextjs, react, vue, etc.)
- `capability` - Component capability
- `agent` - Component agent
- `skill` - Component skill

---

## build-opencode.js

**Đường dẫn**: `scripts/build-opencode.js`

Xây dựng mã TypeScript cho plugin OpenCode.

### Chức năng

1. Dọn dẹp thư mục `dist`
2. Biên dịch mã `.opencode` sang `.opencode/dist`

### Điều kiện tiên quyết

Yêu cầu dev dependencies đã được cài đặt từ root, đảm bảo trình biên dịch TypeScript khả dụng.

### Cách sử dụng

```bash
node scripts/build-opencode.js
```