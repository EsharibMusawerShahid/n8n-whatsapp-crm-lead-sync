# WhatsApp-to-CRM Lead Sync (n8n Workflow Template)

A ready-to-import n8n workflow that catches an incoming WhatsApp Business API message, pulls out the lead's name and intent with an LLM step, and pushes a clean record into your CRM — instead of a lead sitting unread in a WhatsApp inbox.

## What it does

1. **Trigger**: WhatsApp Business API webhook receives an inbound message
2. **Extract**: an LLM node parses the free-text message into structured fields (name, intent, urgency)
3. **Dedupe**: checks the CRM for an existing contact by phone number before creating a duplicate
4. **Push**: creates or updates the CRM record (built for HubSpot; swap the node for Zoho/Salesforce/Pipedrive — the mapping step is the same)
5. **Alert**: posts a Slack/email notification if the parsed urgency is "high"

## Why this exists

This is the single most common automation request we see: a form or WhatsApp number that doesn't sync to a CRM, so leads either sit unread or someone copy-pastes them manually. This template is the minimum real version of that fix — two systems connected, one judgment call (urgency) handled by an LLM step instead of a brittle keyword match.

## Setup

1. Import `workflow.json` into your n8n instance
2. Add your WhatsApp Business API and CRM credentials in n8n's credential manager
3. Update the CRM field mapping in the "Push to CRM" node to match your CRM's actual field names
4. Set your urgency-alert threshold in the "Check urgency" node

## Requirements

- n8n (self-hosted or cloud)
- WhatsApp Business API access
- A CRM with a public API (HubSpot, Zoho, Salesforce, Pipedrive, etc.)
- An LLM API key (OpenAI, Anthropic, or any n8n-supported provider)

## Notes on production use

- Add retry logic on the CRM push step — API rate limits are the most common failure point
- Log every parsed message somewhere before the CRM push, so a bad parse doesn't silently lose a lead
- Test with real messy input (typos, mixed languages, voice-note transcripts) before relying on the LLM extraction in production

---

Built and maintained by [AleyaAI](https://www.aleyaai.com/n8n-automation-agency) — we build custom automation workflows like this one for clients. Issues and PRs welcome.
