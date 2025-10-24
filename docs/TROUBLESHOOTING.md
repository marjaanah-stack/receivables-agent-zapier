# Troubleshooting

### Drafts created but `last_contacted_at` not updated
- Ensure the Agent instructions include a **Sheet update step after creating each draft**.
- Confirm the column names match exactly (`last_contacted_at`, `last_email_subject`).
- Make sure your Agent has the **Google Sheets** tool enabled and the account is correct.

### Slack message not posted
- Verify `SUMMARY_SLACK_CHANNEL` (e.g., `#finance-updates`) exists and Zapier has permission to post to it.

### Gmail draft From/Reply-To
- Drafts will show your Gmail account as **From**. We set **Reply-To** via Controls (e.g., `ar@yourco.example`). To change From, add a Gmail alias and use the alias in the Gmail app settings.

### OpenAI errors
- Check your API key in **App Connections**. Reduce body length if you hit token limits.

### Attach statements
- Add a **Google Docs / Drive** action to generate or fetch a statement PDF by `invoice_id`. Map the resulting file into **Gmail → Create Draft → Attachments** field.

### Run controls
- `DRY_RUN=TRUE` ensures nothing is sent.
- `MAX_EMAILS_PER_RUN` protects from spamming.
- `MIN_AMOUNT_GBP` and `MIN_DAYS_OVERDUE` filter out noise and provide a **grace period** before first chase.
- Buckets control **tone/escalation**, not whether an invoice is eligible to be chased.

