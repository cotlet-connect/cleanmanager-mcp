# CleanManager MCP server

Connect CleanManager to ChatGPT, Claude and any AI agent through Cotlet.dev. Read cleaning schedules and check-ins, arrange absence cover, review quality reports and manage stock in your own signed-in account. Write actions ask for approval by default.

## What you can ask

> Show today's cleaning tasks and any missed check-ins.

> Find cleaning products below their minimum stock levels.

> Summarize completed quality reports for this month.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its CleanManager tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 cleanmanager_agent.py serve
```

Or with Docker:

```bash
docker build -t cleanmanager-mcp .
docker run -i --rm -e COTLET_API_KEY cleanmanager-mcp
```

Tools: `cleanmanager_guide`, `cleanmanager_status`, `cleanmanager_open`, `cleanmanager_read`, `cleanmanager_click`, `cleanmanager_type`, `cleanmanager_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 cleanmanager_agent.py connect   # ready config for ChatGPT, Claude, Claude Code and Cursor
python3 cleanmanager_agent.py status    # is CleanManager ready in your Cotlet.dev account
python3 cleanmanager_agent.py guide     # CleanManager screens and sign-in link
python3 cleanmanager_agent.py read      # open CleanManager, read the page, close the browser
python3 cleanmanager_agent.py tools     # list Cotlet.dev tools
python3 cleanmanager_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from CleanManager users

**Which CleanManager login should I use?**

Use an administrator for the whole company, or a supervisor for their assigned customer areas.

**Do all tools require add-on modules?**

Calendar, check-in, customers, cleaning plans and storage are in the Base module. Quality reports need Audits, and SMS needs Text Messaging.

**Is Cotlet affiliated with CleanManager?**

No. Cotlet.dev is independent and not affiliated with CleanManager.

## Learn more

- [CleanManager integration and hosted tools](https://cotlet.dev/integrations/cleanmanager-api-mcp-server)

---

More: https://cotlet.dev/integrations/cleanmanager-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=cleanmanager

Request access: https://cotlet.dev/signup?app=cleanmanager&utm_source=github&utm_medium=readme&utm_campaign=cleanmanager

## Workflows on the hosted integration

The live integration page documents these 29 operations:

- `get_calendar`
- `create_task`
- `cancel_task`
- `create_closing_days`
- `register_absence`
- `list_daily_tasks`
- `manage_daily_task`
- `list_check_ins`
- `list_active_alarms`
- `get_photo_documentation`
- `list_customers`
- `manage_customer`
- `add_customer_resource`
- `set_service_agreement_billing`
- `get_invoicing`
- `list_employees`
- `manage_employee`
- `manage_wage_profiles`
- `list_cleaning_plans`
- `manage_cleaning_plan`
- `manage_report_template`
- `fill_out_report`
- `list_completed_reports`
- `get_report_statistics`
- `get_inventory`
- `handle_product_request`
- `order_or_transfer_products`
- `manage_storage_items`
- `send_message`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
