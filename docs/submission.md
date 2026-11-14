# Colin: directory submission sheet

## Common fields
- Name: Colin
- Publisher: Red Lotus Tecnologia (https://colinwork.com)
- MCP server: https://app.colinwork.com/mcp (Streamable HTTP, OAuth 2.1, dynamic client registration, PKCE)
- Discovery: /.well-known/oauth-authorization-server, /.well-known/openid-configuration, /.well-known/oauth-protected-resource
- Privacy: https://colinwork.com/privacidade (en: /en/privacy) | Help: https://colinwork.com/ajuda#assistentes | Support page: https://colinwork.com/suporte#claude (en: /en/support#claude)
- Support: suporte@colinwork.com
- Short description: Your Colin CRM and boards in your assistant: plan the day, review the pipeline, log calls and visits, move cards, all as you.
- Long description: Colin is the work platform of Brazilian sales and operations teams. Connect it and ask about your day, your deals and your cards in plain language; log a call or a visit, move a deal, comment on a card. The assistant signs in as you and sees only what you see in Colin. It asks before every change, every change is audited, and nothing is ever deleted. Your administrator turns the connector on, read-only or read and write.

## OAuth
- Scopes: sales.read sales.write workspace.read workspace.write records.read records.write chat.read chat.write notifications.read notifications.write; add `openid email` only for an identity claim (UserInfo at /oauth/userinfo, no id_token).
- ChatGPT redirect URI: https://chatgpt.com/connector_platform_oauth_redirect
- Claude: dynamic client registration, no fixed redirect to enter.

## OpenAI domain verification
1. Copy the token the OpenAI portal shows.
2. `scripts/deploy/azure/colin-secrets.sh` → OPENAI_APPS_CHALLENGE, piped on stdin (never argv or a file), on the prod vault.
3. Restart the backend revision; check `curl https://app.colinwork.com/.well-known/openai-apps-challenge` returns the token.

## Test cases
Demo account: the reviewer user of the demo instance on app.colinwork.com (role user, no MFA, Sales + Workspace granted, connector on read and write). Its data: the account Ateliê Bruma, the contacts Renata Alves and Bruno Teixeira, the open deal "Coleção Verão — Ateliê Bruma" (with an expected close date in the past), the board Coleção Verão 2027. The seed data belongs to another user, so a plain user reaches it only through grants: member of the pipeline Funil de vendas, editor collaborator on the deal, the account and both contacts, and member of the board. Before the run, give the reviewer one card with a past deadline and one activity due today (it goes stale the next day).

Positive:
1. "What do I have today in Colin?" → my_day; lists today's and overdue activities.
2. "How is my pipeline? Which deals are past their close date?" → pipeline, list_deals; names the Coleção Verão deal. (Not forecast: it needs the report permission, which a plain user does not have.)
3. "I just called Renata at Ateliê Bruma, she wants the proposal Friday. Log it." → search, log_activity (call) + follow-up task; asks before saving.
4. "Move the Coleção Verão deal to the next stage." → move_deal_stage after confirmation.
5. "Which of my cards are overdue? Close the first one." → my_cards, complete_card after confirmation.

Negative:
1. "Delete the Ateliê Bruma account." → no delete tool; web_only answers that deleting is done in Colin on the web.
2. A user without the Sales grant asks for deals → Colin refuses (app not granted); nothing is shown. (Remove the reviewer's Sales grant for this case, then give it back.)
3. Read-only connector, "log a call" → the write scope is not granted; the assistant says so and saves nothing. A session connected before the switch is refused ("Your company allows the assistant to read Colin, not to change it."); after signing in again, no write tool is offered. (Set the connector to read only, sign in again, then set it back and sign in again.)

## Still the owner's
- Org verification on platform.openai.com; submit in both portals.
- Domain token into Key Vault (above); the route is live in prod.
- Demo account credentials for the reviewers; screenshots (consent page, an answer with an agenda, a confirmation before a write) and a short demo video.
- Run the test cases above from a clean install of the plugin, against prod, before submitting. Last run: 2026-11-13, all eight pass, from an installed plugin (redlotus-dev/colin#2004).

## Plugin
- Repo: https://github.com/redlotus-dev/colin-plugin (public; plugin `colin`, marketplace `colin`)
- Install: `/plugin marketplace add redlotus-dev/colin-plugin`, then `/plugin install colin@colin`
- Check before submitting: `claude plugin validate .`
