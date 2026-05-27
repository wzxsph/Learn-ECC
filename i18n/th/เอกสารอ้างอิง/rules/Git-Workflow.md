# Git Workflow

## รูปแบบข้อความ Commit

```
<type>: <description>

<optional body>
```

**ประเภท (type)**:
- `feat`: ฟีเจอร์ใหม่
- `fix`: แก้ไข bug
- `refactor`: ปรับโครงสร้าง
- `docs`: เอกสาร
- `test`: การทดสอบ
- `chore`: งานเบ็ดเตล็ด
- `perf`: การเพิ่มประสิทธิภาพ
- `ci`: CI/CD

> หมายเหตุ: attribution ถูกปิดใช้งานทั่วโลกผ่าน `~/.claude/settings.json`

## Pull Request Workflow

เมื่อสร้าง PR:
1. วิเคราะห์ประวัติ commit ทั้งหมด (ไม่ใช่เฉพาะ commit ล่าสุด)
2. ใช้ `git diff [base-branch]...HEAD` เพื่อดูการเปลี่ยนแปลงทั้งหมด
3. ร่าง PR summary ที่ครอบคลุม
4. รวม TODOs ของแผนการทดสอบ
5. ถ้าเป็น branch ใหม่ ใช้ flag `-u` เมื่อ push

## การตั้งชื่อ Branch

ใช้มาตรฐาน git flow branch naming:
- `feature/`: ฟีเจอร์ใหม่
- `fix/`: การแก้ไข
- `refactor/`: การปรับโครงสร้าง
- `docs/`: เอกสาร

## ขั้นตอนการพัฒนา

ขั้นตอนการพัฒนาที่สมบูรณ์:
1. **วิจัย & นำกลับมาใช้ใหม่** - GitHub search, เอกสารไลบรารี, ทะเบียนแพ็คเกจ
2. **วางแผน** - ใช้ตัวแทน planner เพื่อสร้างแผนการดำเนินการ
3. **วิธี TDD** - เขียนการทดสอบก่อน แล้วจึง implementation
4. **การตรวจสอบโค้ด** - ใช้ตัวแทน code-reviewer
5. **Commit & Push** - ข้อความ commit โดยละเอียด, tuân thủ conventional commits