# Autonomous Email Classifier & Management Pipeline (`n8n` + Gemini AI)

An automated email management workflow built in **n8n** that monitors incoming Gmail messages via native polling triggers, classifies email intent using **Google Gemini AI**, logs metadata to a structured Data Table, and performs automated inbox maintenance (Mark as Read / Delete).

---

## 📌 Features & Key Capabilities

- **Native Event Monitoring**: Uses n8n's `Gmail Trigger` node to automatically poll for unread incoming emails.
- **Intelligent LLM Classification**: Connects directly to Gemini models (`gemini-flash-latest` / `gemini-3-flash-preview`) to generate structured JSON classifications.
- **Strict Feasibility & Sanity Rules**: Prompt engineered with strict negative constraints to catch gibberish noise, spam, and impossible time schedules (e.g., "2 PM night meetings") as `NOT_IMPORTANT`.
- **Structured Data Logging**: Captures email metadata (`msg_id`, `thread_id`, `From`, `Subject`, `Snippet`, `Classification`, and `Reason`) and persists it into an n8n Data Table.
- **Automated Inbox Cleanup**: Evaluates classification outputs to automatically mark critical emails as `READ` and delete irrelevant noise or test messages.

---

## 🛠️ Workflow Architecture

```text
[ Gmail Trigger (Unread Filter) ]
                │
                ▼
[ Gemini LLM: Structured JSON Classification ]
                │
                ▼
[ Data Table: Log Metadata & Reason ]
                │
                ▼
             [ If ]
             ├── true  (IMPORTANT)     ──> [ Gmail: Mark as Read ]
             └── false (NOT_IMPORTANT) ──> [ Gmail: Delete Message ]
