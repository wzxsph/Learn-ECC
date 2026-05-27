# ประเภทของ Hook

## ภาพรวม

ระบบ Hooks ของ ECC เป็นกลไกอัตโนมัติที่ขับเคลื่อนด้วยเหตุการณ์ ทำงานก่อนและหลังการทำงานของเครื่องมือ Claude Code เพื่อบังคับใช้คุณภาพโค้ด ตรวจจับข้อผิดพลาดตั้งแต่เนิ่นๆ และทำให้การตรวจสอบซ้ำๆ เป็นไปโดยอัตโนมัติ

```
คำขอของผู้ใช้ → Claude เลือกเครื่องมือ → PreToolUse Hooks ทำงาน → เครื่องมือทำงาน → PostToolUse Hooks ทำงาน
```

## ประเภท Hook โดยละเอียด

### 1. PreToolUse Hooks

#### คำอธิบายประเภท
PreToolUse Hooks ทำงานก่อนการทำงานของเครื่องมือ และสามารถบล็อก (exit code 2) หรือเตือน (stderr output แต่ไม่บล็อก)

#### จังหวะที่ทำงาน
- ก่อนการทำงานของเครื่องมือใดๆ (Edit, Write, Bash, Read ฯลฯ)
- ใช้กับ Pre-Checks สำหรับการทำงานของเครื่องมือทั้งหมด

#### พารามิเตอร์ที่ส่งไป
```typescript
interface HookInput {
  tool_name: string;          // ชื่อเครื่องมือ: "Bash", "Edit", "Write", "Read" ฯลฯ
  tool_input: {
    command?: string;         // Bash: คำสั่งที่จะรัน
    file_path?: string;        // Edit/Write/Read: เส้นทางไฟล์เป้าหมาย
    old_string?: string;       // Edit: ข้อความที่ถูกแทนที่
    new_string?: string;       // Edit: ข้อความแทนที่
    content?: string;          // Write: เนื้อหาไฟล์
  };
}
```

#### Exit Codes
| Exit Code | ความหมาย | พฤติกรรม |
|--------|------|------|
| 0 | สำเร็จ | ดำเนินการต่อ ไม่มีการเตือน |
| 2 | บล็อก | บล็อกการทำงานของเครื่องมือ |
| อื่นๆ ที่ไม่ใช่ null | ข้อผิดพลาด | Log แต่ไม่บล็อก |

#### ตัวอย่างการใช้งาน

**Dev-Server-Blocker (ป้องกันการรัน npm run dev นอก tmux):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "ป้องกันการรันเซิร์ฟเวอร์พัฒนานอก tmux"
}
```

**คำเตือนไฟล์เอกสาร (เตือนไฟล์ .md ที่ไม่มาตรฐาน):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "เตือนเมื่อสร้างไฟล์เอกสารที่ไม่มาตรฐาน"
}
```

#### หมายเหตุ
- PreToolUse เป็น hook ที่ **บล็อกได้** ต้องรันเร็ว (<200ms)
- ไม่ควรทำการเรียกเครือข่ายใน PreToolUse hooks
- ใช้การบล็อก (exit 2) อย่างประหยัด เฉพาะสำหรับการตรวจสอบความปลอดภัยที่สำคัญเท่านั้น

---

### 2. PostToolUse Hooks

#### คำอธิบายประเภท
PostToolUse Hooks ทำงานหลังการทำงานของเครื่องมือ สามารถวิเคราะห์ output แต่ไม่สามารถบล็อกการทำงานของเครื่องมือได้

#### จังหวะที่ทำงาน
- หลังจากเครื่องมือใดๆ เสร็จสิ้น
- สามารถเข้าถึงผลลัพธ์ของเครื่องมือได้

#### พารามิเตอร์ที่ส่งไป
```typescript
interface HookInput {
  tool_name: string;          // ชื่อเครื่องมือ
  tool_input: {               // เหมือนกับ PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // มีเฉพาะใน PostToolUse
    output?: string;          // ผลลัพธ์คำสั่ง/เครื่องมือ
  };
}
```

#### Exit Codes
| Exit Code | ความหมาย |
|--------|------|
| 0 | สำเร็จ ดำเนินการต่อ |
| ไม่ใช่ null | ข้อผิดพลาด log แต่ไม่บล็อกการทำงานของเครื่องมือ |

