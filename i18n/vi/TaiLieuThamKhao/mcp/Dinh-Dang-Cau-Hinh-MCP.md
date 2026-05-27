# Định dạng Cấu hình MCP

## Tổng quan

MCP (Model Context Protocol) servers được cấu hình qua phần `mcpServers` trong file `~/.claude.json`. Claude Code hỗ trợ hai loại MCP servers:

- **Process mode (process)**: Servers được khởi động qua lệnh local
- **HTTP mode (http)**: Remote servers được truy cập qua HTTP/HTTPS

## Cấu trúc cấu hình JSON

```json
{
  "mcpServers": {
    "server-name": {
      "type": "http",           // Tùy chọn, mặc định là process
      "command": "npx",        // Bắt buộc cho process mode
      "url": "https://...",    // Bắt buộc cho HTTP mode
      "args": ["param1", "param2"],  // Tùy chọn
      "env": {                 // Tùy chọn, biến môi trường
        "KEY": "value"
      },
      "headers": {             // Tùy chọn, HTTP request headers
        "Header-Name": "value"
      },
      "description": "Server description"  // Tùy chọn
    }
  }
}
```

## Mô tả trường

| Trường | Loại | Bắt buộc | Mô tả |
|------|------|------|------|
| `type` | string | Không | Loại kết nối: `process` (mặc định) hoặc `http` |
| `command` | string | Khi type=process | Lệnh khởi động (ví dụ: `npx`, `uvx`, `python3`) |
| `url` | string | Khi type=http | HTTP/HTTPS URL của MCP server |
| `args` | array | Không | Mảng tham số truyền cho lệnh |
| `env` | object | Không | Các cặp key-value biến môi trường |
| `headers` | object | Không | HTTP request headers (chỉ HTTP mode) |
| `description` | string | Không | Mô tả chức năng server |

## Ví dụ cấu hình

### Ví dụ Process Mode

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "Thao tác GitHub - PRs, issues, repos"
    }
  }
}
```

### Ví dụ HTTP Mode

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Deployment và project Vercel"
    }
  }
}
```

### HTTP Mode với Authentication Header

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI browser agent"
    }
  }
}
```

## Best Practices

1. **Quản lý biến môi trường**: Thông tin nhạy cảm (API keys, tokens) phải sử dụng biến môi trường, không hardcode
2. **Context window**: Khuyến nghị không bật quá 10 MCP servers để giữ context window
3. **Vô hiệu hóa server**: Sử dụng biến môi trường `ECC_DISABLED_MCPS=server1,server2` để vô hiệu hóa MCP servers đi kèm

## Khắc phục sự cố

- Đảm bảo lệnh có thể thực thi (npx, uvx, v.v. đã được cài đặt)
- Kiểm tra biến môi trường đã được đặt đúng chưa
- Xem Claude Code logs để tìm server connection errors