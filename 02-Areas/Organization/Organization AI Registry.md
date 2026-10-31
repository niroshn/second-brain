---
title: Organization AI Registry
type: organization-config
schema_version: "1.0"
updated: 2026-10-31
status: template
chief_id: chief-001
routing_policy: department-then-team-then-lead
---

# Organization AI Registry

> [!IMPORTANT]
> This is the canonical organization directory for humans and AI agents. Fill in `TBD` values. IDs must remain unique and stable. An AI must not assume a person/bot is active, available, or authorized merely because they appear here.

## Reporting hierarchy

`Chief → Department → Team → Team Lead → Team Members (Human | Bot)`

- **Chief:** strategy, cross-department priorities, final escalation.
- **Department:** functional ownership and routing.
- **Team Lead:** accepts and decomposes work, assigns tasks, reviews outputs, escalates blockers.
- **Team Member:** human or AI bot responsible for assigned work.
- **Shared services:** SEO/AEO, lead generation, and development may serve multiple teams; each must still have one primary reporting team and lead.

## AI operating rules

1. Route incoming requests to the relevant **department**, then **team**, then **team lead**.
2. Only the designated lead assigns work to members unless explicit delegation is recorded.
3. Select a member by `skills`, `availability`, `access`, and `task_types`; never by name alone.
4. AI bots may draft, analyze, test, and recommend within their listed permissions. Actions affecting money, customer communications, production deployments, deletion, or external publication require the approval specified in `approval_required_for`.
5. Human and bot members use the same task lifecycle: `backlog → ready → in_progress → review → done` (or `blocked` / `cancelled`).
6. Every task must include `task_id`, `team_id`, `assignee_id`, `reviewer_id`, `priority`, `due_date`, `status`, and a link to its project note.
7. If no lead/member is assigned, mark the task `unassigned`; do not invent an owner.
8. Escalate member blockers to the team lead; cross-team conflicts to the Chief (or a named department head, if configured).
9. Do not expose secrets in this file. Reference secret vault paths or access-request procedures only.
10. Changes to reporting lines, permissions, and approval policies require Chief approval.

## Chief

```yaml
id: chief-001
name: TBD
kind: human # human | bot
role: Chief
status: active # active | inactive | planned
contact: TBD
responsibilities: [strategy, prioritization, approvals, escalation]
```

## Departments and teams

Each team has exactly one primary `lead_id`. A lead may be a human or bot. Add a department head if needed, without changing the Chief → Team Lead → Member responsibility chain.

```yaml
departments:
  - id: dept-marketing
    name: Marketing
    department_head_id: TBD
    teams:
      - id: team-figuro-marketing
        name: Figuro Marketing
        lead_id: TBD
        mission: Grow Figuro traffic, conversions, and sales
        projects: [Figuro]
        capabilities: [paid_ads, organic_social, content, email_marketing]
      - id: team-personal-branding
        name: Personal Branding
        lead_id: TBD
        mission: Grow personal authority and audience
        projects: [Personal Branding]
        capabilities: [linkedin, content, public_relations]
      - id: team-nocensor-marketing
        name: NoCensor AI Marketing
        lead_id: TBD
        mission: Market all image and video generation products
        projects: [NoCensor AI]
        capabilities: [product_marketing, campaigns, creative, growth]
      - id: team-seo-aeo
        name: SEO / AEO
        lead_id: TBD
        mission: Search and answer-engine visibility across products
        projects: [Figuro, NoCensor AI]
        capabilities: [technical_seo, content_seo, structured_data, aeo]
      - id: team-lead-gen
        name: Lead Generation
        lead_id: TBD
        mission: Identify and qualify potential customers and partners
        projects: [Figuro, NoCensor AI]
        capabilities: [prospecting, research, lead_qualification, outreach_drafting]

  - id: dept-engineering
    name: Engineering
    department_head_id: TBD
    teams:
      - id: team-figuro-dev
        name: Figuro Development
        lead_id: TBD
        mission: Build and maintain Figuro digital products and integrations
        projects: [Figuro]
        capabilities: [shopify, frontend, backend, integrations, qa]
      - id: team-nocensor-dev
        name: NoCensor Development
        lead_id: TBD
        mission: Develop all image/video generation products
        projects: [NoCensor AI]
        capabilities: [frontend, backend, ai_inference, video_generation, infrastructure, qa]
      - id: team-3d-modeling
        name: 3D Modeling
        lead_id: TBD
        mission: Produce and quality-check printable 3D assets
        projects: [Figuro]
        capabilities: [sculpting, mesh_cleanup, modeling, print_readiness, qa]
      - id: team-shared-development
        name: Shared Development
        lead_id: TBD
        mission: Shared engineering tools, automation, and platform services
        projects: []
        capabilities: [automation, devops, shared_services, testing]

  - id: dept-customer-support
    name: Customer Support
    department_head_id: TBD
    teams:
      - id: team-customer-support
        name: Customer Support Operations
        lead_id: TBD
        mission: Handle customer questions, order updates, and escalations
        projects: [Figuro, NoCensor AI]
        capabilities: [customer_inquiries, order_tracking, returns_escalation, multilingual_support]

  - id: dept-personal-assistant
    name: Personal Assistant
    department_head_id: TBD
    teams:
      - id: team-executive-assistant
        name: Executive Assistant
        lead_id: TBD
        mission: Organize priorities, calendar, follow-ups, and Chief briefings
        projects: [Executive Operations]
        capabilities: [planning, reminders, research, reporting, coordination]
```

