---
name: incident-report-generator
description: "Generate Taiwan government-format cybersecurity incident reports (個人資料侵害事故通報與紀錄表) as .docx files using generate.py. Use when creating 資安事件報告, government notification forms (通報表), data breach reports for Taiwan regulatory bodies, or 個人資料侵害事故通報 documents."
---

# Incident Report Generator — 資安事件通報報告產生器

Generate cybersecurity incident reports in the official Taiwan government format (個人資料侵害事故通報與紀錄表) as `.docx` files, following the notification format required by Taiwan's Ministry of Digital Affairs (數位發展部) under the Personal Data Protection Act (個人資料保護法).

## Workflow

### Step 1: Gather Information

Collect the following from the user (ask for anything not provided):

- **Incident description** — what happened and when
- **Company's relationship** to the incident (direct breach vs. third-party)
- **Whether company data was actually breached**
- **Reporter contact info** — name, title, phone, email, address
- **Receiving agency** — defaults to 數位發展部數位產業署
- **Incident type** — one of: 竊取 / 洩漏 / 竄改 / 毀損 / 滅失 / 其他
- **Records affected** — count of 一般個資 and 特種個資

### Step 2: Build Config JSON

Create a config JSON file with the gathered information. All fields are optional with sensible defaults. See `example-config.json` for the full schema.

Minimal working example:

```json
{
  "company_name": "某某科技股份有限公司",
  "incident_date": "2026-03-07",
  "incident_type": "洩漏",
  "general_records": 500,
  "reporter": {
    "name": "王小明",
    "title": "資安長",
    "phone": "02-1234-5678",
    "email": "security@example.com",
    "address": "台北市信義區某路100號"
  },
  "cause_summary": "外部攻擊導致資料外洩",
  "damage": "客戶姓名及電子郵件遭洩漏",
  "countermeasures": "已封鎖攻擊來源並重設所有存取金鑰",
  "appendix": {
    "title": "資料外洩事件 — 某某科技事件報告",
    "sections": {
      "summary": "2026年3月7日發現外部攻擊...",
      "timeline": [["2026/03/07 06:45", "發現異常存取"], ["2026/03/07 08:00", "啟動應變團隊"]],
      "conclusions": ["事件已控制，無進一步外洩風險"],
      "follow_up": ["持續監控並強化存取控制"]
    }
  }
}
```

### Step 3: Generate the Report

```bash
python3 generate.py --output /path/to/output.docx --config /path/to/config.json
```

Or call `generate_report(config_dict, "output.docx")` directly from Python.

### Step 4: Validate Output

Verify the generated `.docx`:

1. **All sections present** — confirm both the government form (Part 1) and appendix (Part 2) rendered
2. **No placeholder text** — check that default/example values were replaced with actual incident data
3. **72-hour compliance** — verify `within_72_hours` field matches the actual notification timeline
4. **Record counts** — confirm `general_records` and `special_records` match the incident scope

If `generate.py` fails, check: config JSON is valid (use `python3 -m json.tool config.json`), `python-docx` is installed (`pip install python-docx`), and output path is writable.

### Step 5: Review with User

Review the document with the user and refine content as needed.

## Report Structure

The output `.docx` contains two parts:

**Part 1 — 個人資料侵害事故通報與紀錄表 (Government Form):** A structured 3-column table with merged cells covering: company/agency info, reporter details, incident timing/type, records affected, cause summary, damage assessment, countermeasures, notification plan, and 72-hour compliance.

**Part 2 — 附錄說明文件 (Detailed Explanation):** Seven sections — (1) 事件摘要, (2) 與本公司之關聯, (3) 事件時間軸, (4) 系統安全架構說明 (subsections 4.1–4.6: infrastructure, encryption, API security, monitoring, IDS/IPS, access control), (5) 系統排查報告, (6) 結論, (7) 後續措施.

## Document Formatting

- **Page**: A4, margins: top 1.45cm, bottom 2.45cm, left 1.99cm, right 1.95cm
- **Government form font**: 楷體 (Kai), 14pt; **Body text**: Calibri, ~11pt
- **Appendix headings**: 14pt (H2), 12pt (H3)
- **Tables**: Full borders; timeline = 2 columns; audit results = 4 columns

## Security Architecture Config Formats

The `security_architecture.subsections` field supports three formats:

- **Simple list** (4.1–4.4): `"4.1 基礎架構": ["Point 1", "Point 2"]`
- **Intro + items** (4.5): `"4.5 入侵偵測與防禦": {"intro": "...", "items": ["..."]}`
- **Dual-section** (4.6): `"4.6 存取控制": {"system_title": "...", "system_items": [...], "app_title": "...", "app_items": [...]}`

## Reference Data

For common audit items (AWS GuardDuty, CloudTrail, WAF, etc.), internal procedure names (網路安全管理程序, 存取控制管理程序, etc.), and compliance standards (PCI DSS, ISO 27001, ISO 27701, Taiwan PDPA), see `example-config.json` which includes representative examples for each section.
