# 02 - Cài đặt và cấu hình

## Yêu cầu môi trường

- Node.js >= 18
- npm / pnpm / yarn / bun một trong các công cụ này
- Claude Code CLI v2.1.0+ đã cài đặt

## Cách cài đặt

### Cách 1: Cài đặt qua plugin (Khuyến nghị)

#### 1. Thêm marketplace plugin

```bash
/plugin marketplace add https://github.com/affaan-m/ECC
```

#### 2. Cài đặt plugin

```bash
/plugin install ecc@ecc
```

#### 3. Cài đặt Rules (plugin không tự động cài)

```bash
# Clone repo
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Copy rules vào thư mục user
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
# Chọn tech stack của bạn
cp -r rules/typescript ~/.claude/rules/ecc/   # hoặc python, golang, swift, php
```

### Cách 2: Cài đặt thủ công

Nếu bạn muốn kiểm soát tinh tế hơn:

```bash
# Clone repo
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Cài đặt dependencies
npm install        # hoặc pnpm install | yarn install | bun install

# Cài đặt agents
cp agents/*.md ~/.claude/agents/

# Cài đặt skills (bề mặt workflow chính)
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/

# Cài đặt rules
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/

# Cài đặt commands (tùy chọn, ECC đang chuyển sang skills)
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

## Xác minh cài đặt

```bash
# Kiểm tra plugin đã cài đặt chưa
/plugin list ecc@ecc

# Kiểm tra các lệnh khả dụng
/ecc:plan
/ecc:code-review
```

## Cấu hình cơ bản

### Đặt trình quản lý package

ECC tự động phát hiện trình quản lý package bạn thích, cũng có thể đặt thủ công:

```bash
# Qua biến môi trường
export CLAUDE_PACKAGE_MANAGER=pnpm

# Hoặc dùng lệnh
/setup-pm
```

Ưu tiên: Biến môi trường > Cấu hình dự án > package.json > lock file > Cấu hình global

### Kiểm soát runtime Hook

```bash
# Mức độ nghiêm ngặt của Hook (mặc định: standard)
export ECC_HOOK_PROFILE=standard

# Vô hiệu hóa Hook cụ thể
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Giới hạn context SessionStart (mặc định: 8000 ký tự)
export ECC_SESSION_START_MAX_CHARS=4000
```

### Cài đặt model

Khuyến nghị đặt trong `~/.claude/settings.json`:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

## Bước tiếp theo

- [Lệnh cơ bản](./03-LenhCoBan.md) - Học các lệnh thường dùng
- [Quay lại thư mục nhập môn](../README.md)