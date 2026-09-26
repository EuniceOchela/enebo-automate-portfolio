# Task 1: Client Onboarding & Follow-Up Automation

**Niche:** Customer Support & CRM Automation | Client Onboarding & Workflow Systems  
**Tools used:** n8n, HubSpot CRM, Gmail, Tally

## The Challenge

A business coach (simulated client: Amara O.) needed a way to automatically onboard new clients the moment they signed — without manually sending welcome emails, booking links, and intake forms one by one. She was also losing track of leads between initial inquiry and becoming a paying client, since everything lived in scattered forms and her personal inbox.

## The Action

I built a two-part automation in n8n connected to HubSpot:

**1. Lead Capture Workflow**
- A Tally form captures new leads (name, contact info, coaching interest, biggest challenge)
- A webhook triggers n8n to automatically create or update the lead as a HubSpot contact
- n8n then creates a linked deal in HubSpot's Sales pipeline, correctly associated with that contact

**2. Client Onboarding Workflow**
- Runs on a schedule, checking HubSpot every 15 minutes for deals that just moved to "Closed Won"
- Filters specifically for deals that: (a) are in the Closed Won stage, (b) haven't already received a welcome email, and (c) have a properly linked contact
- Pulls the client's real name and email from their linked HubSpot contact record
- Sends a personalized welcome email via Gmail, including a client onboarding form link and a booking link for the kickoff call
- Marks the deal as "onboarding email sent" in HubSpot, so the automation never emails the same client twice

## The Result

- **Zero manual work** between a client signing and receiving their welcome sequence
- **Built-in duplicate prevention** — a custom HubSpot property (`onboarding_email_sent`) ensures each client is emailed exactly once, even though the workflow checks every 15 minutes
- **Fully dynamic** — the automation works for any client, not just the test case; no hardcoded names or emails anywhere in the workflow
- **Error-resistant** — added a safeguard so deals without a linked contact are automatically skipped instead of breaking the workflow

## Files in this folder

- `lead-capture-workflow.json` — n8n export of the Tally → HubSpot lead capture flow
- `onboarding-workflow.json` — n8n export of the Closed Won → welcome email flow
