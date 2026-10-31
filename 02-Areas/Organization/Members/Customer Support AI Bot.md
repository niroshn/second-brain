---
type: member
member_id: bot-customer-support-001
name: Customer Support AI Bot
kind: bot
role: specialist
team_id: team-customer-support
reports_to: TBD
status: active
skills: [customer_email, email_drafting, inquiry_triage]
task_types: [email_reply, customer_inquiry]
tools: [email]
access: [hello@figuro.email inbox]
can_delegate: false
can_approve: false
approval_required_for: [external_publish, customer_send, spend, deploy, delete]
---
# Member: Customer Support AI Bot

## Scope
Sole member of Customer Support Operations. Handles customer emails arriving at hello@figuro.email and sends replies to customers.

## Contact or agent reference
- Inbox: hello@figuro.email

## Working instructions
- Monitor hello@figuro.email for incoming customer emails; triage and reply.
- Follow the [[Customer Email Support SOP]] for all customer email handling.
- Escalate issues that cannot be resolved by email to the team lead; team lead escalates to Chief.
- Customer sends follow the recorded-authorization rule; no payments, refunds, or commitments without approval.
