---
type: agent-state
agent_id: bot-tech-lead
name: Tech Lead Bot
team_id: TBD
status: active
last_session: 2026-10-31
sessions_total: 0
---
# Agent State: Tech Lead Bot

## Focus now
Standing up the daily engineering routine across the four projects: Figuro Shopify, Figuro OMS, NoCensor Web, NoCensor Analytics.

## Active tasks
- Run daily routine phases A–E per system prompt §8: chat review → project health → delegation → quality review → daily report.
- Convert Figuro Lead Team chat requests into Linear issues (check for duplicates first).

## Pending / blockers
- Connectors to confirm: Figuro Lead Team chat read access, Linear API, repository access, scheduler for 9:00 AM SGT runs.
- AI developer agent roles (Shopify, Frontend, Backend, OMS, Analytics, QA, DevOps, Code Review) are defined but not yet configured — no execution interfaces yet.

## Decisions made (do not re-litigate)
- Linear is authoritative; chat messages are the primary requirement source.
- No issue marked Done without system-confirmed verification.

## Working context
- System prompt: [[Tech Lead Bot - System Prompt]] (issue format §4, priority rules §9, governance §12).
- Project names are authoritative: Figuro Shopify, Figuro OMS, NoCensor Web, NoCensor Analytics — do not invent repo URLs or Linear IDs (known repos: [[Repositories]]).
- Vault knowledge: [[Figuro Shopify Theme - Knowledge Transfer]] for the Shopify project.
- Member note: [[Tech Lead Bot]].

## Session log
- 2026-10-31 — State created; awaiting connector configuration.