## Member registry

Duplicate this record for every **human** or **bot**. `reports_to` must match a team `lead_id` (unless the member is itself the lead). For team leads, use `role: team_lead` and `reports_to: chief-001` or the configured department head.

```yaml
members:
  - id: member-example-001
    name: TBD
    kind: bot # human | bot
    role: specialist # team_lead | specialist | qa | developer | designer | coordinator
    department_id: dept-marketing
    team_id: team-seo-aeo
    reports_to: TBD
    status: planned # planned | active | inactive
    availability: TBD # e.g. weekdays 09:00-18:00 SGT | on_demand
    timezone: Asia/Singapore
    contact_or_agent_ref: TBD # Slack handle, Obsidian note, agent config path, etc.
    skills: [seo, keyword_research]
    task_types: [audit, research, report]
    tools: []
    access: []
    can_delegate: false
    can_approve: false
    approval_required_for: [external_publish, customer_send, spend, deploy, delete]
    backup_member_id: TBD
    notes: TBD
```

## Task template

Create task notes under `Projects/<Project>/Tasks/` or `Operations/Tasks/`. Use the same frontmatter for human and bot assignees.

```yaml
---
task_id: TASK-0001
title: TBD
project: Figuro
department_id: dept-engineering
team_id: team-figuro-dev
requested_by: chief-001
assigned_by: TBD
assignee_id: TBD
reviewer_id: TBD
priority: medium # low | medium | high | urgent
status: backlog # backlog | ready | in_progress | blocked | review | done | cancelled
due_date: YYYY-MM-DD
approval_status: pending # pending | approved | rejected | not_required
related_notes: []
---
```

### Task details

**Objective:** TBD  
**Acceptance criteria:** TBD  
**Dependencies:** TBD  
**Deliverables:** TBD  
**Activity log:** TBD  
**Review result:** TBD

## Delegation examples

| Request | Primary team | Review / collaboration |
|---|---|---|
| Figuro Facebook ad campaign | Figuro Marketing | SEO/AEO if landing-page changes |
| Figuro Shopify bug | Figuro Development | QA member in development team |
| NoCensor AI video feature | NoCensor Development | NoCensor AI Marketing for launch |
| 3D model cleanup | 3D Modeling | Assigned human QA / lead |
| SEO audit across products | SEO / AEO | Product-specific marketing and dev leads |
| New partnership prospects | Lead Generation | Relevant marketing team lead |
| Customer order inquiry | Customer Support Operations | Figuro Dev or 3D Modeling if needed |
| Daily Chief briefing | Executive Assistant | All team leads |

## Team note template

Create one note per team in `02-Areas/Organization/Teams/`:

```yaml
---
team_id: team-figuro-dev
department_id: dept-engineering
lead_id: TBD
member_ids: []
active_projects: [Figuro]
status: active
weekly_review_day: Monday
---
```

### Mission
TBD

### Responsibilities
- TBD

### Members
- [[Member Name]] — human/bot — role — status

### Current priorities
- TBD

### KPIs
- TBD

### Escalation path
Member → Team Lead → Chief

## Recommended Obsidian vault layout

```text
00-Inbox/
01-Projects/
  Figuro/
    Tasks/
  NoCensor-AI/
    Tasks/
  Personal-Branding/
    Tasks/
02-Areas/
  Organization/
    organization-ai-template.md   # This file
    Departments/
    Teams/
    Members/
  Marketing/
  Engineering/
  Customer-Support/
  Personal-Assistant/
03-Resources/
  SOPs/
  Agent-Prompts/
  Templates/
04-Archive/
Operations/
  Tasks/
  Daily-Reports/
  Weekly-Reviews/
```

## Setup checklist

- [ ] Set Chief identity
- [ ] Confirm department heads (optional)
- [ ] Set one lead for each team
- [ ] Create one member note per human or bot
- [ ] Fill permissions and tool access for each bot
- [ ] Assign shared SEO/AEO, lead generation, and development ownership
- [ ] Link active projects and tasks
- [ ] Define approval and escalation contacts
- [ ] Review structure weekly
