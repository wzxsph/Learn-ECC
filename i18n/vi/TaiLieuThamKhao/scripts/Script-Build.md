# Script xây dựng

Tài liệu này giới thiệu các script xây dựng và phát hành của dự án ECC.

---

## release.sh

**Đường dẫn**: `scripts/release.sh`

Script nâng cấp và phát hành phiên bản plugin.

### Chức năng

1. **Xác minh phiên bản**
   - Xác nhận phiên bản semver được cung cấp (X.Y.Z hoặc X.Y.Z-prerelease)
   - Xác nhận nhánh hiện tại là main
   - Xác nhận working tree sạch

2. **Cập nhật file**
   - Cập nhật số phiên bản trong tất cả manifest của gói/plugin
   - Cập nhật tham chiếu phiên bản trong tài liệu
   - Cập nhật cấu hình agent

3. **Xác minh xây dựng**
   - Chạy xây dựng OpenCode
   - Chạy kiểm thử xây dựng
   - Chạy kiểm thử manifest plugin

4. **Thao tác Git**
   - Stage tất cả thay đổi
   - Tạo commit
   - Tạo và push tag

### Cách sử dụng

```bash
# Phát hành phiên bản mới
./scripts/release.sh 1.5.0

# Phiên bản pre-release
./scripts/release.sh 2.0.0-rc.1
```

### Các file được cập nhật

| File | Nội dung cập nhật |
|------|----------|
| `package.json` | Trường version |
| `package-lock.json` | version và packages[""].version |
| `AGENTS.md` | Dòng Version |
| `docs/tr/AGENTS.md` | Dòng Version |
| `docs/zh-CN/AGENTS.md` | Dòng Version |
| `agent.yaml` | Trường version |
| `VERSION` | File phiên bản |
| `.claude-plugin/plugin.json` | Trường version |
| `.claude-plugin/marketplace.json` | Trường version |
| `.agents/plugins/marketplace.json` | Phiên bản plugin ecc |
| `.codex-plugin/plugin.json` | Trường version |
| `.opencode/package.json` | Trường version |
| `.opencode/package-lock.json` | version và packages[""].version |
| `.opencode/plugins/ecc-hooks.ts` | Banner version |
| `README.md` | Dòng bảng version |
| `README.zh-CN.md` | Dòng bảng version |
| `docs/tr/README.md` | Tiêu đề phát hành mới nhất |
| `docs/pt-BR/README.md` | Tiêu đề phát hành mới nhất |
| `docs/zh-CN/README.md` | Tiêu đề phát hành mới nhất |
| `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` | Ví dụ repoVersion |

### Điều kiện tiên quyết

- Git working tree phải sạch
- Phải ở trên nhánh main
- Tất cả file manifest phải tồn tại

---

## build-opencode.js

**Đường dẫn**: `scripts/build-opencode.js`

Xây dựng mã TypeScript cho plugin OpenCode.

### Quy trình xây dựng

1. Dọn dẹp thư mục `dist`
2. Tìm trình biên dịch TypeScript (`typescript/bin/tsc`)
3. Biên dịch thư mục `.opencode` bằng tsconfig.json từ root

### Cách sử dụng

```bash
# Xây dựng OpenCode
node scripts/build-opencode.js

# Tự động chạy như một phần của release
./scripts/release.sh 1.5.0
```

### Điều kiện tiên quyết

Yêu cầu dev dependencies đã được cài đặt từ root, đảm bảo trình biên dịch TypeScript khả dụng.

---

## Các script khác

### gan-harness.sh

**Đường dẫn**: `scripts/gan-harness.sh`

Công cụ liên quan đến thử nghiệm GAN (General Agent Network).

### orchestrate-codex-worker.sh

**Đường dẫn**: `scripts/orchestrate-codex-worker.sh`

Điều phối các worker process Codex.

### sync-ecc-to-codex.sh

**Đường dẫn**: `scripts/sync-ecc-to-codex.sh`

Đồng bộ ECC sang Codex.

---

## Quy trình phát hành

```
1. Đảm bảo ở nhánh main và working tree sạch
2. Chạy script phát hành với số phiên bản
3. Script tự động:
   ├── Xác minh điều kiện tiên quyết
   ├── Cập nhật tất cả tham chiếu phiên bản
   ├── Xây dựng OpenCode
   ├── Chạy kiểm thử xác minh
   ├── Tạo Git commit
   └── Push tag
4. GitHub Actions tự động phát hành khi phát hiện tag
```