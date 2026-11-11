---
name: log-a-call-or-visit
description: Record a call, meeting, visit, WhatsApp or e-mail the user just had as a Colin Sales activity, linked to the right account, contact and deal, with its outcome and a follow-up. Use when the user says they talked to, met or visited a customer and wants it logged in Colin, or dictates notes from a conversation.
---

# Log a call or visit

1. Find the records. Call `search` with the company or person the user named (`types` narrowed when obvious). If several match, list them with what tells them apart (city, CNPJ, owner) and ask. Never create an account or contact from this skill.
2. Pick the kind: `call`, `meeting`, `visit`, `whatsapp` or `email` (`task` only for a follow-up). If the tenant has its own types, `list_activity_types` with that `kind` gives them and their outcomes; use an outcome only when the user's words match one, otherwise ask.
3. Draft the activity and show it before sending: title, kind, when it happened (`occurred_at`; ask if unsure, never assume now for something in the past), the records it links, the outcome and the notes in the user's own words, tidied, nothing added.
4. On the user's yes, call `log_activity` with a fresh `client_id` (a UUID) so a retry never logs it twice, the ids from step 1 (`account_ids`, `contact_ids`, `deal_ids`), `notes`, `outcome` or `outcome_id`.
5. Offer the follow-up the conversation implies (send a proposal, call back on Friday): a second `log_activity` of kind `task` with its `due_at`. If the deal moved, offer `move_deal_stage`; a Lost stage needs a reason the user picks from `list_lost_reasons`.
6. Long free-form context that is not an interaction goes in `log_note` instead.

Personal data (CPF, phone, e-mail) goes only where the user put it. Confirm what was saved, with the record names.
