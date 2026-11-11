# Colin for Claude

Colin is a platform for Brazilian companies: Colin Sales (a B2B and B2C CRM) and Colin Workspace (Kanban boards for how a team works), with chat, notifications and shared records for accounts, contacts and products.

This plugin connects Claude to your Colin instance at `https://app.colinwork.com/mcp` and adds four skills:

| Skill | What it does |
|---|---|
| `plan-my-day` | Reads your activities and cards for today and orders them. |
| `review-my-pipeline` | Looks at your open deals and points out the stuck, late and unprotected ones. |
| `log-a-call-or-visit` | Turns your notes on a call or visit into the activity, the next step and any deal change. |
| `triage-my-cards` | Goes through your Workspace cards and proposes what to close, move or chase. |

## Install

In Claude Code:

```
/plugin install colin --marketplace redlotus-dev/colin-plugin
```

In Claude (web, desktop): add Colin from the connector directory, or add the custom connector `https://app.colinwork.com/mcp`.

## Before you start

- Your company's Colin administrator turns the assistant connector on, under **Admin → Assistant connector**, as Read only or Read and write.
- You need a grant for Colin Sales, Colin Workspace, or both. Claude sees only what you see in Colin.

## Signing in

The first time Claude calls Colin, a browser opens Colin's own sign-in page. Sign in with your e-mail and password, or with Google or Microsoft, then choose what Claude may read and change. You can end the session at any time in Colin under **Connected devices**, in your account menu.

## How it acts

Every call is made as you, with your role, permissions and visibility. Claude asks before every change. Changes that overwrite, archive or remove something are marked destructive, and each one is written to Colin's audit log under your name. Nothing is deleted outright: deals are closed as lost, and records are archived.

## Support

[colinwork.com](https://colinwork.com) · suporte@colinwork.com

Privacy policy: https://colinwork.com/privacidade
