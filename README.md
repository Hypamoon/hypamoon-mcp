# Hypamoon MCP server

**Where AI lives.** Everything between AI and the workfloor.

Most people can already get a chat to write a website, a booking form or a small app. Then it stops. The page sits in the chat, nobody can visit it, the form sends nowhere, and there is no way to take a payment or answer the phone. That gap between what AI writes and what actually runs in a business is what Hypamoon fills.

Connect Hypamoon to Claude, ChatGPT or Cursor and ask for it in plain words. Your chat builds it, Hypamoon runs it.

This repository holds the public listing of the Hypamoon MCP server: this README, the `server.json` we publish to the MCP Registry, and how to report a security issue. The server itself is hosted by us. There is nothing to install.

## What your chat can do with it

- **Put a site or app live.** A real address on hypamoon.app or your own domain. Apps can have sign-in, roles, private data per user and file uploads.
- **Collect forms.** Contact forms, bookings and orders land in your project, and your chat can read them back.
- **Take payments.** A checkout on your own site, paid into your own Mollie account. You are the seller and the money goes straight to you. Hypamoon never holds it.
- **Send email.** From your own domain, to people who already know you, like someone who filled in your form.
- **Domains.** Find a new domain or connect the one you have. Your existing mailbox keeps working.
- **A phone number that answers.** Calls can ring your own phones first, and a voice assistant picks up when nobody does. Also SMS, and a chat on your site.

The full list of tools, with what each one does, is in the docs: https://hypamoon.com/docs/mcp

## Connect

The MCP URL:

```text
https://api.hypamoon.com/api/v1/mcp
```

Start with your test project. In test mode nothing costs money, numbers are test numbers and calls are simulated. Switch to your live project when it works.

### Claude (claude.ai and Claude Desktop)

Customize, Connectors, Add custom connector. Name it `Hypamoon`, paste the MCP URL and leave the OAuth fields empty. Choose Connect and sign in.

### Claude Code

```bash
claude mcp add --transport http hypamoon https://api.hypamoon.com/api/v1/mcp
```

Then type `/mcp`, pick `hypamoon` and choose Authenticate. Your browser opens and you sign in.

### ChatGPT

Plugins, Add, Add custom MCP server. Name it `Hypamoon`, paste the MCP URL, choose OAuth and leave the client fields empty. Connect and sign in. In a work account an admin may have to allow custom MCP servers first.

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

Cursor opens the Hypamoon sign-in in your browser. Prefer a key? See below.

Step by step for every app: https://hypamoon.com/connect

## Signing in

**OAuth (the chat apps).** You paste the URL, the app sends you to a Hypamoon consent screen, and there you choose which project the app may use and what it may do. You can see every connected app in the portal, with when it was last used, and revoke it there. It stops working right away.

**API key (scripts, CI, your own app).** Create a key in the portal and send it as `Authorization: Bearer hm_test_...` or `hm_live_...`. We only store a hash of it. Keep it out of the chat and out of browser code.

**What it may do.** Every tool needs a scope, and a key or connection only sees the tools its scopes allow. In plain words:

| Scope | What it allows |
|---|---|
| `project:read` | See the project, its balance and its agents. Always on. |
| `project:write` | Change company details and manage agents. |
| `sites:read` / `sites:write` | See or publish sites and apps, and manage domains and media. |
| `data:read` / `data:write` | Read or write form entries, orders and other collections. |
| `numbers:read` / `numbers:write` | See numbers, or claim one and set who it rings. |
| `assistant:write` | Set up the voice assistant. |
| `calls:read` / `calls:write` | Read calls and transcripts, or let the assistant call someone. |
| `messages:read` / `messages:write` | Read or send SMS and email. |
| `chat:read` / `chat:write` | Read or run the chat on your site. |
| `webhooks:write` | Send events to your own systems. |

An owner can also limit a key or a connection to a few sites and apps. More: https://hypamoon.com/docs/authentication

## Safeguards

We would rather say no than let something go wrong in a customer's name. This is what is in place today.

- **Test and live.** A test key or test project keeps everything simulated: no money moves, no real calls, test payments only.
- **Four eyes, if you want them.** An owner can turn on four-eyes approval. A live deploy or a deleted site then waits for a second owner or builder. Nobody approves their own change, not even one made by their own agent.
- **Money stays with the owner.** Connecting your payment account and agreeing to our fee only happen in the portal, by the owner.
- **Passwords stay out of the chat.** When your chat needs a key for another service, it asks you to paste it in the portal, not in the conversation.
- **Calls with manners.** Before every live call your chat tells you who it will call and why, and waits for your yes. The voice assistant always says it is a digital assistant and asks if it is a good time; that cannot be switched off. Sales calls only go out when the person's consent is recorded, and only within calling hours. Outbound calls need a verified account.
- **Checks on every site.** Fake login pages, card fields and forms that send data elsewhere do not go live. Public sites are checked daily against lists of known phishing and malware.
- **Rate limits.** Every key and every connected app has a rate limit.

## Security and abuse

Found a vulnerability? Email security@hypamoon.com. Please read https://hypamoon.com/security first; our contact details are also in https://hypamoon.com/.well-known/security.txt. See [SECURITY.md](SECURITY.md).

Seen a site, number or message on Hypamoon that looks like phishing, fraud or spam? Report it at https://hypamoon.com/abuse or email abuse@hypamoon.com.

## Links

- Docs: https://hypamoon.com/docs
- MCP reference: https://hypamoon.com/docs/mcp
- Status: https://status.hypamoon.com
- Support: https://hypamoon.com/support
- Privacy: https://hypamoon.com/privacy
- Terms: https://hypamoon.com/terms

## Who we are

Hypamoon is run by Hypamoon B.V., Rotterdam, the Netherlands.

The text in this repository is ours. You are welcome to link to it and quote from it, but please do not republish it as your own.
