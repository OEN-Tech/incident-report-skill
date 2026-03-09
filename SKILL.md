# Incident Report Generator — 資安事件通報報告產生器

Generate cybersecurity incident reports in the official Taiwan government format (個人資料侵害事故通報與紀錄表) as `.docx` files.

This skill is designed for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and follows the notification format required by Taiwan's Ministry of Digital Affairs (數位發展部) under the Personal Data Protection Act (個人資料保護法).

## When to Use

Use this skill when the user asks to:
- Create a cybersecurity incident report (資安事件報告)
- Generate a government notification form (通報表)
- Write an incident report for a Taiwan regulatory body
- Respond to a data breach notification requirement
- Create a 個人資料侵害事故通報 document

Trigger words: "incident report", "資安事件", "通報", "個資事件", "data breach report", "事故報告", "通報表"

## Report Format

The report follows Taiwan's official **個人資料侵害事故通報與紀錄表** format, consisting of two parts:

### Part 1: 個人資料侵害事故通報與紀錄表 (Government Form)

A structured table form with these fields:
- **事業名稱** / **通報機關**: Company name and receiving agency
- **通報時間**: Notification timestamp
- **通報人**: Reporter name, title, phone, email, address
- **事件發生時間**: When the incident occurred
- **事件發生種類**: Incident type checkboxes (竊取/洩漏/竄改/毀損/滅失/其他)
- **個資侵害之總筆數**: Number of records affected (一般個/特種個)
- **發生原因及事件摘要**: Cause and summary
- **損害狀況**: Damage assessment
- **個資侵害可能結果**: Possible consequences
- **擬採取之因應措施**: Planned countermeasures
- **擬採通知當事人之時間及方式**: Notification plan for affected individuals
- **72小時通報**: Whether reported within 72 hours

### Part 2: 附錄 — 說明文件 (Detailed Explanation)

The appendix follows this section structure:
1. **一、事件摘要** — What happened, when, scope
2. **二、與本公司之關聯** — How the company is connected to the incident
3. **三、事件時間軸** — Chronological table of events and response actions
4. **四、本公司系統安全架構說明** — Technical security overview:
   - 4.1 基礎架構 (Infrastructure)
   - 4.2 加密與金鑰管理 (Encryption & Key Management)
   - 4.3 API 安全 (API Security)
   - 4.4 監控與威脅偵測 (Monitoring & Threat Detection)
   - 4.5 入侵偵測與防禦 (IDS/IPS)
   - 4.6 存取控制 (Access Control)
5. **五、系統排查報告** — Audit results table (item, scope, result)
6. **六、結論** — Key conclusions
7. **七、後續措施** — Follow-up actions

## Document Formatting Specifications

### Page Setup
- **Page size**: A4 (21.00 cm × 29.70 cm)
- **Margins**: Top 1.45 cm, Bottom 2.45 cm, Left 1.99 cm, Right 1.95 cm

### Fonts
- **Government form (Part 1)**: 楷體 (Kai), 14pt
- **Appendix headings (Heading 2)**: Default heading font, 14pt
- **Appendix sub-headings (Heading 3)**: Default heading font, 12pt
- **Body text**: Calibri, ~11pt

### Tables
- **Government form**: 3-column table with merged cells, bordered
- **Timeline table**: 2 columns (時間, 事件)
- **Audit results table**: 4 columns (項次, 排查項目, 排查範圍, 結果)

## How to Generate

Use the Python script at the skill directory's `generate.py`.

### Steps

1. **Gather information** from the user. Ask for anything not provided:
   - Incident description (what happened)
   - Date of incident
   - Company's relationship to the incident
   - Whether any company data was actually breached
   - Reporter contact info
   - Receiving agency

2. **Run the generator**:
```bash
python3 generate.py --output /path/to/output.docx --config /path/to/config.json
```

Or call the `generate_report()` function directly from Python with a config dict.