#### ตัวอย่างการใช้งาน

**PR Logging (บันทึก URL หลัง gh pr create):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "บันทึก URL และคำสั่ง review หลังสร้าง PR"
}
```

**การตรวจสอบ Quality Gate (รันการตรวจสอบคุณภาพหลังแก้ไข):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "รันการตรวจสอบ quality gate หลังแก้ไขไฟล์"
}
```

#### หมายเหตุ
- PostToolUse Hooks ไม่สามารถบล็อกการทำงานของเครื่องมือได้
- สามารถตั้งค่าเป็น async (async: true) เพื่อไม่บล็อก
- Hooks แบบ async ควรเสร็จภายใน 30 วินาที

---

### 3. Stop Hooks

#### คำอธิบายประเภท
Stop Hooks ทำงานหลังจาก Claude ตอบทุกครั้ง สำหรับการประมวลผลแบบ batch การตรวจสอบขั้นสุดท้าย และการบันทึกสถานะเซสชัน

#### จังหวะที่ทำงาน
- หลังเสร็จสิ้นทุกคำถามของผู้ใช้และการสร้างคำตอบของ Claude
- ทำงานหลังคำตอบสุดท้ายของเซสชัน

#### พารามิเตอร์ที่ส่งไป
Stop Hooks รับ input เดียวกับ PostToolUse รวมถึงประวัติการเรียกเครื่องมือทั้งหมด

#### ตัวอย่างการใช้งาน

**การตรวจสอบ Console Log (ตรวจสอบ console.log ในไฟล์ที่แก้ไขหลังคำตอบ):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "ตรวจสอบว่ามี console.log ในไฟล์ที่แก้ไขหรือไม่"
}
```

**การจัดรูปแบบและตรวจสอบประเภทแบบ batch:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "จัดรูปแบบ batch ไฟล์ JS/TS ที่แก้ไขทั้งหมดและตรวจสอบประเภท"
}
```

**การบันทึกสรุปเซสชัน:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "บันทึกสถานะเซสชัน"
}
```

#### หมายเหตุ
- Stop Hooks รันหลังทุกคำตอบ เหมาะสำหรับการทำงานแบบ batch
- สามารถใช้โหมด async สำหรับการทำงานที่ไม่บล็อก
- การจัดรูปแบบ/ตรวจสอบประเภทเหมาะสำหรับการรันแบบ batch ตอน stop

---

### 4. SessionStart Hooks

#### คำอธิบายประเภท
SessionStart Hooks ทำงานเมื่อเริ่มวงจรชีวิตของเซสชัน สำหรับการโหลดบริบทก่อนหน้าและการตรวจจับสถานะโครงการ

#### จังหวะที่ทำงาน
- เมื่อเซสชันใหม่เริ่มต้น
- เมื่อ Claude Code เริ่มต้นหรือสร้างเซสชันใหม่

#### ตัวอย่างการใช้งาน

**โหลดบริบทก่อนหน้าและตรวจจับตัวจัดการแพ็คเกจ:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "โหลดบริบทก่อนหน้าที่จำกัดและตรวจจับสถานะโครงการเมื่อเริ่มเซสชัน",
  "blocking": false
}
```

#### หมายเหตุ
- Lifecycle hooks ต้องเสร็จสิ้นเร็ว (<30 วินาที)
- สามารถควบคุมขนาดบริบทเพิ่มเติมผ่านตัวแปรสภาพแวดล้อม `ECC_SESSION_START_MAX_CHARS`
- สามารถปิดใช้งานได้อย่างสมบูรณ์ด้วย `ECC_SESSION_START_CONTEXT=off`

---

### 5. SessionEnd Hooks

#### คำอธิบายประเภท
SessionEnd Hooks ทำงานเมื่อเซสชันสิ้นสุด สำหรับการบันทึกสรุปสิ้นสุดเซสชันและการทำความสะอาด

#### จังหวะที่ทำงาน
- เมื่อเซสชันสิ้นสุด
- เมื่อ metadata ของ transcript เซสชันพร้อมใช้งาน

#### ตัวอย่างการใช้งาน

