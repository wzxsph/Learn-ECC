# Lệnh Tái Cấu Trúc và Dọn Dẹp

## Tổng quan

Lệnh tái cấu trúc và dọn dẹp dùng để refactor code, xóa dead code và tự động update.

## Danh sách lệnh

### /refactor-clean

**Mục đích**: Xóa dead code + merge duplicate

**Mô tả**: Identify và delete unused code, merge duplicate code fragment.

**Analysis nội dung**:
- Unused function và variable
- Duplicate code block
- Obsolete comment
- Unused import

**Tool support**:
- `knip` - Dead code detection
- `depcheck` - Unused dependency detection
- `ts-prune` - TypeScript dead code detection

---

### /auto-update

**Mục đích**: Auto update capability

**Mô tả**: Automatically update project dependency và configuration lên version mới nhất.

**Update nội dung**:
- npm/pip package update
- Configuration file migration
- Code migration
- Version compatibility check

---

## Các lệnh liên quan

- `/refactor-clean` - Refactor cleanup
- `/auto-update` - Auto update