# AI-Powered Lead Management Automation (n8n)

An end-to-end lead management pipeline built in **n8n** that captures leads from **Typeform**, validates and filters spam, scores lead quality using **Groq**, and routes hot leads through **HubSpot**, **Airtable**, **Slack**, and automated **calendar scheduling** — with zero manual triage.

![n8n](https://img.shields.io/badge/n8n-workflow-EA4B71?logo=n8n&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## 🧠 Overview

This workflow automates the entire lifecycle of an inbound lead — from form submission to a qualified rep follow-up — without a human touching a spreadsheet.

**Flow at a glance:**

```
Typeform Submission
      ↓
Webhook → Filter Webhook Data → Is this data Valid?
      ↓ (valid)                      ↓ (invalid)
Spam Detection (AI)             Rejection Response
      ↓
Parsing the Output → If spam?
      ↓ (not spam)        ↓ (spam)
                       Log spam + Rejection
      ↓
AI Lead Scoring (Groq Llama versatile)
      ↓
Parsing the Scoring
      ↓
   ┌──────────────┬──────────────┐
Add to HubSpot   Backup on Airtable   Email the lead
   ↓
Is the score high?
   ↓ (yes)                    ↓ (no)
Send Slack message      (no alert)
   ↓
Determine Follow-up Schedule (business hours aware)
   ↓
Create a calendar Event
```

## ✨ Features

- **Data validation** — filters incomplete/malformed submissions before they enter the pipeline
- **AI spam detection** — checks email patterns, empty names, and message length/coherence to reject spammy or irrelevant submissions
- **AI lead scoring** — uses OpenAI to score and prioritize each lead, with a structured summary
- **CRM sync** — pushes qualified leads into **HubSpot** with custom properties (`score`, `priority`, `summary`)
- **Redundant backup** — mirrors every scored lead into **Airtable**
- **Real-time alerts** — notifies the sales team on **Slack** the moment a lead scores as "hot"
- **Smart scheduling** — automatically determines the right follow-up call time based on lead urgency, business hours, and skips weekends/off-hours
- **Calendar automation** — books the follow-up directly onto the calendar
- **Automated lead email** — sends a confirmation/response email to the lead
- **Built-in resilience** — critical nodes use n8n's **Retry on Fail** strategy, so transient API failures (HubSpot, OpenAI, Slack rate limits, etc.) don't break the pipeline

## 🛠️ Tech Stack

| Tool | Role |
|---|---|
| [n8n](https://n8n.io) | Workflow orchestration |
| [Typeform](https://typeform.com) | Lead capture form |
| [OpenAI](https://openai.com) | Spam detection + lead scoring (AI Agent nodes) |
| [HubSpot](https://hubspot.com) | CRM / contact management |
| [Airtable](https://airtable.com) | Backup lead database |
| [Slack](https://slack.com) | Real-time hot-lead alerts |
| Google Calendar (or similar) | Follow-up scheduling |
| Gmail / SMTP | Automated lead email response |

## ⚙️ Setup

1. Import [`workflow.json`](./workflow.json) into your n8n instance (**Workflows → Import from File**)
2. Configure credentials for each service used:
   - Typeform (Webhook, or Personal Access Token if using the Typeform Trigger node)
   - OpenAI API key
   - HubSpot Private App token (with contacts read/write scope)
   - Airtable Personal Access Token
   - Slack Bot Token (`chat:write` scope)
   - Google Calendar OAuth2
   - Email service (SMTP or Gmail OAuth2)
3. Set up your Typeform webhook to point to this workflow's production Webhook URL
4. Create the required custom HubSpot contact properties: `score`, `priority`, `summary`
5. Activate the workflow

## 🛡️ Error Handling

Key nodes (HubSpot, Airtable, Slack, OpenAI calls) are configured with n8n's **Retry on Fail** setting, so transient failures (rate limits, timeouts, brief API outages) are automatically retried instead of silently dropping a lead.

## 📸 Workflow in Action

<details>
<summary>Click to expand screenshots</summary>

### Typeform submission
`![Typeform](./screenshots/typeform.png)`

### Lead scored & synced to HubSpot
`![HubSpot](./screenshots/hubspot.png)`

### Backup record in Airtable
`![Airtable](./screenshots/airtable.png)`

### Hot lead Slack alert
`![Slack](./screenshots/slack.png)`

### Follow-up calendar event created
`![Calendar](./screenshots/calendar.png)`

### Automated email to the lead
`![Email](./screenshots/email.png)`

</details>

*(Replace the paths above with real screenshots in a `/screenshots` folder — drag your images in and update the markdown image syntax, e.g. `![HubSpot](./screenshots/hubspot.png)`.)*

## 📄 License

This project is licensed under the [MIT License](./LICENSE).

---

Built with n8n as part of an ongoing exploration of AI-powered business process automation.