**เครื่องหมายสิ้นสุดเซสชัน:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "บันทึกสรุปสิ้นสุดเซสชันเมื่อ metadata ของ transcript พร้อมใช้งาน",
  "blocking": false
}
```

#### หมายเหตุ
- ส่วนใหญ่รันแบบ async ไม่ควรบล็อกสิ้นสุดเซสชัน
- เครื่องหมายวงจรชีวิตสำหรับการเก็บรวบรวมตัวชี้วัดและการทำความสะอาด

---

### 6. PreCompact Hooks

#### คำอธิบายประเภท
PreCompact Hooks ทำงานก่อนการบีบอัดบริบท สำหรับการบันทึกสถานะ

#### จังหวะที่ทำงาน
- ก่อนการบีบอัด/ลดขนาดบริบท
- เมื่อหน้าต่างบริบทเกือบเต็ม

#### ตัวอย่างการใช้งาน

**บันทึกสถานะก่อนบีบอัดบริบท:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "บันทึกสถานะเซสชันก่อนบีบอัดบริบท",
  "blocking": false
}
```

#### หมายเหตุ
- เหมาะสำหรับบันทึกสถานะของงานที่ใช้เวลานาน
- Non-blocking ไม่มีผลต่อกระบวนการบีบอัด

---

### 7. PostToolUseFailure Hooks

#### คำอธิบายประเภท
PostToolUseFailure Hooks ทำงานหลังข้อผิดพลาดการทำงานของเครื่องมือ

#### จังหวะที่ทำงาน
- หลังการเรียกเครื่องมือใดๆ ที่ล้มเหลว

#### ตัวอย่างการใช้งาน

**การตรวจสอบสุขภาพ MCP (ติดตามการเรียก MCP ที่ล้มเหลว):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "ติดตามการเรียกเครื่องมือ MCP ที่ล้มเหลว ทำเครื่องหมายเซิร์ฟเวอร์ที่ไม่สุขภาพดี และพยายามเชื่อมต่อใหม่"
}
```

#### หมายเหตุ
- สามารถใช้สำหรับตรรกะการลองใหม่หรือการติดตามสถานะสุขภาพ
- ไม่เปลี่ยนสถานะของข้อผิดพลาดเครื่องมือ

---

## ตารางเปรียบเทียบประเภท Hook

| ประเภท | จังหวะที่ทำงาน | สามารถบล็อกได้ | การใช้งานทั่วไป |
|------|----------|----------|----------|
| PreToolUse | ก่อนทำงานเครื่องมือ | ใช่ (exit 2) | การตรวจสอบคุณภาพ การตรวจสอบความปลอดภัย |
| PostToolUse | หลังทำงานเครื่องมือ | ไม่ | Logging การวิเคราะห์ |
| Stop | หลังทุกคำตอบ | ไม่ | การจัดรูปแบบ batch สรุปเซสชัน |
| SessionStart | เริ่มเซสชัน | ไม่ | โหลดบริบท จดจำโครงการ |
| SessionEnd | สิ้นสุดเซสชัน | ไม่ | บันทึกสรุป ทำความสะอาด |
| PreCompact | ก่อนบีบอัด | ไม่ | บันทึกสถานะ |
| PostToolUseFailure | หลังข้อผิดพลาดเครื่องมือ | ไม่ | การตรวจสอบสุขภาพ ลองใหม่ |

---

## การควบคุมด้วยตัวแปรสภาพแวดล้อม

พฤติกรรมของ Hook สามารถควบคุมได้ผ่านตัวแปรสภาพแวดล้อม:

```bash
# minimal | standard | strict (ค่าเริ่มต้น: standard)
export ECC_HOOK_PROFILE=standard

# ปิดใช้งาน Hook ID เฉพาะ (คั่นด้วย comma)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# ปิดใช้งาน GateGuard
export ECC_GATEGUARD=off

# ควบคุมขนาดบริบทเพิ่มเติมของ SessionStart (ค่าเริ่มต้น: 8000 ตัวอักษร)
export ECC_SESSION_START_MAX_CHARS=4000

# ปิดใช้งานบริบทเพิ่มเติมของ SessionStart อย่างสมบูรณ์
export ECC_SESSION_START_CONTEXT=off
```