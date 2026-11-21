# Wild Apricot MCP server for ChatGPT and Claude

Wild Apricot connects to ChatGPT, Claude and other AI agents through Cotlet.dev's remote MCP server and your signed-in Cloud Browser. Your agent works on the contacts, members, events and finances screens your own login can access. The live integration page, checked 20 November 2026, reports no official Wild Apricot MCP server found. Wild Apricot offers its own API for custom code.

## What you can ask

> Read pending member applications and prepare a list of complete, paid applications. Ask me before each approval.

> Draft a renewal reminder for lapsed members and ask me before sending.

> Copy last month's Saturday clean-up event with the same registration types.

> Which invoices are unpaid after 30 days, and who are they for?

> List registrations for our next event and identify unpaid registrations.

> Export active members with their membership level and renewal date.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Open Plugins > Add > Add custom MCP server. Name it Cotlet and use `https://cotlet.dev/mcp/v1/connect` with OAuth. Enable Developer mode in Settings > Security and login if the option is missing. Connect Cotlet, allow access, then select @Cotlet in a new chat.

### Claude

Open Connectors > Add > Add custom connector. Name it Cotlet and use `https://cotlet.dev/mcp/v1/connect`. Allow access, then enable Cotlet under the plus button > Connectors in a chat.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Wild Apricot tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Clone this repository and run `python3 wild_apricot_agent.py serve` with `COTLET_API_KEY` set.

Or with Docker:

```bash
docker build -t wild-apricot-mcp .
docker run -i --rm -e COTLET_API_KEY wild-apricot-mcp
```

The local server exposes seven helpers for the hosted browser tools.

Local tools: `wild_apricot_guide`, `wild_apricot_status`, `wild_apricot_open`, `wild_apricot_read`, `wild_apricot_click`, `wild_apricot_type`, `wild_apricot_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Run from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 wild_apricot_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 wild_apricot_agent.py status    # is Wild Apricot ready in your Cotlet.dev account
python3 wild_apricot_agent.py guide     # Wild Apricot screens and sign-in link
python3 wild_apricot_agent.py read      # open Wild Apricot, read the page, close the browser
python3 wild_apricot_agent.py tools     # list Cotlet.dev tools
python3 wild_apricot_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Wild Apricot users

**Is this an official Wild Apricot integration?**

No. Cotlet.dev is independent and not affiliated with Wild Apricot.

**Do I need a Wild Apricot API key?**

No. Cotlet.dev works through your own signed-in Wild Apricot account. Wild Apricot also offers an API for your own code.

**How do I sign in?**

Open your organization's Wild Apricot address in your Cotlet Cloud Browser and sign in yourself. Enter any sign-in code there yourself. Use your actual organization address rather than the wildapricot.org root.

**Will members receive emails automatically?**

Ask your agent to prepare changes first and request your approval before member approvals, emails or invoice changes.

**What access does the agent have?**

The agent works on the same screens your own login can open. Keep payment settings, account billing and admin roles in your own hands.

## Live integration

[Wild Apricot tools and connection guide](https://cotlet.dev/integrations/wild-apricot-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=wild-apricot)

---

More: https://cotlet.dev/integrations/wild-apricot-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=wild-apricot

Start free: https://cotlet.dev/signup?app=wild-apricot&utm_source=github&utm_medium=readme&utm_campaign=wild-apricot
