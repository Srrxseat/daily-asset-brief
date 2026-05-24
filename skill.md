---
name: daily-asset-brief
description: วิจัยข่าว 24h → draft บทความผลกระทบสินทรัพย์ (ทอง/เงิน/น้ำมัน/Bitcoin) → วิเคราะห์ 4 มุม → rewrite + Action Plans → commit ผ่าน GitHub MCP → notify LINE
---

# Daily Asset Brief — Skill Instructions

## Runtime Context

- **Timezone**: Asia/Bangkok (UTC+7) — ใช้เวลาไทยทุกที่ในบทความและ log
- **Date**: ดึงจาก `currentDate` system context (`YYYY-MM-DD`)
- **GitHub repo**: `${GITHUB_OWNER}/${GITHUB_REPO}` (ดูจาก env)
- **Article path**: `articles/{YYYY-MM-DD}-brief.md`
- **Branch**: `main`

---

## Guardrails (บังคับ — ห้ามฝ่าฝืน)

| Rule | Action |
|------|--------|
| ห้ามใช้ Bash / shell / git CLI ทุกกรณี | ใช้ GitHub MCP connector เท่านั้น |
| ห้ามแต่งข่าว / hallucinate URL | ทุก claim ต้องมี URL จริงจาก WebSearch |
| GitHub MCP ไม่พร้อม | log `ERROR: GitHub connector unavailable — aborting` แล้ว abort |
| `LINE_CHANNEL_ACCESS_TOKEN` หรือ `LINE_TO` ไม่มี | log `SKIP: {VAR} not set` แล้ว skip LINE step (commit ยังดำเนินต่อ) |
| LINE API ตอบ non-200 | log `ERROR: LINE API {status} — {body}` ห้าม retry |

---

## Step 1 — Research (ข่าว 24 ชั่วโมงที่ผ่านมา)

1. อ่าน `reference/sources.md` เพื่อดู trusted sources list
2. ค้นข่าวด้วย **WebSearch** สำหรับแต่ละ asset — เน้นแหล่งที่อยู่ใน sources.md:

   | Asset | Query ตัวอย่าง |
   |-------|----------------|
   | ทอง/เงิน | `gold silver price news 24h site:reuters.com OR site:bloomberg.com OR site:ft.com` |
   | น้ำมัน | `crude oil WTI Brent price news today site:reuters.com OR site:bloomberg.com OR site:cnbc.com` |
   | Bitcoin | `Bitcoin BTC crypto price news today site:coindesk.com OR site:reuters.com OR site:bloomberg.com` |

3. รวบรวมข่าวอย่างน้อย **3 ข้อต่อ asset** พร้อม URL, headline, แหล่งข่าว, วันที่
4. เก็บ raw research ไว้ใน working memory สำหรับ step ถัดไป

---

## Step 2 — Draft Article (เนื้อหาดิบ)

สร้าง draft โดยใช้โครงสร้างนี้:

```markdown
# Daily Asset Brief: {TOPIC} — {YYYY-MM-DD}

> สร้างเมื่อ: {YYYY-MM-DD HH:MM} เวลาไทย

## ทองคำ & เงิน (Gold / Silver)

{อธิบายข่าวและผลกระทบต่อราคา 200-300 คำ — เป็นกลาง ไม่ชี้นำ}

**แหล่งข่าว:**
- [{headline}]({url}) — {ชื่อแหล่งข่าว}, {วันที่}

## น้ำมัน (Crude Oil — WTI / Brent)

{200-300 คำ}

**แหล่งข่าว:**
- [{headline}]({url}) — {ชื่อแหล่งข่าว}, {วันที่}

## Bitcoin (BTC)

{200-300 คำ}

**แหล่งข่าว:**
- [{headline}]({url}) — {ชื่อแหล่งข่าว}, {วันที่}
```

---

## Step 3 — 4-Perspective Analysis

วิเคราะห์แต่ละ asset จาก 4 มุมมอง โดยอิง framework ใน `reference/perspectives.md`:

### มุมที่ 1: กองทุนสถาบัน (Institutional Fund)
- ขอบฟ้าเวลา: 3–12 เดือน
- โฟกัส: macro trends, Fed/ธนาคารกลาง, geopolitics, portfolio allocation, DXY
- คำถาม: *ข่าวนี้เปลี่ยน long-term thesis หรือแค่ noise?*

### มุมที่ 2: Whale / Large Holder
- โฟกัส: COT report (Gold/Oil), on-chain data (BTC), volume profile, open interest
- คำถาม: *Smart money กำลัง accumulate หรือ distribute?*

### มุมที่ 3: Swing Trader (Weekly/Daily chart)
- โฟกัส: key S/R levels, trend structure, RSI/MACD divergence, weekly close
- คำถาม: *Setup ที่น่าสนใจคืออะไร? risk/reward เป็นอย่างไร?*

### มุมที่ 4: Day Trader (4H/1H/15M chart)
- โฟกัส: intraday bias, session timing (London/NY open), liquidity zones, immediate PA
- คำถาม: *วันนี้ควร trade ทิศทางไหน? ระวัง trap อะไร?*

