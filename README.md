# Hypamoon MCP server

[![Hypamoon MCP connector, tool definition quality and endpoint health on Glama](https://glama.ai/mcp/connectors/com.hypamoon/hypamoon/badges/score.svg)](https://glama.ai/mcp/connectors/com.hypamoon/hypamoon)

**The cloud your AI builds in.**

Hypamoon is a hosted MCP server. Connect it once to Claude, ChatGPT, Cursor, VS Code or Copilot, and your chat can put what it builds live: a site on its own address, an app with sign-in and a database, forms that save somewhere, payments, email from your own domain, a phone number with voice AI, and a connection to the software your business already uses.

There is nothing to install. The server runs at Hypamoon; this repository holds the public listing: this README, the `server.json` we publish to the official MCP Registry, and how to report a security issue.

```text
https://api.hypamoon.com/api/v1/mcp
```

Transport: Streamable HTTP. Sign-in: OAuth 2.1 (the apps register themselves) or an API key. Registry name: `com.hypamoon/hypamoon`.

## What your chat can do with it

- **Sites.** Publish a site at `<name>.hypamoon.app` or on your own domain, change one file, roll back to an earlier version, put a password on it, or keep it unlisted. Import an existing site or a WordPress site.
- **Apps.** Server code next to a site, with sign-in for your users, roles, uploads and a database per app. Functions on their own address for webhooks and small APIs, and containers for a full app or server.
- **Data and forms.** Collections of JSON documents for form entries, bookings and orders, with a working form snippet and an email or webhook on every new entry.
- **Payments.** A checkout, a cart, discount codes, gift cards, invoices, paid access to a site and subscriptions, paid into your own payment account. Hypamoon never holds the money.
- **Email.** Send from your own domain, receive on an address of your own, and run templates, automations and sequences.
- **Domains.** Find and register a domain, connect or transfer the one you have, and manage DNS. Your existing mailbox keeps working.
- **Phone, SMS and chat.** A number that rings your team first and lets the voice AI pick up when nobody can, SMS to your customers, and a chat on your site. Which numbers you can order depends on your country; `search_numbers` shows what is available.
- **Adapters on your own software.** Your chat writes an adapter for the system you already use (accounting, a CRM, a shop), with your key in a vault. Agents and chats then read from it under rules you allow, and every write waits for your approval.
- **Agents.** Agents that run on a schedule, with approvals for what they do.

## Connect

Start with your test project. In test mode nothing costs money, numbers are test numbers and calls are simulated. Switch to your live project when it works.

### Claude (claude.ai and Claude Desktop)

[Add to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Hypamoon&connectorUrl=https%3A%2F%2Fapi.hypamoon.com%2Fapi%2Fv1%2Fmcp) opens the form with everything filled in. Or by hand: Customize, Connectors, Add custom connector. Name it `Hypamoon`, paste the MCP URL and leave the OAuth fields empty. Choose Connect and sign in.

### Claude Code

```bash
claude mcp add --transport http hypamoon https://api.hypamoon.com/api/v1/mcp
```

Then type `/mcp`, pick `hypamoon` and choose Authenticate. Your browser opens and you sign in.

### ChatGPT

Hypamoon is in the ChatGPT plugin directory: open [Hypamoon in ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6ac39fd9fbf08191a4eab9c4aedcc198), click Install plugin and sign in.

Not available in your country or plan? Add it yourself: Plugins, Add, Add custom MCP server. Name it `Hypamoon`, paste the MCP URL, choose OAuth and leave the client fields empty. In a work account an admin may have to allow custom MCP servers first. ChatGPT sees most tools; buying or transferring a domain, payments setup, refunds and secrets stay in the Hypamoon portal.

### Cursor

In `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "hypamoon": {
      "url": "https://api.hypamoon.com/api/v1/mcp"
    }
  }
}
```

Cursor opens the Hypamoon sign-in in your browser. Cursor shows about 40 tools per server; add `?tools=sites` (or another set, see below) to the URL to load only what you need.

### VS Code and GitHub Copilot

In `.vscode/mcp.json`, or with MCP: Add Server in the command palette:

```json
{
  "servers": {
    "hypamoon": {
      "type": "http",
      "url": "https://api.hypamoon.com/api/v1/mcp"
    }
  }
}
```

Start the server; VS Code opens the Hypamoon sign-in in your browser. Then use Copilot Chat in agent mode. VS Code stops at 128 tools, so a smaller set helps here too.

The GitHub Copilot coding agent cannot sign in with OAuth. Give it a test key, or a live key with only the scopes it needs, as a repository secret whose name starts with `COPILOT_MCP_`, and send it in the `Authorization` header.

### Microsoft Copilot (Copilot Studio and Microsoft 365 Copilot)

In Copilot Studio: Tools, Add a tool, New tool, Model Context Protocol. Paste the MCP URL and choose OAuth 2.0 with Dynamic discovery. For Copilot Chat and Teams there is a ready-made declarative agent package. Step by step: https://hypamoon.com/docs/microsoft-copilot

### Other apps

Grok, Le Chat, Gemini CLI, Cline, Continue, LM Studio, Open WebUI and the model APIs use the same address. Steps per app: https://hypamoon.com/connect

## Signing in

**OAuth (the chat apps and editors).** You paste the URL, the app sends you to a Hypamoon consent screen, and there you choose which project the app may use and what it may do. Every connected app is listed in the portal with when it was last used, and you can revoke it there. It stops working right away.

**API key (scripts, CI, your own app).** Create a key in the portal and send it as `Authorization: Bearer hm_test_...` or `hm_live_...`. We only store a hash of it. Keep it out of the chat and out of browser code.

For client builders: OAuth 2.1 with PKCE, Protected Resource Metadata (RFC 9728), Authorization Server Metadata (RFC 8414), Dynamic Client Registration (RFC 7591), resource indicators (RFC 8707) and token revocation (RFC 7009). Protocol versions `2025-11-25`, `2025-06-18`, `2025-03-26` and `2024-11-05`. A static server card with every tool is at https://api.hypamoon.com/.well-known/mcp/server-card.json

**What it may do.** Every tool needs a scope, and a key or connection only sees the tools its scopes allow:

| Scope | What it allows |
|---|---|
| `project:read` | See the project, its balance and its agents. Always on. |
| `project:write` | Change company details, manage agents and secrets, propose adapter access. |
| `sites:read` / `sites:write` | See or publish sites and apps, and manage domains and media. |
| `data:read` / `data:write` | Read or write collections, orders, payments and adapters. |
| `numbers:read` / `numbers:write` | See numbers, or claim one and set who it rings. |
| `assistant:write` | Set up the voice AI. |
| `calls:read` / `calls:write` | Read calls and transcripts, or let the voice AI call someone. |
| `messages:read` / `messages:write` | Read or send SMS and email. |
| `chat:read` / `chat:write` | Read or run the chat on your site. |
| `webhooks:write` | Send events to your own systems. |

An owner can also limit a key or a connection to a few sites and apps. More: https://hypamoon.com/docs/authentication

## Tools

295 tools on 24 November 2026, in the groups below. What each one does, with its scope, is in the docs: https://hypamoon.com/docs/mcp

**Start, help and going live:** `get_project`, `get_capabilities`, `get_balance`, `set_company_details`, `get_status`, `ask_hypamoon`, `report_issue`, `get_recent_errors`, `get_advice`, `list_recipes`, `get_recipe`, `get_confirmation`, `propose_plan`, `move_to_live`, `undo_move_to_live`, `get_integration_code`

**Sites:** `deploy_site`, `create_upload_urls`, `create_drop_link`, `get_drop`, `list_sites`, `get_site`, `read_site_file`, `edit_site_file`, `fetch_site_page`, `delete_site`, `restore_site`, `suggest_site_address`, `update_site_address`, `rotate_site_secret_url`, `list_site_versions`, `get_site_diff`, `rollback_site`, `restore_site_files`, `promote_site_version`, `label_site_version`, `get_site_stats`, `verify_search_console`, `set_site_recipients`, `check_readiness`

**Media library:** `create_media_link`, `list_media`, `get_media`, `update_media`, `delete_media`

**Moving a site or WordPress:** `import_site`, `inspect_wordpress`, `connect_wordpress`, `get_wordpress_inventory`, `suggest_wordpress_selection`, `select_wordpress_import`, `import_wordpress`, `migrate_wordpress`, `set_wordpress_embeds`

**Apps (server code, users, files and database):** `deploy_app`, `create_app_from_template`, `list_apps`, `get_app`, `get_app_code`, `get_app_logs`, `rollback_app`, `delete_app`, `get_app_schema`, `apply_app_migration`, `query_app_db`, `set_table_owner_column`, `list_app_users`, `invite_app_user`, `import_app_users`, `set_app_user_role`, `set_app_user_email`, `remove_app_user`, `create_test_login_link`, `list_app_mails`, `preview_login_email`, `create_app_group`, `add_app_group_member`, `list_app_groups`, `list_app_files`, `delete_app_file`, `set_app_secret`, `list_app_secrets`, `delete_app_secret`, `export_app_user`, `delete_app_user_data`, `export_app_subject`, `delete_app_subject`

**Functions:** `deploy_function`, `list_functions`, `get_function`, `get_function_logs`, `rollback_function`, `delete_function`

**Containers:** `deploy_container`, `list_containers`, `get_container`, `get_container_logs`, `list_container_versions`, `rollback_container`, `restart_container`, `delete_container`

**Apps in the App Store and Google Play:** `check_store_readiness`, `generate_store_listing`, `get_privacy_answers`, `export_app_project`

**Data, forms and secrets:** `create_collection`, `list_collections`, `update_collection`, `delete_collection`, `insert_document`, `query_documents`, `update_document`, `delete_document`, `get_form_snippet`, `render_pdf`, `request_secret`, `list_secrets`, `delete_secret`

**Adapters on your own software:** `list_adapters`, `query_adapter`, `write_adapter`, `grant_adapter_access`, `revoke_adapter_access`

**Domains and DNS:** `check_domain`, `suggest_domains`, `register_domain`, `transfer_domain`, `connect_domain`, `activate_domain`, `get_domain`, `update_domain`, `disconnect_domain`, `diagnose_domain`, `preflight_domain`, `set_domain_auto_renew`, `get_domain_authcode`, `list_dns_records`, `set_dns_record`, `undo_dns_change`, `import_zone_file`, `switch_domain_nameservers`, `restore_domain_dns`, `watch_domain`, `list_domain_health`, `get_dns_cleanup`, `clean_up_dns`

**Email:** `send_email`, `send_email_batch`, `list_emails`, `get_email`, `add_email_domain`, `list_email_domains`, `get_email_domain`, `verify_email_domain`, `allow_email_recipient`, `check_email_address`, `get_email_deliverability`, `get_email_bimi`, `set_email_bimi`, `request_dedicated_ip`, `add_inbound_address`, `list_inbound_addresses`, `delete_inbound_address`, `list_inbound_emails`, `get_inbound_email`, `answer_inbound_email`, `delete_inbound_email`, `create_email_template`, `list_email_templates`, `update_email_template`, `delete_email_template`, `preview_email_template`, `create_email_automation`, `list_email_automations`, `update_email_automation`, `delete_email_automation`, `list_scheduled_emails`, `cancel_scheduled_email`, `create_email_sequence`, `list_email_sequences`, `delete_email_sequence`, `add_to_email_sequence`, `remove_from_email_sequence`

**Payments, shop and paid access:** `connect_payments`, `get_payments_status`, `list_payment_profiles`, `set_site_payment_profile`, `get_shop`, `configure_shop`, `configure_texts`, `get_checkout_snippet`, `get_cart_snippet`, `create_download_upload_urls`, `list_orders`, `get_order`, `update_order_status`, `export_orders`, `refund_order`, `get_refund_link`, `create_discount_code`, `list_discount_codes`, `update_discount_code`, `delete_discount_code`, `issue_gift_card`, `check_gift_card`, `redeem_gift_card`, `list_gift_cards`, `void_gift_card`, `create_invoice`, `get_invoice`, `get_payouts`, `check_business_readiness`, `create_access_product`, `set_site_access`, `list_access`, `revoke_access_pass`, `pause_subscription`, `resume_subscription`

**Marketplaces:** `configure_marketplace`, `connect_marketplace_payments`, `get_marketplace_rules`, `invite_seller`, `create_seller_onboarding_link`, `list_sellers`, `get_seller`, `suspend_seller`, `reinstate_seller`, `list_suborders`, `reverse_seller_transfer`

**Phone, SMS and chat:** `search_numbers`, `order_number`, `list_numbers`, `release_number`, `get_assistant`, `set_assistant`, `get_call_plan`, `set_call_plan`, `simulate_call`, `start_call`, `list_calls`, `get_call`, `list_call_events`, `record_call_consent`, `list_call_consents`, `revoke_call_consent`, `send_message`, `list_messages`, `get_message`, `start_conversation`, `send_chat_message`, `list_conversations`, `get_conversation`, `close_conversation`, `create_chat_widget`, `list_chat_widgets`, `update_chat_widget`, `revoke_chat_widget`

**Webhooks:** `create_webhook`, `list_webhooks`, `delete_webhook`, `enable_webhook`, `send_test_webhook`

**Agents:** `create_agent`, `list_agents`, `get_agent`, `update_agent`, `pause_agent`, `delete_agent`, `list_agent_runs`, `approve_agent_action`

**Work scan:** `rank_opportunities`, `start_team_scan`, `submit_work_scan`, `get_team_scan`, `get_work_scan_inventory`

**Agencies and client projects:** `create_client_project`, `list_client_projects`, `restore_client_project`, `invite_client`, `request_client_access`, `grant_client_access`, `list_client_access`, `revoke_client_relation`, `set_client_payer`, `set_client_budget`, `get_client_usage`, `get_client_activity`, `export_client_invoice_lines`, `create_payment_link`, `create_subscription`, `list_client_payments`, `cancel_client_payment_link`, `cancel_client_subscription`, `transfer_project`, `cancel_project_transfer`

### A smaller set of tools

Some editors limit the number of tools per server. Add `?tools=` to the MCP URL with one or more sets, separated by commas: `sites`, `media`, `domains`, `data`, `phone`, `mail`, `mail_automation`, `commerce`, `stores`, `apps`, `agents`, `team`, `agency`. For example `https://api.hypamoon.com/api/v1/mcp?tools=sites,data`. Every set also brings the core tools. The scopes of your key still decide what you can do; a set only hides tools. ChatGPT ignores `?tools=`.

## Safeguards

This is what is in place today.

- **Test and live.** A test key or test project keeps everything simulated: no money moves, no real calls, test payments only.
- **You confirm what cannot be undone.** When a chat connected with OAuth asks for something you cannot take back, like deleting a site, releasing a number, changing DNS on a live domain, changing who is in the team or moving to live, the tool does not do it. It answers with a link to the portal. You read what will happen in plain words and confirm with your passkey. Cancel changes nothing.
- **Four eyes, if you want them.** An owner can turn on four-eyes approval. A live deploy or a deleted site then waits for a second owner or builder. Nobody approves their own change, not even one made by their own agent.
- **No money from the chat.** Connecting your payment account and agreeing to our fee only happen in the portal, by the owner. Buying a number or a domain and refunding an order set up a confirmation in the portal; you check the amount and click there, or nothing happens.
- **Passwords stay out of the chat.** When your chat needs a key for another service, it asks you to paste it in the portal, not in the conversation.
- **Adapters ask first.** A chat cannot give itself access to an adapter. An owner allows each rule in the portal, and every write through an adapter waits for approval.
- **Calls with manners.** Before every live call your chat tells you who it will call and why, and waits for your yes. The voice AI always says it is a digital assistant; that cannot be switched off. Sales calls only go out when the person's consent is recorded. Outbound calls need a verified account.
- **Checks on every site.** Fake login pages, card fields and forms that send data elsewhere do not go live. Public sites are checked daily against lists of known phishing and malware.
- **Rate limits.** Every key and every connected app has a rate limit.

## Learn more

- MCP reference, every tool with its scope: https://hypamoon.com/docs/mcp
- Connect step by step, per app: https://hypamoon.com/connect
- Does Hypamoon connect to your software? A page per package with what its API allows: https://hypamoon.com/connect-to
- Your own version of the tools you rent, with the sentence to start with: https://hypamoon.com/your-own
- Examples built with Hypamoon: https://hypamoon.com/examples
- Adapters: https://hypamoon.com/docs/adapters
- All docs: https://hypamoon.com/docs
- Status: https://status.hypamoon.com
- Support: https://hypamoon.com/support

## Security and abuse

Found a vulnerability? Email security@hypamoon.com. Please read https://hypamoon.com/security first; our contact details are also in https://hypamoon.com/.well-known/security.txt. See [SECURITY.md](SECURITY.md).

Seen a site, number or message on Hypamoon that looks like phishing, fraud or spam? Report it at https://hypamoon.com/abuse or email abuse@hypamoon.com.

## Who we are

Hypamoon is run by Hypamoon B.V., Rotterdam, the Netherlands. Privacy: https://hypamoon.com/privacy. Terms: https://hypamoon.com/terms

The text in this repository is ours. You are welcome to link to it and quote from it, but please do not republish it as your own.
