# Daily Asset Brief — Claude Code Context

## Project Purpose
Remote Routine skill ที่รันทุกวัน วิจัยข่าว 24h → วิเคราะห์ผลกระทบสินทรัพย์ (ทอง/เงิน/น้ำมัน/Bitcoin) จาก 4 มุมมอง → commit บทความขึ้น GitHub → แจ้งเตือน LINE

## Key Files
- `skill.md` — skill instructions ที่ Claude Remote Routine รัน (อย่าแก้ลำดับ step)
- `reference/sources.md` — trusted sources list (แก้ได้)
- `reference/perspectives.md` — framework + log (skill จะ append วันละครั้ง)
- `articles/` — บทความที่ skill สร้าง (ไม่ต้องแก้มือ)

## Constraints
- ห้ามใช้ git CLI — ใช้ GitHub MCP connector เท่านั้นสำหรับ commit/push
- ทุก URL ในบทความต้องมาจาก WebSearch จริง
- GitHub MCP connector เท่านั้นสำหรับ commit
- LINE ผ่าน Bash (curl POST) — WebFetch ไม่รองรับ POST + custom headers

## Environment Variables Required
```
GITHUB_OWNER, GITHUB_REPO, LINE_CHANNEL_ACCESS_TOKEN (75lbJVwQmqzFSmCBSk/k9xOU/YlnHJdbpO0ezWGrydvStIok5macIXG8ErCocwOs0SelZRYPe21joWMLXrTLhFObff3rEgWSjGZSvQKKSLH18Geg7TAhKHxRYSpcWulRj7yKQjCM4LbJamqv1T3lIwdB04t89/1O/w1cDnyilFU=), LINE_TO (U42f9f3c6d398f9d4052171d046fff754)
```
