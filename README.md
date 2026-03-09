# 資安事件通報報告產生器 — Incident Report Generator

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) / Openclaw skill that generates cybersecurity incident reports in the official Taiwan government format (**個人資料侵害事故通報與紀錄表**).

Produces `.docx` files matching the notification format required by Taiwan's Ministry of Digital Affairs (數位發展部) under the Personal Data Protection Act (個人資料保護法).

## What It Generates

The report consists of two parts:

### Part 1: Government Notification Form

A structured form table (個人資料侵害事故通報與紀錄表) with all required fields:
- Company and agency information
- Reporter details
- Incident classification (竊取/洩漏/竄改/毀損/滅失/其他)
- Records affected count
- Cause summary, damage assessment, countermeasures
- 72-hour notification compliance

### Part 2: Detailed Appendix

A comprehensive explanation document with 7 sections:
1. Event summary
2. Company relationship to the incident
3. Incident timeline
4. System security architecture (infrastructure, encryption, API security, monitoring, IDS/IPS, access control)
5. System audit report with results table
6. Conclusions
7. Follow-up measures

## Installation as Claude Code Skill

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/Oen-Tech/incident-report-skill.git \
  ~/.claude/skills/incident-report
```

Or symlink if you prefer:
```bash
git clone https://github.com/Oen-Tech/incident-report-skill.git ~/Code/incident-report-skill
ln -s ~/Code/incident-report-skill ~/.claude/skills/incident-report
```

### Requirements

```bash
pip install python-docx
```

## Usage

### With Claude Code

Simply ask Claude to generate an incident report:
- "Generate a cybersecurity incident report"
- "建立資安事件通報表"
- "Create an incident report for the data breach"

Claude will use the skill to gather details and generate the `.docx` file.

### Standalone CLI

```bash
# With a config file
python3 generate.py --output report.docx --config my-config.json

# With example defaults
python3 generate.py --output report.docx
```

### As a Python Library

```python
from generate import generate_report

config = {
    "company_name": "My Company",
    "incident_date": "2026-03-07",
    "reporter": {
        "name": "Jane Smith",
        "title": "CISO",
        "email": "security@mycompany.com"
    },
    "appendix": {
        "title": "Data Breach — Incident Report",
        "sections": {
            "summary": "Description of what happened...",
            "timeline": [
                ["2026/03/07", "Incident discovered"],
                ["2026/03/07", "Response team activated"]
            ],
            # ... more sections
        }
    }
}

generate_report(config, "output.docx")
```

## Configuration

All fields are optional — sensible defaults are provided. See `example-config.json` for a complete example or `SKILL.md` for the full schema documentation.

### Key Config Fields

| Field | Description | Default |
|-------|-------------|---------|
| `company_name` | Your company name | "ACME Technology Co., Ltd." |
| `receiving_agency` | Government agency | "數位發展部數位產業署" |
| `report_date` | Report date (YYYY-MM-DD) | Today |
| `reporter` | Reporter contact info object | Example values |
| `incident_date` | When the incident occurred | Empty |
| `incident_type` | 竊取/洩漏/竄改/毀損/滅失/其他 | "其他" |
| `appendix.sections` | Detailed report content | See SKILL.md |

## Document Format

- **Page size**: A4 (21 × 29.7 cm)
- **Margins**: Top 1.45cm, Bottom 2.45cm, Left 1.99cm, Right 1.95cm
- **Form font**: 楷體 (Kai), 14pt
- **Body font**: Calibri, ~11pt
- **Tables**: Bordered, with Kai font

## License

MIT
