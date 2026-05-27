# รูปแบบการกำหนดค่า MCP

## ภาพรวม

เซิร์ฟเวอร์ MCP (Model Context Protocol) ตั้งค่าผ่านส่วน `mcpServers` ในไฟล์ `~/.claude.json` Claude Code รองรับสองประเภทเซิร์ฟเวอร์ MCP:

- **โหมดกระบวนการ (process)**: เซิร์ฟเวอร์ที่เปิดตัวผ่านคำสั่งในพื้นที่
- **โหมด HTTP (http)**: เซิร์ฟเวอร์ระยะไกลที่เข้าถึงผ่าน HTTP/HTTPS

## โครงสร้างการกำหนดค่า JSON

```json
{
  "mcpServers": {
    "ชื่อเซิร์ฟเวอร์": {
      "type": "http",           // ไม่บังคับ, ค่าเริ่มต้นคือ process
      "command": "npx",        // ต้องระบุเมื่อ type=process
      "url": "https://...",    // ต้องระบุเมื่อ type=http
      "args": ["พารามิเตอร์1", "พารามิเตอร์2"],  // ไม่บังคับ
      "env": {                 // ไม่บังคับ, ตัวแปรสิ่งแวดล้อม
        "KEY": "value"
      },
      "headers": {             // ไม่บังคับ, HTTP request headers
        "Header-Name": "value"
      },
      "description": "คำอธิบายเซิร์ฟเวอร์"  // ไม่บังคับ
    }
  }
}
```

## คำอธิบายฟิลด์

| ฟิลด์ | ประเภท | บังคับ | คำอธิบาย |
|------|------|------|------|
| `type` | string | ไม่ | ประเภทการเชื่อมต่อ: `process` (ค่าเริ่มต้น) หรือ `http` |
| `command` | string | เมื่อ type=process | คำสั่งเปิดตัว (เช่น `npx`, `uvx`, `python3`) |
| `url` | string | เมื่อ type=http | URL ของเซิร์ฟเวอร์ MCP HTTP/HTTPS |
| `args` | array | ไม่ | อาร์เรย์ของพารามิเตอร์ที่ส่งไปยังคำสั่ง |
| `env` | object | ไม่ | คู่ค่าตัวแปรสิ่งแวดล้อม |
| `headers` | object | ไม่ | HTTP request headers (เฉพาะโหมด http) |
| `description` | string | ไม่ | คำอธิบายฟังก์ชันของเซิร์ฟเวอร์ |

## ตัวอย่างการตั้งค่า

### ตัวอย่างโหมดกระบวนการ

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "การดำเนินการ GitHub - PRs, issues, repos"
    }
  }
}
```

### ตัวอย่างโหมด HTTP

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "การ Deploy และโครงการ Vercel"
    }
  }
}
```

### โหมด HTTP พร้อม Authentication Header

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

## แนวปฏิบัติที่ดีที่สุด

1. **การจัดการตัวแปรสิ่งแวดล้อม**: ข้อมูลที่ละเอียดอ่อน (API keys, tokens) ต้องใช้ตัวแปรสิ่งแวดล้อม ไม่ hardcode
2. **บริบทหน้าต่าง**: แนะนำให้เปิดใช้งานเซิร์ฟเวอร์ MCP ไม่เกิน 10 ตัว เพื่อรักษาบริบทหน้าต่าง
3. **การปิดใช้งานเซิร์ฟเวอร์**: ใช้ตัวแปรสิ่งแวดล้อม `ECC_DISABLED_MCPS=server1,server2` เพื่อปิดใช้งานเซิร์ฟเวอร์ MCP ที่รวมอยู่

## การแก้ไขปัญหา

- ตรวจสอบว่าคำสั่งสามารถรันได้ (npx, uvx ฯลฯ ติดตั้งแล้ว)
- ตรวจสอบว่าตัวแปรสิ่งแวดล้อมตั้งค่าถูกต้อง
- ดูข้อผิดพลาดการเชื่อมต่อเซิร์ฟเวอร์ใน Claude Code logs