**หลังวิเคราะห์เสร็จ**: append section วันที่ปัจจุบันลงใน `reference/perspectives.md` ผ่าน GitHub MCP (อัปเดตไฟล์เดิม ไม่ได้สร้างใหม่)

---

## Step 4 — Rewrite & Action Plans (Final Article)

รวม draft + 4-perspective analysis เป็น final article ที่อ่านลื่น:

```markdown
# Daily Asset Brief: {TOPIC} — {YYYY-MM-DD}

> สร้างเมื่อ: {YYYY-MM-DD HH:MM} เวลาไทย

## ภาพรวมตลาด

{1-2 ย่อหน้า สรุปสภาพตลาดโดยรวม บริบท macro ที่สำคัญวันนี้}

## ทองคำ & เงิน (Gold / Silver)

{รวมข่าว + มุมมอง 4 กลุ่มไว้ในย่อหน้าเดียวกัน อ่านต่อเนื่อง ไม่แยก bullet}

**แหล่งข่าว:** [1]({url1}) [2]({url2}) ...

## น้ำมัน (Crude Oil — WTI / Brent)

{รวมข่าว + มุมมอง 4 กลุ่ม}

**แหล่งข่าว:** ...

## Bitcoin (BTC)

{รวมข่าว + มุมมอง 4 กลุ่ม}

**แหล่งข่าว:** ...

---

## Action Plans

### ทองคำ & เงิน

| กลุ่ม | Bias | Zone เข้า | เป้าหมาย | Stop |
|-------|------|-----------|----------|------|
| Institutional | ... | ... | ... | ... |
| Whale | ... | ... | ... | ... |
| Swing Trader | ... | ... | ... | ... |
| Day Trader | ... | ... | ... | ... |

### น้ำมัน

| กลุ่ม | Bias | Zone เข้า | เป้าหมาย | Stop |
|-------|------|-----------|----------|------|
| Institutional | ... | ... | ... | ... |
| Whale | ... | ... | ... | ... |
| Swing Trader | ... | ... | ... | ... |
| Day Trader | ... | ... | ... | ... |

### Bitcoin

| กลุ่ม | Bias | Zone เข้า | เป้าหมาย | Stop |
|-------|------|-----------|----------|------|
| Institutional | ... | ... | ... | ... |
| Whale | ... | ... | ... | ... |
| Swing Trader | ... | ... | ... | ... |
| Day Trader | ... | ... | ... | ... |

---

*⚠️ Disclaimer: บทความนี้เป็นข้อมูลเพื่อการศึกษาเท่านั้น ไม่ใช่คำแนะนำการลงทุน ผู้อ่านควรทำการวิจัยเพิ่มเติมก่อนตัดสินใจลงทุน*
```

---

## Step 5 — Publish

### 5a. Commit ผ่าน GitHub MCP

1. **ตรวจสอบ connector**: ถ้า GitHub MCP tools ไม่พร้อม → `ERROR: GitHub connector unavailable — aborting` แล้ว abort ทันที
2. อ่าน env: `GITHUB_OWNER`, `GITHUB_REPO`
3. ใช้ GitHub MCP tool `create_or_update_file`:
   ```
   owner:   ${GITHUB_OWNER}
   repo:    ${GITHUB_REPO}
   path:    articles/{YYYY-MM-DD}-brief.md
   message: "brief: {TOPIC} {YYYY-MM-DD}"
   content: <base64-encoded final article>
   branch:  main
   ```
4. บันทึก **commit SHA** จาก response
5. สร้าง permalink:
   ```
   https://github.com/${GITHUB_OWNER}/${GITHUB_REPO}/blob/{SHA}/articles/{YYYY-MM-DD}-brief.md
   ```

### 5b. LINE Notification ผ่าน WebFetch

1. ตรวจสอบ env:
   - ไม่มี `LINE_CHANNEL_ACCESS_TOKEN` → `SKIP: LINE_CHANNEL_ACCESS_TOKEN not set` แล้ว skip
   - ไม่มี `LINE_TO` → `SKIP: LINE_TO not set` แล้ว skip

2. สร้าง message:
   ```
   📊 Daily Asset Brief — {YYYY-MM-DD}

   🏅 ทอง/เงิน: {1-line summary}
   🛢️ น้ำมัน: {1-line summary}
   ₿ Bitcoin: {1-line summary}

   📌 อ่านเพิ่มเติม: {permalink}
   ```

3. ส่งด้วย **WebFetch** (POST):
   ```
   URL:    https://api.line.me/v2/bot/message/push
   Method: POST
   Headers:
     Content-Type: application/json
     Authorization: Bearer ${LINE_CHANNEL_ACCESS_TOKEN}
   Body:
     {
       "to": "${LINE_TO}",
       "messages": [{"type": "text", "text": "<msg>"}]
     }
   ```

4. ถ้า status ≠ 200 → log `ERROR: LINE API returned {status} — {body}` ห้าม retry

---

## Step 6 — Summary Log

```
=== Daily Asset Brief Complete ===
Date    : {YYYY-MM-DD HH:MM} Asia/Bangkok
Article : articles/{YYYY-MM-DD}-brief.md
Commit  : {SHA}
Permalink: {permalink}
LINE    : {sent | skipped ({reason}) | error ({status})}
===================================
```
