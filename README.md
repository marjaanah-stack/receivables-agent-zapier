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

![Zapier App Connections](docs/screenshots/02-app-connections.png)

The Agent:
1. Reads AR data from **Google Sheets**
2. Filters invoices that are overdue
3. Drafts tailored email reminders using ChatGPT
4. Creates Gmail drafts (never sends)
5. Updates the sheet with contact date & subject
6. Posts a concise summary to **Slack**

---

## 🧾 Example Data Structure (Google Sheets)

### 1️⃣ Receivables Tab
![Google Sheets Tabs](docs/screenshots/01-sheets-tabs.png)

Columns:
invoice_id | customer_name | customer_email | amount_gbp |
issue_date | due_date | terms_days | status |
last_contacted_at | promise_to_pay_date | notes | last_email_subject

shell
Copy code

### 2️⃣ Controls Tab
Key configuration flags (editable in Google Sheets):
DRY_RUN, TRUE
MAX_EMAILS_PER_RUN, 20
MIN_AMOUNT_GBP, 100
MIN_DAYS_OVERDUE, 3
SUMMARY_SLACK_CHANNEL, #finance-updates
REPLY_TO, ar@yourco.example

csharp
Copy code

### 3️⃣ Email Templates Tab
Contains message structures by **aging bucket**:
template_name, bucket, min_days_overdue, subject, body

yaml
Copy code

---

## ⚙️ Agent Setup in Zapier

### App Connections
![App Connections](docs/screenshots/02-app-connections.png)

Connect:
- Google Sheets  
- Gmail  
- Slack  
- ChatGPT (OpenAI)

### Agent Tools
![Agent Tools](docs/screenshots/03-agent-tools.png)

Tools used:
- Google Sheets → Lookup Spreadsheet Rows (Advanced)
- Gmail → Create Draft
- Slack → Send Channel Message
- ChatGPT (OpenAI) → Conversation

### Agent Instructions
![Agent Instructions](docs/screenshots/04-instructions.png)

Paste the full **Instructions to follow** block from  
`docs/AGENT_INSTRUCTIONS.md`.

The agent reads configuration values dynamically from the Controls tab (e.g. DRY_RUN, thresholds, Slack channel).

---

## 🚀 Running the Agent

### Activity Run
![Agent Run](docs/screenshots/05-run.png)

You can run it manually using:
> **Run receivables chase now**

Watch the logs as it:
- Reads data from Google Sheets  
- Computes overdue status  
- Renders emails via ChatGPT  
- Creates Gmail drafts  
- Updates the sheet  
- Posts to Slack

---

## 📨 Gmail Draft Output
![Gmail Drafts](docs/screenshots/06-gmail-drafts.png)

Each email draft includes:
- Subject tailored to invoice and overdue bucket
- Tone that escalates with days overdue
- Notes (e.g. “Partial dispute”) incorporated for context
- Reply-To field from the Controls tab

---

## 📊 Sheet Updates
![Sheet Updates](docs/screenshots/07-sheet-updates.png)

After drafting:
- `status` → “Draft created”  
- `last_contacted_at` → today’s date  
- `last_email_subject` → subject line used  

This provides a clear audit trail for the finance team.

---

## 💬 Slack Summary
![Slack Summary](docs/screenshots/08-slack-summary.png)

After each run, the Agent posts a summary such as:

> **Receivables Agent Run — 29 Sep 2025**  
> Drafts created: 7 (capped at 20)  
> Oldest overdue: 92 days  
> Total chased: £41,550  
> Guardrails: DRY_RUN=TRUE · MIN £100 · MIN 3 days

---

## 🧩 How It Works

| Step | Description |
|------|--------------|
| 1️⃣ | Read Receivables tab from Google Sheets |
| 2️⃣ | Compute `days_past_due` |
| 3️⃣ | Filter by overdue thresholds |
| 4️⃣ | Choose correct **email template** by bucket |
| 5️⃣ | Render subject/body using **ChatGPT** |
| 6️⃣ | Create **Gmail Draft** |
| 7️⃣ | Update row (status, last_contacted_at, last_email_subject) |
| 8️⃣ | Post Slack summary |

---

## 🧱 Folder Structure

receivables-agent-zapier/
│
├── data/ # Sample CSVs for Receivables, Controls, Templates
├── docs/
│ ├── screenshots/ # All screenshot PNGs used in README
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

Observe:
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
- Check `MIN_DAYS_OVERDUE` and `promise_to_pay_date` filters
- Ensure all tabs (Receivables, Controls, Templates) exist
- Verify the Zapier Google Sheets connection points to the correct account
- Confirm `DRY_RUN` is not blocking output

For PDF statement attachments, add a Google Drive step:
> *Google Drive → Export File → Microsoft PowerPoint (.pptx) or PDF*  
and map the exported file into Gmail “Attachments”.

---

## 🧩 Screenshot Reference Index

| File | Description |
|------|--------------|
| `01-sheets-tabs.png` | Google Sheets — Receivables, Controls, Templates tabs |
| `02-app-connections.png` | Zapier — App connections view |
| `03-agent-tools.png` | Zapier — Tools used in the agent |
| `04-instructions.png` | Zapier — Agent instructions block |
| `05-run.png` | Agent run activity log |
| `06-gmail-drafts.png` | Gmail — generated drafts |
| `07-sheet-updates.png` | Google Sheets — status + contact fields updated |
| `08-slack-summary.png` | Slack — summary message posted |

---

## 📄 License
MIT License — free to reuse and modify.

---

## 👩‍💻 Author

**Marjaana Peeters**  
Finance-AI & Automation Enthusiast  
📍 London, UK  
💼 [LinkedIn](www.linkedin.com/in/marjaana-peeters-0442a4) · 
