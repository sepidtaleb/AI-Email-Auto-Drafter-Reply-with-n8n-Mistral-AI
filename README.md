# AI-Email-Auto-Drafter-Reply-with-n8n-Mistral-AI
This project automates drafting professional email replies for common workplace notifications (sick leave, vacation/day off) using n8n and Mistral LLM.
Instead of automatically sending emails, the workflow creates Gmail drafts in the correct email thread, allowing full human review before sending.

✨ What This Project Does

- Watches unread Gmail messages only

- Uses an LLM (Mistral) to classify incoming emails

- Detects whether an email is about:
sick
vacation
other

- Automatically drafts a polite reply for sick/vacation emails

- Leaves unrelated emails untouched


🧠 Architecture Overview

The workflow is built entirely in n8n using native nodes and HTTP requests.

Gmail Trigger (Unread Emails, every minute)
   ↓
HTTP Request → Mistral (Classification)
   ↓
Edit Fields (normalize output)
   ↓
IF Node (sick / vacation vs other)
   ↓
HTTP Request → Mistral (Draft Reply)
   ↓
Edit Fields (clean + HTML formatting)
   ↓
Gmail → Create Draft (reply in same thread)

No Python scripts, cron jobs, or external servers are required.

🧩 Workflow Nodes Explained
1️⃣ Gmail Trigger

Purpose: Listens for new incoming emails

Filter: is:unread

Prevents duplicate processing

2️⃣ HTTP Request – Email Classification (Mistral)

Sends the email body to a self-hosted Mistral server

Uses /v1/chat/completions

Prompts the model to classify the email as:

sick

vacation

other

3️⃣ Trim (Normalize Output)

Converts model output to:

lowercase

trimmed text

This ensures consistent logic in later steps.

4️⃣ IF Node – Decision Logic

TRUE branch: sick or vacation

FALSE branch: all other emails

Only relevant emails continue through the automation.

5️⃣ HTTP Request – Draft Reply (Mistral)

Uses the classification result as context

Generates a short, polite, professional reply

No mention of automation or AI

Neutral workplace tone

6️⃣ Trim1 and fix line breaks – Cleanup & Formatting

Trims the AI output

Converts newlines to HTML:

replace(/\n/g, '<br>')

Ensures proper formatting in Gmail drafts

7️⃣ Gmail – Create Draft

Creates a reply draft, not a sent email

Preserves the original thread using:

Message ID

Thread ID

Allows manual review before sending

🔐 Authentication & Privacy

Uses a self-hosted Mistral model (no OpenAI API)

Communication via secure HTTP headers

Emails are processed only in-memory during execution

No data is stored outside Gmail and n8n

🛠 Tech Stack

n8n – workflow automation

Gmail API – email trigger and drafts

Mistral LLM (self-hosted) – NLP & text generation

HTTP / JSON – model integration

🚀 Why This Project Matters

This project demonstrates:

Real-world AI integration into business workflows

Safe, reviewable automation (drafts instead of auto-send)

LLM usage without vendor lock-in

Production-style thinking (filters, normalization, safeguards)


🔮 Possible Extensions

Multi-language replies

Custom labels (e.g. Auto-Drafted)

Office-hours-based execution

Slack / Teams integration

Analytics on email categories


👤 Author

Built by Sepide as a practical AI automation project using n8n and self-hosted LLMs.
