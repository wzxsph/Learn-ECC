# 01-Phát triển MCP

Tìm hiểu về phát triển扩Mở rộng giao thức Model Context Protocol (MCP).

## MCP là gì?

MCP (Model Context Protocol) là một giao thức cho phép Claude tích hợp với các công cụ và dịch vụ bên ngoài. Thông qua MCP, bạn có thể mở rộng khả năng của Claude, kết nối cơ sở dữ liệu, API, các mô hình AI khác, v.v.

> Tham khảo: [Định dạng cấu hình MCP](../TaiLieuThamKhao/mcp/MCPDinh-Dang-Cau-Hinh.md)

## Mục tiêu học tập

- Hiểu kiến trúc và nguyên lý của MCP
- Tạo MCP server
- Triển khai công cụ MCP
- Kiểm tra và gỡ lỗi扩Mở rộng MCP

## Các khái niệm cốt lõi

### 1. MCP Server
MCP server là tiến trình cung cấp công cụ và tài nguyên.

### 2. Công cụ (Tools)
Các thao tác có thể được Claude gọi để thực thi.

### 3. Tài nguyên (Resources)
Các nguồn dữ liệu mà Claude có thể đọc.

### 4. Prompt (Prompts)
Các mẫu prompt được định nghĩa trước.

## Cấu hình MCP

Tệp cấu hình MCP nằm tại `.claude/mcp.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["./mcp-server.js"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

## Bước tiếp theo

- [Đại lý tự trị](./02-AgentTuChu.md) - Xây dựng Agent tự trị
- [Quay lại mục lục chuyên gia](../README.md)