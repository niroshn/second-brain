# Agent States

This folder is the persistent memory for the org's AI bots (Grok or any other agent). Each bot keeps **one state note** here so a new session starts with full context already loaded — no re-explaining, minimal token usage.

## Protocol (every bot, every session)

1. **Start of session:** read your state note first, then the notes it links to (member note, SOPs, trackers). Do not ask humans for context that is already written down.
2. **During the session:** work from task notes and SOPs. Link to them; do not paste large content into chat.
3. **End of session:** update your state note — current focus, active tasks, decisions made, blockers — and add one line to the session log with the date.
4. **Size limit:** keep each state note under one page. If it grows, move detail into a task note and leave only the link here.
5. **No secrets:** never store API keys, passwords, OTPs, or customer secrets in a state note. Reference where to find them instead.

## States

| Agent | Team | State | Last session |
|---|---|---|---|
| Customer Support AI Bot | Customer Support Operations | [[Customer Support AI Bot State]] | 2026-10-31 |
| SEO/AEO Architect | Figuro Marketing | [[SEO AEO Architect State]] | 2026-10-31 |
| Lead Gen Bot | Figuro Marketing | [[Lead Gen Bot State]] | 2026-10-31 |
| Outreach Bot | Figuro Marketing | [[Outreach Bot State]] | 2026-10-31 |
| Tech Lead Bot | Engineering (cross-project, reports to Chief) | [[Tech Lead Bot State]] | 2026-10-31 |
| Shopify AI Developer | Figuro Development (reports to Tech Lead Bot) | [[Shopify AI Developer State]] | 2026-10-31 |

Create a new state note from [[99-Templates/Agent State Template]] when adding a bot.
