# 02-Đại lý tự trị

Tìm hiểu cách xây dựng hệ thống đại lý có khả năng tự ra quyết định và thực thi nhiệm vụ.

## Đại lý tự trị là gì?

Đại lý tự trị (Autonomous Agent) là một hệ thống thông minh có khả năng:
- Tự lập kế hoạch các bước nhiệm vụ
- Điều chỉnh chiến lược theo phản hồi từ môi trường
- Thực thi liên tục cho đến khi đạt được mục tiêu

> Tham khảo: [Tự động hóa và kịch bản](../TaiLieuThamKhao/skills/自动化与脚本.md) | [Agent kiểm tra mã](../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md)

## Mục tiêu học tập

- Hiểu kiến trúc đại lý tự trị
- Thiết kế quy trình ra quyết định của đại lý
- Triển khai vòng thực thi của đại lý
- Xử lý ngoại lệ và khôi phục

## Các thành phần cốt lõi

### 1. Bộ lập kế hoạch (Planner)
Phân rã nhiệm vụ phức tạp thành các bước có thể thực thi.

### 2. Bộ thực thi (Executor)
Gọi công cụ để thực hiện các thao tác cụ thể.

### 3. Bộ đánh giá (Evaluator)
Xác định xem nhiệm vụ đã hoàn thành chưa, đánh giá kết quả thực thi.

### 4. Bộ nhớ (Memory)
Lưu trữ lịch sử cuộc trò chuyện và ngữ cảnh.

## Các mẫu đại lý

### Đại lý phản ứng
Phản hồi trực tiếp với các thay đổi của môi trường dựa trên trạng thái hiện tại.

### Đại lý hướng mục tiêu
Lập kế hoạch và thực thi xung quanh mục tiêu rõ ràng.

### Đại lý học tập
Cải thiện chiến lược ra quyết định thông qua kinh nghiệm.

## Bước tiếp theo

- [Mẫu nâng cao](./03-MauNangCao.md) - Các mẫu thiết kế và thực hành tốt nhất
- [Quay lại mục lục chuyên gia](../README.md)

## Tài nguyên đi kèm

- [Phát triển MCP](./01-PhatTrienMCP.md) - Nắm vững giao thức ngữ cảnh mô hình