3. **Config JSON structure** (all fields optional, defaults provided):
```json
{
  "company_name": "Your Company Name",
  "receiving_agency": "數位發展部數位產業署",
  "report_date": "2026-03-09",
  "report_time": "12:00",
  "reporter": {
    "name": "Reporter Name",
    "title": "Job Title",
    "phone": "0900-000000",
    "email": "security@example.com",
    "address": "Company Address"
  },
  "incident_date": "2026-03-07",
  "incident_type": "其他",
  "incident_type_note": "Description of incident type",
  "records_affected": "Description of affected records",
  "general_records": 0,
  "special_records": 0,
  "cause_summary": "Brief cause description or '請參考底部附錄'",
  "damage": "Damage assessment",
  "possible_consequences": "Possible consequences description",
  "countermeasures": "Countermeasures or '請參考底部附錄'",
  "notification_plan": "How affected individuals will be notified",
  "within_72_hours": true,
  "within_72_hours_reason": "Reason if not within 72 hours",
  "appendix": {
    "title": "Event Name — Company Incident Report",
    "doc_nature": "資安事件通報說明",
    "sections": {
      "summary": "Full event summary paragraph...",
      "relation": "How your company relates to the incident...",
      "relation_conclusion": "Key conclusion about company involvement...",
      "relation_details": [
        "Detail point 1...",
        "Detail point 2..."
      ],
      "timeline": [
        ["2026/03/07 06:45", "Event description"],
        ["2026/03/07 AM", "Response action"]
      ],
      "security_architecture": {
        "intro": "Company platform description...",
        "standards_intro": "Standards compliance intro...",
        "standards": ["PCI DSS Level 1...", "ISO 27001...", "ISO 27701..."],
        "subsections": {
          "4.1 基礎架構": ["Infrastructure point 1", "Infrastructure point 2"],
          "4.2 加密與金鑰管理": ["Encryption point 1", "Encryption point 2"],
          "4.3 API 安全": ["API security point 1"],
          "4.4 監控與威脅偵測": ["Monitoring point 1"],
          "4.5 入侵偵測與防禦": {
            "intro": "IDS/IPS description...",
            "items": ["IDS item 1", "IDS item 2"]
          },
          "4.6 存取控制": {
            "system_title": "System-level Access Control (IAM)",
            "system_items": ["IAM point 1", "IAM point 2"],
            "app_title": "Application-level Access Control (RBAC)",
            "app_items": ["RBAC point 1", "RBAC point 2"]
          }
        }
      },
      "audit_procedures": [
        "Internal procedure reference 1",
        "Internal procedure reference 2"
      ],
      "audit_results": [
        ["Audit Item Name", "Audit Scope", "Result"],
        ["Another Audit Item", "Another Scope", "No anomalies"]
      ],
      "audit_conclusion": "Overall audit conclusion...",
      "conclusions": [
        "Conclusion point 1...",
        "Conclusion point 2..."
      ],
      "follow_up": [
        "Follow-up action 1...",
        "Follow-up action 2..."
      ]
    }
  }
}
```

4. **Review and adjust** the generated document as needed.

## Customization

### Security Architecture

The `security_architecture` section in the config supports these subsection formats:

**Simple list** (for 4.1–4.4):
```json
"4.1 基礎架構": ["Point 1", "Point 2"]
```

**Intro + items** (for 4.5):
```json
"4.5 入侵偵測與防禦": {
  "intro": "Overview paragraph...",
  "items": ["Detail 1", "Detail 2"]
}
```

**Dual-section** (for 4.6):
```json
"4.6 存取控制": {
  "system_title": "System-level title",
  "system_items": ["Item 1"],
  "app_title": "App-level title",
  "app_items": ["Item 1"]
}
```

### Audit Results

The audit results table accepts rows of `[item_name, scope, result]`:
```json
"audit_results": [
  ["GuardDuty Threat Detection", "System threat detection", "No anomalies"],
  ["CloudTrail API Audit", "All API access logs", "No unauthorized access"]
]
```

## Common Audit Items for AWS-based Systems

These are typical items to include in the audit results table:

1. GuardDuty 威脅偵測紀錄 — System threat detection
2. GuardDuty IAM 異常連線偵測 — IAM credential anomaly detection
3. Security Hub 安全態勢檢查 — Unified security posture
4. CloudTrail API 呼叫稽核 — Full API access audit
5. WAF 日誌分析 — Web Application Firewall logs
6. Database 資料存取紀錄 — Database read/write operations
7. Lambda/Function 執行日誌 — Compute function execution logs
8. Secrets Manager 存取紀錄 — Secret access audit
9. KMS 金鑰使用紀錄 — Encryption key usage audit

## Common Internal Procedure References

Typical information security management procedures to reference:

- Network Security Management (網路安全管理程序)
- Access Control Management (存取控制管理程序)
- Information Security Incident Management (資訊安全事件管理程序)
- Personal Data Management (個人資料管理程序)
- Incident Notification & Crisis Management (資訊安全事件通報及危機處理作業說明書)
- Account & Password Management (帳號及密碼管理要點)
- Firewall Management (防火牆管理作業說明書)
- Encryption Key Management (加密金鑰管理作業說明書)

## Compliance Standards

Common standards to reference in reports:
- **PCI DSS** (Payment Card Industry Data Security Standard) — for payment processing
- **ISO 27001** (ISMS) — Information Security Management System
- **ISO 27701** (PIMS) — Privacy Information Management System
- **Taiwan PDPA** (個人資料保護法) — Personal Data Protection Act
