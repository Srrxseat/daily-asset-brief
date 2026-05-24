# Daily Asset Brief

Claude Remote Routine สำหรับวิจัยข่าว + วิเคราะห์ผลกระทบต่อราคาสินทรัพย์ (ทอง/เงิน · น้ำมัน · Bitcoin) ด้วย 4 มุมมอง พร้อม commit บทความขึ้น GitHub และแจ้งเตือน LINE อัตโนมัติทุกวัน

---

## Output ที่ได้แต่ละ session

```
articles/
└── YYYY-MM-DD-brief.md    # บทความรายวัน พร้อม Action Plans
reference/
└── perspectives.md        # log การวิเคราะห์ 4 มุม (append ทุกวัน)
```

---

## Skill Flow

```
Step 1  Research      WebSearch ข่าว 24h จาก trusted sources
Step 2  Draft         บทความดิบ + แหล่งข่าว URL จริงทุกข้อ
Step 3  Analysis      วิเคราะห์ 4 มุม: Institutional / Whale / Swing / Day
Step 4  Rewrite       รวม 4 มุมเป็นเนื้อหาลื่น + Action Plans table
Step 5a Commit        GitHub MCP → articles/YYYY-MM-DD-brief.md (branch: main)
Step 5b LINE Notify   WebFetch POST → LINE push message + permalink
Step 6  Summary Log   สรุปผลการทำงาน
```

---

## Running in Claude Web Routine

### 1. Prerequisites

| สิ่งที่ต้องมี | วิธีเตรียม |
|--------------|-----------|
| GitHub account + repo | สร้าง repo ชื่อ `daily-asset-brief` (public หรือ private) |
| GitHub MCP connector | ดู [ขั้นตอนด้านล่าง](#setup-github-connector) |
| LINE Messaging API | สร้าง Messaging API channel ที่ [LINE Developers](https://developers.line.biz/) |

### 2. Setup GitHub Connector

1. เปิด [claude.ai](https://claude.ai) → **Settings** → **Connectors**
2. เลือก **GitHub** → **Connect**
3. Authorize และเลือก repository `daily-asset-brief` ให้ connector เข้าถึงได้
4. หมายเหตุ: connector ต้องมีสิทธิ์ **read + write contents** บน repo นี้

### 3. Environment Variables

กำหนดใน Claude Web Routine → **Environment Variables**:

| Variable | ค่า | หมายเหตุ |
|----------|-----|---------|
| `GITHUB_OWNER` | `your-github-username` | username หรือ org |
| `GITHUB_REPO` | `daily-asset-brief` | ชื่อ repo |
| `LINE_CHANNEL_ACCESS_TOKEN` | `(token จาก LINE Developers Console)` | optional |
| `LINE_TO` | `(User ID หรือ Group ID)` | optional — ถ้าไม่มีจะ skip LINE |

> ถ้าไม่ต้องการ LINE notification: ไม่ต้องกำหนด `LINE_CHANNEL_ACCESS_TOKEN` และ `LINE_TO` — skill จะ skip step นั้นโดยอัตโนมัติ

### 4. สร้าง Routine

1. ไปที่ [claude.ai](https://claude.ai) → **Routines** (หรือ **Scheduled Tasks**)
2. กด **New Routine**
3. กำหนดค่า:
   ```
   Name:     Daily Asset Brief
   Skill:    daily-asset-brief
   Schedule: 0 6 * * *   (ทุกวัน 06:00 Asia/Bangkok = 23:00 UTC)
   ```
4. เพิ่ม Environment Variables ตามข้อ 3
5. กด **Save & Enable**

### 5. ทดสอบ Manual Run

กด **Run now** ใน Routine dashboard เพื่อทดสอบก่อน schedule จริง  
ตรวจสอบผลลัพธ์ที่:
- `articles/` ใน GitHub repo
- LINE (ถ้ากำหนด token ไว้)
- Log ใน Routine execution history

---

## Cron Schedule แนะนำ

| เวลา | Cron (UTC) | เหมาะกับ |
|------|-----------|---------|
| 06:00 ไทย | `0 23 * * *` | ก่อนตลาด Asia เปิด |
| 08:00 ไทย | `0 1 * * *` | หลังตลาด Asia เปิด 1 ชั่วโมง |
| 14:00 ไทย | `0 7 * * *` | ก่อน London Open |

---

## Guardrails

- **ห้ามใช้ Bash/shell/git CLI** ทุกกรณี — commit ผ่าน GitHub MCP เท่านั้น
- ทุก URL ในบทความต้องมาจาก WebSearch จริง ห้าม hallucinate
- GitHub connector ไม่พร้อม → abort ทั้ง session
- LINE env ไม่ครบ → skip LINE, commit ยังดำเนินต่อ
- LINE API error → log เท่านั้น ห้าม retry

---

## Project Structure

```
daily-asset-brief/
├── skill.md                    # Skill definition (Claude Remote Routine)
├── README.md                   # ไฟล์นี้
├── .env.example                # Template environment variables
├── .gitignore
├── reference/
│   ├── sources.md              # Trusted news sources list
│   └── perspectives.md         # 4-perspective framework + analysis log
└── articles/
    ├── .gitkeep
    └── YYYY-MM-DD-brief.md     # บทความรายวัน (สร้างโดย skill)
```

---

## LINE Webhook Setup (Optional)

ถ้าต้องการ LINE notification:

1. ไปที่ [LINE Developers Console](https://developers.line.biz/console/)
2. สร้าง **Messaging API** channel
3. คัดลอก **Channel access token (long-lived)**
4. หา **User ID** ของตัวเอง: เพิ่ม bot เป็นเพื่อน แล้วดูใน webhook event หรือ LINE OA Manager
5. กำหนด `LINE_CHANNEL_ACCESS_TOKEN` และ `LINE_TO` ใน Routine env vars

---

*Disclaimer: บทความที่สร้างโดย skill นี้เป็นข้อมูลเพื่อการศึกษาเท่านั้น ไม่ใช่คำแนะนำการลงทุน*
