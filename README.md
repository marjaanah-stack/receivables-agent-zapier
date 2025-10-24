# 🤖 Receivables Agent — Automated AR Follow-Up (Zapier + OpenAI + Google Workspace)

![Demo](demo.gif)

A smart, **AI-powered Receivables Agent** that automates AR (accounts receivable) chasers — reading invoice data from **Google Sheets**, drafting polite-but-firm reminder emails, posting a summary to **Slack**, and updating the sheet status — all through **Zapier AI Agents** and the **OpenAI API**.

---

## 📚 Overview

| Component | Description |
|------------|--------------|
| **Trigger** | On-demand (manual or scheduled daily) |
| **Apps Used** | Google Sheets, Gmail, Slack, ChatGPT (OpenAI) |
| **Purpose** | Automate receivables chasing with tone control and audit trail |
| **Safety** | Drafts only (no auto-send), rate-limited by run parameters |

---

## 🧠 Flow Diagram

![Agent Flow Overview](docs/screenshots/02-agent-setup.png)

The Agent:
1. Reads AR data from **Google Sheets**
2. Filters invoices that are overdue
3. Drafts tailored email reminders using ChatGPT
4. Creates Gmail drafts (never sends)
5. Updates the sheet with contact date & subject
6. Posts a concise summary to **Slack**

---

## 🧾 Example Data Structure (Google Sheets)

### Receivables Tab
![Sheets Tabs Overview](docs/screenshots/01-sheets-tabs.png)

Columns:
invoice_id | customer_name | customer_email | amount_gbp |
issue_date | due_date | terms_days | status |
last_contacted_at | promise_to_pay_date | notes | last_email_subject

shell
Copy code

### Controls Tab
Key configuration flags (editable in Google Sheets):
DRY_RUN, TRUE
MAX_EMAILS_PER_RUN, 20
MIN_AMOUNT_GBP, 100
MIN_DAYS_OVERDUE, 3
SUMMARY_SLACK_CHANNEL, #finance-updates
REPLY_TO, ar@yourco.example

csharp
Copy code

### Email Templates Tab
Contains message structures by **aging bucket**:
template_name, bucket, min_days_overdue, subject, body

yaml
Copy code

---

## ⚙️ Agent Setup in Zapier

![Zapier Agent Configuration](docs/screenshots/03-agent-instructions.png)

**Steps:**
1. Create a new **Zapier Agent** called `Receivables Agent`.
2. Add tools:
   - Google Sheets → Lookup Spreadsheet Rows (Advanced)
   - Gmail → Create Draft
   - Slack → Send Channel Message
   - ChatGPT (OpenAI) → Conversation
3. Paste the *instructions block* from `docs/AGENT_INSTRUCTIONS.md`.

The agent reads configuration values (e.g. DRY_RUN, thresholds) dynamically from the Controls tab.

---

## 📨 Gmail Draft Output

![Gmail Draft Example](docs/screenshots/04-gmail-draft.png)

Each email is created as a **draft** in Gmail:
- Subject line tailored to the invoice and overdue bucket
- Tone varies with aging severity
- Notes (e.g. “Partial dispute”) influence context
- Reply-to is configurable from the Controls tab

---

## 💬 Slack Summary Report

![Slack Summary Example](docs/screenshots/05-slack-summary.png)

After every run, the Agent posts a summary message such as:

> **Receivables Agent Run — 29 Sep 2025**  
> Drafts created: 7 (capped at 20)  
> Oldest overdue: 92 days  
> Total chased: £41,550  
> Guardrails: DRY_RUN=TRUE · MIN £100 · MIN 3 days

---

## 🧩 How It Works

| Step | Description |
|------|--------------|
| 1️⃣ | **Read Receivables tab** from Google Sheets |
| 2️⃣ | Compute `days_past_due` |
| 3️⃣ | Filter by overdue thresholds |
| 4️⃣ | Choose correct **email template** by bucket |
| 5️⃣ | Render subject/body using **ChatGPT** |
| 6️⃣ | Create **Gmail Draft** |
| 7️⃣ | Update the row (status, last_contacted_at, last_email_subject) |
| 8️⃣ | Post Slack summary |

---

## 🧱 Folder Structure

receivables-agent-zapier/
│
├── data/ # Sample CSVs for Receivables, Controls, Templates
├── docs/
│ ├── screenshots/ # PNGs used in README
│ ├── AGENT_INSTRUCTIONS.md # Zapier Agent instruction block
│ ├── ARCHITECTURE.md # Flow & design notes
│ ├── SETUP.md # Setup guide (Google Sheets + Zapier)
│ ├── TROUBLESHOOTING.md # Common fixes
│
├── demo.gif # Visual demo of the agent run
├── README.md # This file
└── LICENSE

yaml
Copy code

---

## 🧪 Test Run

Use the **Zapier Agent “Activity → New Run”** command:

> `Run receivables chase now`

Watch in the Zapier UI:
- ✅ Rows fetched  
- 🧠 Email bodies rendered via OpenAI  
- ✉️ Gmail drafts appear  
- 🧾 Sheet updates occur  
- 🔔 Slack summary posted

---

## ⚙️ Controls & Safety

| Parameter | Purpose |
|------------|----------|
| **DRY_RUN** | `TRUE` = drafts only (safe default) |
| **MAX_EMAILS_PER_RUN** | Prevents spamming |
| **MIN_AMOUNT_GBP / MIN_DAYS_OVERDUE** | Materiality filters |
| **Slack Channel** | For reporting outcomes |

---

## 🧰 Troubleshooting

If the agent skips records:
- Check the `MIN_DAYS_OVERDUE` and `promise_to_pay_date` filters
- Ensure all tabs (Receivables, Controls, Templates) exist
- Verify Zapier Google Sheets connection is to the correct account

For PDF statement attachments, connect **Google Drive → Export File** (optional).

---

## 📄 License
MIT License — free to reuse and modify.

---

## 🙋‍♀️ Author

**Marjaana Peeters**  
Finance-AI & Automation Enthusiast  
📍 London, UK  
💼 [LinkedIn](www.linkedin.com/in/marjaana-peeters-0442a4) | 🧠 [ChatGPT-Native Portfolio](https://github.com/marjaan-stack)

---

### ⭐ Screenshot Reference Index

| File | Description |
|------|--------------|
| `01-sheets-tabs.png` | Google Sheets tabs overview |
| `02-agent-setup.png` | Zapier agent workflow |
| `03-agent-instructions.png` | Agent instruction block |
| `04-gmail-draft.png` | Example generated Gmail draft |
| `05-slack-summary.png` | Slack summary message |
| `06-run-complete.png` | Completed Zapier agent run |

---

💡 *Tip:* To make images render instantly, ensure paths are relative (as above) and committed to `docs/screenshots/`.

