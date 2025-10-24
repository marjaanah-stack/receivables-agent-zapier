![Demo](demo.gif)

# Receivables Agent (Zapier + OpenAI + Google Workspace)

This project showcases an **Accounts Receivable automation** built with **Zapier Agents**, **OpenAI**, **Google Sheets & Gmail**, and **Slack**.  
The Agent reads AR data from a Sheet, computes *days past due* and aging buckets, drafts **polite but firm UK‑spelling** chaser emails (firmness scales with amount & lateness), creates **Gmail drafts only** (never sends), posts a **Slack summary**, and updates the Sheet.

---

## ✨ Features
- Google Sheets → ingest AR rows and **Controls**
- Auto compute **days_past_due** and **aging buckets** (0–30 / 31–60 / 61–90 / 90+)
- Draft professional chaser emails via **OpenAI** → **Gmail Drafts** (no send)
- **Slack** daily/On‑demand summary
- Updates `status`, `last_contacted_at`, and `last_email_subject` in the Sheet
- Guardrails: **DRY_RUN**, **MAX_EMAILS_PER_RUN**, **MIN_AMOUNT_GBP**, **MIN_DAYS_OVERDUE**

---

## 🧱 Stack
- Zapier **Agents** (On‑demand)
- Apps: Google Sheets, Gmail (Draft), Slack, ChatGPT (OpenAI)
- Optional: Google Drive (for attaching statements), Notion (follow‑ups)
- Language/tone: **UK spelling**

---

## 📁 Repository layout
```
.
├── README.md
├── data/
│   ├── receivables.csv
│   ├── controls.csv
│   └── email_templates.csv
└── docs/
    ├── SETUP.md
    ├── AGENT_INSTRUCTIONS.md
    ├── ARCHITECTURE.md
    ├── TROUBLESHOOTING.md
    └── screenshots/   # put your screenshots here
```
> Use `docs/screenshots` to drop your real Zapier/Sheets/Gmail screenshots.

---

## 🚀 Quickstart
1. **Create** a Google Sheet named `Receivables_Exercise` and import CSVs from `data/` as tabs:
   - `Receivables`, `Controls`, `Email_Templates`
2. **Zapier → App Connections:** connect Google Sheets, Gmail, Slack, ChatGPT (OpenAI).
3. **Create Agent → On‑demand trigger** and add tools:
   - Google Sheets: *Lookup Spreadsheet Rows (Advanced)*
   - Gmail: *Create Draft*
   - Slack: *Send Channel Message*
   - ChatGPT (OpenAI): *Conversation*
4. Paste the full instructions from `docs/AGENT_INSTRUCTIONS.md`.
5. **Run:** Agent → Activity → *New run* → `Run receivables chase now`.
6. Check **Gmail → Drafts**, **Slack**, and the **Sheet** for updated fields.

---

## 🗺️ Architecture
See `docs/ARCHITECTURE.md` for a **Mermaid** flowchart + sequence diagram you can view directly on GitHub.

---

## 🧪 Data & Controls
Edit `controls.csv` to change guardrails:
- `DRY_RUN=TRUE` → Drafts only (safe default)
- `MAX_EMAILS_PER_RUN` → Cap drafts (e.g., 20)
- `MIN_AMOUNT_GBP`, `MIN_DAYS_OVERDUE` → Materiality & grace period

---

## 🧯 Troubleshooting
Common issues and fixes in `docs/TROUBLESHOOTING.md` (e.g., Gmail drafts created but columns not updated, Slack channel not found, OpenAI errors, attachments).

---

## 📸 Screenshots
Add your screenshots under `docs/screenshots/` and embed them in `docs/SETUP.md`. A checklist is provided in that file.

---

## 📝 License
MIT — free to use for your portfolio demos.



### Screenshots (exact filenames)
Place these PNGs into `docs/screenshots/` **with exactly these names** so they render in the README:

1. `01-sheets-tabs.png` — Google Sheet with tabs imported  
2. `02-app-connections.png` — Zapier App Connections  
3. `03-agent-tools.png` — Zapier Agent tools list  
4. `04-instructions.png` — Instructions pasted in Agent  
5. `05-run.png` — Activity run transcript  
6. `06-gmail-drafts.png` — Gmail drafts view  
7. `07-sheet-updates.png` — Receivables updates (last_contacted_at, last_email_subject)  
8. `08-slack-summary.png` — Slack summary post  

They are referenced below:
![Sheets](docs/screenshots/01-sheets-tabs.png)
![Connections](docs/screenshots/02-app-connections.png)
![Tools](docs/screenshots/03-agent-tools.png)
![Instructions](docs/screenshots/04-instructions.png)
![Run](docs/screenshots/05-run.png)
![Drafts](docs/screenshots/06-gmail-drafts.png)
![Updates](docs/screenshots/07-sheet-updates.png)
![Slack](docs/screenshots/08-slack-summary.png)
