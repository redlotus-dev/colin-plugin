---
name: review-my-pipeline
description: Review the user's open Sales deals in Colin — the pipeline by stage, the forecast, deals with no next activity or past their expected close — and move stages when the user decides. Use when the user asks how their pipeline or forecast looks, which deals are stuck, or to prepare a pipeline review.
---

# Review my pipeline

1. Call `list_pipelines` if the user has several; ask which one, or use the one they name.
2. Call `pipeline` (with `pipeline_id`) for the deals by stage and `forecast` for the weighted totals. The amounts are in the tenant's base currency: say so once.
3. Call `list_deals` with `mine: true`, `outcome: "open"` and sort by expected close to find:
   - deals past their **expected close date**;
   - deals with **no next activity** (check with `list_activities` filtered by `deal_id` and `status` open for the deals you flag, a handful at most).
4. Present: totals per stage, the forecast, then the flagged deals with the reason each is flagged. Keep it to what the tools returned.
5. When the user decides on a deal:
   - a new stage → `move_deal_stage` with `stage_name` or `stage_id` and the `version` you read;
   - **lost** needs a reason: read `list_lost_reasons` and ask the user to choose one; never pick it yourself;
   - a next step → `log_activity` of kind `task` (or the kind they say) with `due_at`, linked through `deal_ids`.
6. End with the changes made, one line each.

Do not change amounts, owners or line items from this skill. Colin refuses a deal the user cannot reach; report the refusal as it came.
