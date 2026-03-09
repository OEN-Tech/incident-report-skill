# 資安事件通報報告產生器 — Incident Report Generator

一個 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) / [Openclaw](https://openclaw.ai) skill，自動產生符合台灣政府格式的資安事件報告（**個人資料侵害事故通報與紀錄表**）`.docx` 文件。

依據《個人資料保護法》規範，產出符合數位發展部（數位產業署）要求的通報格式。

## 產出內容

報告包含兩部分：

### 第一部分：政府通報表

結構化表格（個人資料侵害事故通報與紀錄表），涵蓋所有必填欄位：
- 事業名稱與通報機關
- 通報人資訊
- 事件發生種類（竊取／洩漏／竄改／毀損／滅失／其他）
- 個資侵害總筆數
- 發生原因及事件摘要、損害狀況、因應措施
- 72 小時通報合規狀態

### 第二部分：附錄說明文件

完整的事件說明文件，包含 7 個章節：
1. 事件摘要
2. 與本公司之關聯
3. 事件時間軸
4. 系統安全架構說明（基礎架構、加密與金鑰管理、API 安全、監控與威脅偵測、入侵偵測與防禦、存取控制）
5. 系統排查報告（含排查結果表格）
6. 結論
7. 後續措施

## 安裝為 Claude Code Skill

```bash
# Clone 到 Claude Code skills 目錄
git clone https://github.com/Oen-Tech/incident-report-skill.git \
  ~/.claude/skills/incident-report
```

或用 symlink：
```bash
git clone https://github.com/Oen-Tech/incident-report-skill.git ~/Code/incident-report-skill
ln -s ~/Code/incident-report-skill ~/.claude/skills/incident-report
```

### 相依套件

```bash
pip install python-docx
```

## 使用方式

### 搭配 Claude Code

直接請 Claude 產生報告：
- 「建立資安事件通報表」
- 「產生個資事故通報報告」
- 「Generate a cybersecurity incident report」

Claude 會透過 skill 蒐集相關資訊後自動產生 `.docx` 文件。

### 命令列（CLI）

```bash
# 使用設定檔
python3 generate.py --output report.docx --config my-config.json

# 使用預設範例值
python3 generate.py --output report.docx
```

### 作為 Python Library

```python
from generate import generate_report

config = {
    "company_name": "某某科技股份有限公司",
    "incident_date": "2026-03-07",
    "reporter": {
        "name": "王小明",
        "title": "資安長",
        "email": "security@example.com"
    },
    "appendix": {
        "title": "資料外洩事件 — 事件報告",
        "sections": {
            "summary": "事件經過描述...",
            "timeline": [
                ["2026/03/07", "發現事件"],
                ["2026/03/07", "啟動應變團隊"]
            ],
            # ... 更多章節
        }
    }
}

generate_report(config, "output.docx")
```

## 設定

所有欄位皆為選填，預設值已提供。完整範例請參考 `example-config.json`，完整 schema 說明請參考 `SKILL.md`。

### 主要設定欄位

| 欄位 | 說明 | 預設值 |
|------|------|--------|
| `company_name` | 公司名稱 | "ACME Technology Co., Ltd." |
| `receiving_agency` | 通報機關 | "數位發展部數位產業署" |
| `report_date` | 通報日期 (YYYY-MM-DD) | 當天日期 |
| `reporter` | 通報人聯絡資訊 | 範例值 |
| `incident_date` | 事件發生日期 | 空白 |
| `incident_type` | 竊取／洩漏／竄改／毀損／滅失／其他 | "其他" |
| `appendix.sections` | 附錄各章節內容 | 參考 SKILL.md |

## 文件格式

- **紙張大小**：A4（21 × 29.7 cm）
- **邊界**：上 1.45cm、下 2.45cm、左 1.99cm、右 1.95cm
- **表格字體**：楷體（Kai），14pt
- **內文字體**：Calibri，約 11pt
- **表格**：全框線，楷體

## 授權

MIT
