# Daily Asset Brief — Claude Code Context

## Project Purpose
Remote Routine skill ที่รันทุกวัน วิจัยข่าว 24h → วิเคราะห์ผลกระทบสินทรัพย์ (ทอง/เงิน/น้ำมัน/Bitcoin) จาก 4 มุมมอง → commit บทความขึ้น GitHub → แจ้งเตือน LINE

## Key Files
- `skill.md` — skill instructions ที่ Claude Remote Routine รัน (อย่าแก้ลำดับ step)
- `reference/sources.md` — trusted sources list (แก้ได้)
- `reference/perspectives.md` — framework + log (skill จะ append วันละครั้ง)
- `articles/` — บทความที่ skill สร้าง (ไม่ต้องแก้มือ)

## Constraints
- Skill ต้อง Routine-compatible: ห้ามใช้ Bash/shell/git CLI
- ทุก URL ในบทความต้องมาจาก WebSearch จริง
- GitHub MCP connector เท่านั้นสำหรับ commit
- LINE ผ่าน WebFetch (direct HTTP POST) เท่านั้น

## Environment Variables Required
```
GITHUB_OWNER, GITHUB_REPO, LINE_CHANNEL_ACCESS_TOKEN (token form LINE Developers), LINE_TO (User ID)
```
