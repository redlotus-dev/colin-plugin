# Colin: directory submission sheet

## Common fields
- Name: Colin
- Publisher: Red Lotus Tecnologia (https://colinwork.com)
- MCP server: https://app.colinwork.com/mcp (Streamable HTTP, OAuth 2.1, dynamic client registration, PKCE)
- Discovery: /.well-known/oauth-authorization-server, /.well-known/openid-configuration, /.well-known/oauth-protected-resource
- Privacy: https://colinwork.com/privacidade (en: /en/privacy) | Help: https://colinwork.com/ajuda#assistentes
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

## Test cases (demo account: a seeded tenant, no MFA, Sales + Workspace granted, read and write)
Positive:
1. "What do I have today in Colin?" → my_day; lists today's and overdue activities.
2. "How is my pipeline? Which deals are past their close date?" → pipeline, forecast, list_deals.
3. "I just called Ana at Fecchio, she wants the proposal Friday. Log it." → search, log_activity (call) + follow-up task; asks before saving.
4. "Move the Fecchio deal to Proposal." → move_deal_stage after confirmation.
5. "Which of my cards are overdue? Close the brochure card." → my_cards, complete_card.
Negative:
1. "Delete the Fecchio account." → no delete tool; the assistant says Colin only archives, on the web.
2. A user without the Sales grant asks for deals → Colin refuses (app not granted); nothing is shown.
3. Read-only connector, "log a call" → the write scope is not granted; the assistant says so and saves nothing.

## Still the owner's
- Org verification on platform.openai.com; submit in both portals.
- Domain token into Key Vault (above), then release this branch to prod.
- Demo account credentials for the reviewers; screenshots (consent page, an answer with an agenda, a confirmation before a write) and a short demo video.
- Public repo for the plugin (e.g. redlotus-dev/colin-plugin) with a marketplace.json; then add `/plugin install colin --marketplace <owner>/<repo>` to the README.
