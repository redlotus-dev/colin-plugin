---
name: triage-my-cards
description: Go through the user's Colin Workspace cards — overdue, due soon, waiting on them — and move, complete, comment on or re-date them as the user decides. Use when the user asks what is on their plate in Workspace, wants to clean up their cards or boards, or asks for a status of a card.
---

# Triage my cards

1. Call `my_cards` (`due` and `days: 14`; `include_watching: true` only when the user asks about cards they follow). For one card the user names, `search_cards` then `get_card` with `comments: true`.
2. Group what came back:
   - **Overdue** (past the estimated deadline);
   - **Due soon**;
   - **No deadline** or **stalled** (in the same list a long time, by the dates the card shows).
   Show board, list, title and deadline; nothing the tools did not return.
3. Walk the groups with the user, a few cards at a time. For each decision:
   - finished → `complete_card` (Colin refuses it while a required field is empty: say which and ask for the value);
   - next step → `move_card` with `list_name` or `list_id`;
   - new date or priority → `update_card` (`estimated_deadline`, `priority`);
   - a note for the others → `comment_on_card` with a fresh `client_id`; `mention` only people the user named.
   Send the `version` you read; when Colin says the card changed, read it again and show the user before retrying.
4. Finish with a summary of what moved, closed or changed.

Never delete or archive a card, change its members or owner, or create cards here unless the user asks plainly; `create_card` and `transfer_card_owner` are for an explicit request.
