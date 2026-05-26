# Learn-ECC

> Lộ trình học tập có cấu trúc để làm chủ ECC (Everything Claude Code)

Chào mừng đến với Learn-ECC! Đây là lộ trình học tập nâng cao về Claude Code một cách có hệ thống, giúp bạn từ người mới bắt đầu dần trở thành người sử dụng thành thạo.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./en/README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./de/README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./tr/README.md) | [🇧🇷 Português](./pt-BR/README.md) | [🇻🇳 Tiếng Việt](./README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |

---

## Câu chuyện nền

### Tại sao có dự án này?

Tôi không giỏi tiếng Anh, đọc tài liệu ECC bằng tiếng Anh khá vất vả. Vì vậy, tôi đã dịch các lệnh (Commands), tác tử (Agents), kỹ năng (Skills) trong ECC sang tiếng Trung, và tổ chức thành bộ khóa học này.

Tài liệu tham khảo chính là bản dịch tiếng Trung của dự án ECC gốc - cho bạn biết mỗi lệnh, mỗi Agent dùng để làm gì.

### Khóa học này đến từ đâu?

Thành thật mà nói, khóa học này được AI tạo ra. AI đã tạo nội dung cơ bản dựa trên cấu trúc file của ECC, nhưng thiếu suy nghĩ chi tiết và đánh giá thực tế. Nếu bạn thấy nội dung có vấn đề hoặc chưa rõ ràng, hãy góp ý.

---

## Các khái niệm cốt lõi

### ECC là gì?

**ECC** (Everything Claude Code) là dự án plugin Claude Code ([repo gốc](https://github.com/affaan-m/ECC)), cung cấp:
- **75 lệnh** - lệnh gạch chéo (như `/plan`, `/code-review`) và lệnh script
- **60 Agent chuyên nghiệp** - code review, sửa lỗi build, thiết kế kiến trúc, v.v.
- **232+ Skills** - mẫu workflow có thể tái sử dụng
- **Hệ thống Hooks** - cơ chế tự động hóa dựa trên sự kiện
- **Quy tắc Rules** - quy tắc phát triển bắt buộc

### Learn-ECC là gì?

**Learn-ECC** là lộ trình học tập có hệ thống mà tôi tổ chức để học ECC, không phải là chính ECC.

Nó sử dụng triết lý "học qua làm", mỗi giai đoạn đều bao gồm học lý thuyết và thực hành. Bằng cách hoàn thành các dự án thực tế, bạn sẽ nắm vững các khả năng cốt lõi của Claude Code.

### Tài liệu tham khảo là gì?

**Tài liệu tham khảo** (./TaiLieuThamKhao/) là bản dịch tiếng Trung của dự án ECC gốc. Tôi đã dịch tài liệu tiếng Anh của ECC sang tiếng Trung để tiện tra cứu cho mình (và bạn).

Nội dung bao gồm:
- Mô tả đầy đủ các lệnh (mỗi lệnh dùng để làm gì)
- Giới thiệu chi tiết tất cả Agent
- Cấu trúc thư mục Skills và quy tắc viết
- Tài liệu đầy đủ về hệ thống Hooks
- Best practices và design patterns

---

## Tổng quan lộ trình học tập

| Giai đoạn | Nội dung | Thời gian ước tính | Độ khó |
|------|------|----------|------|
| [1-NhapMon](./1-NhapMon/README.md) | Giới thiệu ECC commands, Agent, Skills + Cài đặt | 2-3 giờ | Nhập môn |
| [2-KienThucCoBan](./2-KienThucCoBan/README.md) | Hệ thống lệnh, Agent协作, Hooks定制, Skills开发, Rules编写 | 8-12 giờ | Nâng cao |
| [3-DuongTroiChuyenGia](./3-DuongTroiChuyenGia/README.md) | MCP开发, 自主代理, 高级模式 | 6-8 giờ | Chuyên gia |
| [4-DuAnThucHanh](./4-DuAnThucHanh/README.md) | CLI工具, 代码审查, 自动化, Agent系统 | 10-15 giờ | Thực chiến |
| [5-TraCuuNhanh](./5-TraCuuNhanh/README.md) | Bảng tra cứu lệnh, Agent, workflow | Tra cứu mọi lúc | Tham khảo |

---

## Điều hướng nhanh

### Giai đoạn 1: Nhập môn
- [Giới thiệu khóa học](./1-NhapMon/01-GioiThieuECC.md) - ECC là gì, Learn-ECC có thể làm gì
- [Cài đặt](./1-NhapMon/02-CaiDat.md) - Cấu hình nhanh
- [Lệnh cơ bản](./1-NhapMon/03-LenhCoBan.md) - Tổng quan các lệnh thường dùng
- [Nhiệm vụ đầu tiên](./1-NhapMon/04-NhiemVuDauTien.md) - Hoàn thành nhiệm vụ đầu tiên
- [Bài tập nhập môn](./1-NhapMon/exercises/BaiTapNhapMon.md) - Củng cố kiến thức cơ bản

### Giai đoạn 2: Khả năng cốt lõi
- [Làm chủ hệ thống lệnh](./2-KienThucCoBan/01-HeThongLenh/README.md) - Nắm vững tất cả loại lệnh
- [Agent协作](./2-KienThucCoBan/02-HopTacAgent/README.md) - Thiết kế hệ thống multi-agent
- [Hooks定制](./2-KienThucCoBan/03-TuyChinhHooks/README.md) - Tự động hóa workflow
- [Skills开发](./2-KienThucCoBan/04-PhatTrienSkills/README.md) - Xây dựng kỹ năng tái sử dụng
- [Rules编写](./2-KienThucCoBan/05-VietRules/README.md) - Tùy chỉnh quy tắc phát triển

### Giai đoạn 3: Con đường chuyên gia
- [MCP开发](./3-DuongTroiChuyenGia/01-PhatTrienMCP.md) - Model Context Protocol
- [自主代理](./3-DuongTroiChuyenGia/02-AgentTuChu.md) - Xây dựng Agent tự trị
- [高级模式](./3-DuongTroiChuyenGia/03-MauNangCao.md) - Design patterns và best practices

### Giai đoạn 4: Dự án thực chiến
- [Dự án 1: CLI工具](./4-DuAnThucHanh/project-1-cli.md) - Xây dựng công cụ dòng lệnh
- [Dự án 2: 代码审查](./4-DuAnThucHanh/project-2-review.md) - Quy trình review tự động
- [Dự án 3: 自动化工作流](./4-DuAnThucHanh/project-3-auto.md) - Tự động hóa đầu cuối
- [Dự án 4: Agent系统](./4-DuAnThucHanh/project-4-agent.md) - Nền tảng Agent đa chức năng

### Giai đoạn 5: Tra cứu nhanh
- [Tra cứu lệnh](./5-TraCuuNhanh/cheatsheet-commands.md)
- [Tra cứu Agent](./5-TraCuuNhanh/cheatsheet-agents.md)
- [Tra cứu workflow](./5-TraCuuNhanh/cheatsheet-workflows.md)

---

## Cách sử dụng khóa học này

### Thứ tự học tập khuyến nghị

1. **Học theo từng giai đoạn** - Mỗi giai đoạn xây dựng trên nền tảng giai đoạn trước
2. **Kết hợp lý thuyết + thực hành** - Mỗi chương có bài tập đi kèm, hiểu trước rồi làm
3. **Hoàn thành tất cả bài tập** - Bài tập là phần cốt lõi của lộ trình học

### Đề xuất học tập

- **Giai đoạn nhập môn**: Hoàn thành trong 2-3 giờ, tập trung làm quen các khái niệm và thao tác cơ bản
- **Khả năng cốt lõi**: 8-12 giờ, mỗi module đều phải thực hành tay
- **Con đường chuyên gia**: 6-8 giờ, cần một số kinh nghiệm dự án
- **Dự án thực chiến**: 10-15 giờ, chọn dự án yêu thích để đi sâu

### Checkpoint học tập

Sau mỗi giai đoạn, xác nhận bạn đã nắm vững:
- Có thể hoàn thành độc lập tất cả thao tác của giai đoạn đó
- Có thể giải thích các khái niệm cốt lõi cho người khác
- Có thể áp dụng linh hoạt vào các tình huống khác

---

## Yêu cầu tiên quyết

### Kiến thức cơ bản
- Cơ bản về command line (hiểu các lệnh cơ bản)
- Cơ bản JavaScript/Node.js ( một số chức năng nâng cao cần)
- Cơ bản Git (khái niệm version control)

### Yêu cầu môi trường
- Node.js >= 18
- npm/pnpm/yarn/bun một trong các công cụ này
- Trình soạn thảo (VS Code khuyến nghị)

### Khóa học tiên quyết
Không có. Giai đoạn nhập môn được thiết kế cho người hoàn toàn mới.

---

## Sơ đồ lộ trình học

```
┌─────────────────────────────────────────────────────────────────┐
│                        Learn-ECC 学习路径                        │
└─────────────────────────────────────────────────────────────────┘

    Giai đoạn 1: Nhập môn
        │
        ↓
    ┌───────────────────────┐
    │  Giai đoạn 2: Khả năng cốt lõi │←───────────────────┐
    │  (lệnh/Agent/Hooks/    │                    │
    │   Skills/Rules)       │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │  Giai đoạn 3: Con đường chuyên gia │                    │
    │  (MCP/自主代理/高级模式) │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │  Giai đoạn 4: Dự án thực chiến │────────────────────┘
    │  (CLI/审查/自动化/Agent) │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │  Giai đoạn 5: Tra cứu nhanh │ ← Tra cứu bất kỳ lúc nào
    │  (bảng tra cứu lệnh/Agent)    │
    └───────────────────────┘
```

**Tổng thời gian học ước tính**: 26-38 giờ (khoảng 2-3 tuần)

---

## Cấu trúc thư mục

```
Learn-ECC/
├── 1-NhapMon/                         # Giai đoạn 1: Nhập môn (2-3 giờ)
│   ├── 01-GioiThieuECC.md               # Tổng quan ECC, giá trị cốt lõi, kiến trúc
│   ├── 02-CaiDat.md              # Chuẩn bị môi trường, cài đặt, xác minh
│   ├── 03-LenhCoBan.md              # /plan, /code-review, /build-fix
│   ├── 04-NhiemVuDauTien.md           # Ví dụ quy trình phát triển đầy đủ
│   └── exercises/
│       └── BaiTapNhapMon.md             # 4 bài tập nhập môn
│
├── 2-KienThucCoBan/                     # Giai đoạn 2: Khả năng cốt lõi (8-12 giờ)
│   ├── 01-HeThongLenh/
│   ├── 02-HopTacAgent/
│   ├── 03-TuyChinhHooks/
│   ├── 04-PhatTrienSkills/
│   └── 05-VietRules/
│
├── 3-DuongTroiChuyenGia/                     # Giai đoạn 3: Con đường chuyên gia (6-8 giờ)
│   ├── 01-PhatTrienMCP.md
│   ├── 02-AgentTuChu.md
│   └── 03-MauNangCao.md
│
├── 4-DuAnThucHanh/                     # Giai đoạn 4: Dự án thực chiến (10-15 giờ)
│   ├── project-1-cli.md           # Phát triển công cụ CLI
│   ├── project-2-review.md        # Pipeline review code tự động
│   ├── project-3-auto.md           # Tự động hóa workflow
│   └── project-4-agent.md         # Phát triển Agent tùy chỉnh
│
├── 5-TraCuuNhanh/                     # Giai đoạn 5: Tra cứu nhanh
│   ├── cheatsheet-commands.md     # Bảng tra cứu lệnh
│   ├── cheatsheet-agents.md       # Bảng tra cứu Agent
│   └── cheatsheet-workflows.md    # Bảng tra cứu workflow
│
├── assets/                         # Tài nguyên hình ảnh
├── progress-tracker.md            # Bảng theo dõi tiến độ học tập
├── contributing.md                 # Hướng dẫn đóng góp
└── README.md                       # File này
```

---

## Tips thành công

1. **Từ từ nhưng chắc**: Đừng nhảy cóc giai đoạn, mỗi Stage là nền tảng cho Stage tiếp theo
2. **Học qua làm**: Mỗi khi học một khái niệm, lập tức thực hành, đừng chỉ xem而不 làm
3. **Hoàn thành bài tập**: Mỗi Stage có bài tập đi kèm, hoàn thành nghiêm túc để củng cố kiến thức
4. **Dùng bảng tra cứu**: Trong sử dụng hàng ngày, hãy tận dụng bảng tra cứu, đừng học thuộc lòng
5. **Dự án dẫn lối**: Stage 4 dùng dự án thực tế để kết nối tất cả kiến thức
6. **Ghi chép**: Ghi lại cảm nhận học tập và vấn đề gặp phải
7. **Dạy người khác**: Dạy được người khác mới thực sự nắm vững

---

## Câu hỏi thường gặp (FAQ)

### Hỏi: Cần nền tảng lập trình không?

Đ: Cần kiến thức cơ bản về command line và lập trình (JavaScript/Node.js cơ bản giúp hiểu một số chức năng), nhưng không cần thành thạo bất kỳ ngôn ngữ cụ thể nào. Giai đoạn nhập môn được thiết kế cho người hoàn toàn mới.

### Hỏi: Có thể bỏ qua một số giai đoạn không?

Đ: Giai đoạn 1-3 nên học theo thứ tự, Stage 4 có thể chọn dự án yêu thích theo nhu cầu, Stage 5 tra cứu bất kỳ lúc nào.

### Hỏi: Sau khi học xong có thể làm gì?

Đ: Nắm vững workflow Claude Code production, có thể phát triển, review, test và tự động hóa code hiệu quả; có thể thiết kế và triển khai hệ thống Agent tùy chỉnh; có thể tùy chỉnh Hooks và Skills.

### Hỏi: Gặp vấn đề trong quá trình học thì làm sao?

Đ: 1) Xem [tài liệu tham khảo](./TaiLieuThamKhao/) chương liên quan; 2) Xem [tra cứu lệnh](./5-TraCuuNhanh/cheatsheet-commands.md); 3) Gửi Issue để được giúp đỡ.

### Hỏi: Làm sao để đánh giá hiệu quả học tập?

Đ: Dùng [progress-tracker.md](./progress-tracker.md) theo dõi tiến độ, mỗi giai đoạn có mục tiêu học tập rõ ràng, hoàn thành tất cả bài tập là đã nắm vững nội dung giai đoạn đó.

### Hỏi: Nội dung khóa học có cập nhật không?

Đ: Sẽ cập nhật liên tục, welcome gửi PR đóng góp nội dung. Xem hướng dẫn chi tiết tại [contributing.md](./contributing.md).

---

## Đóng góp và phản hồi

Nếu phát hiện vấn đề hoặc có đề xuất cải thiện trong quá trình học, welcome gửi Issue hoặc Pull Request.

---

*Last updated: 2026-05-26*