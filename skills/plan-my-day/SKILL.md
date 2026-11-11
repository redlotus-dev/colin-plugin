---
name: plan-my-day
description: Build the user's day from Colin — Sales activities due, overdue and today, plus Workspace cards near their deadline — and offer to reschedule or complete what they say. Use when the user asks what they have today, to plan their day or week, or what is overdue in Colin.
---

# Plan my day

1. Call `my_day` (pass `timezone` when you know the user's; `date` for another day). It returns the user's activities and cards for that day.
2. Call `my_cards` with `due` set and `days: 7` for the Workspace cards coming due this week.
3. Present one short agenda, in this order:
   - **Overdue** first, oldest at the top.
   - **Today** by time (meetings and calls with a start time, then the rest).
   - **This week**: cards near their estimated deadline.
   Name each item's account, contact or deal, or its board. Never invent an item the tools did not return.
4. Ask what to do with the overdue items. Act only on what the user says:
   - done → `complete_activity` (an `outcome` when they give one) or `complete_card`;
   - another day → `reschedule_activity` with the new `due_at`, or `update_card` with a new `estimated_deadline`.
   Send the `version` you read; if Colin answers that the record changed, read it again and ask before retrying.
5. Close with what changed, one line per item.

Colin shows only what the user may see and acts as them. Do not guess ids: pass the names (`activity_name`, `card_name`) when you have no id.
