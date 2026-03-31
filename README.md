# Startup Diligence — n8n Workflow

An n8n automation workflow that performs multi-agent due diligence on startup documents.

## Overview

This workflow accepts startup documents via a webhook and runs them through a parallel pipeline of specialized AI agents, then consolidates the results into a final investment memo.

## Agent Pipeline

| Agent | Purpose |
|---|---|
| **KPI & Financials Agent** | Extracts financial metrics, unit economics, burn rate, runway, and flags inconsistencies |
| **Traction & Team Agent** | Evaluates founder backgrounds, customer traction, growth trajectory, and key wins |
| **Market Map Agent** | Maps TAM/SAM/SOM, competitive landscape, differentiation, and market timing |
| **Red Flags Agent** | Identifies critical risks, legal concerns, contradictions, and missing information |
| **Audit Agent** | Cross-verifies all agent outputs, assigns confidence scores, and issues a final investment recommendation (Pass / Watch / Fail) |

## Setup

1. Import `Startup Diligence 4_16.json` into your n8n instance.
2. Create and connect the following credentials in n8n:
   - **OpenAI API** — used by all 5 LLM nodes (GPT-4o)
   - **Gmail OAuth2** — used to email the final investment memo
   - **Telegram API** — used to send the approval notification
3. Update the `sendTo` email address in the **Send Diligence Report** Gmail node.
4. Update the Telegram nodes with your `chatId`.
5. Activate the workflow and POST startup documents to the `/diligence` webhook endpoint.

## Input Format

```json
{
  "companyName": "Acme Corp",
  "companyId": "acme-001",
  "documents": [
    { "type": "pitch_deck", "content": "..." },
    { "type": "financials", "content": "..." }
  ]
}
```

## Output

1. **Email** — the full investment memo is sent to the configured Gmail address, including the final memo, recommendation, diligence score, audit findings, confidence scores, and any contradictions or flags.
2. **Telegram** — a notification is sent after the email, prompting a double-approval review.

## Notes

- All credential IDs, webhook IDs, Telegram chat IDs, and instance IDs have been redacted in the exported JSON. Replace the `YOUR_*` placeholders with your own values after importing.
