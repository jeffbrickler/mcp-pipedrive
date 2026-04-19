---
name: pipedrive-setup
description: First-run setup for the Pipedrive MCP plugin. Prompts for API credentials and writes the MCP server configuration to Claude Code settings.
---

# Pipedrive Setup

You are configuring the Pipedrive MCP server for this user. Follow these steps exactly.

## Step 1: Gather credentials

Ask the user for:
1. **Pipedrive API Token** — found at: Pipedrive → Settings → Personal preferences → API → Your personal API token
2. **Pipedrive Domain** — the full domain like `company-name.pipedrive.com`

Ask both questions in a single message. Do not proceed until you have both values.

## Step 2: Find the plugin install path

Run this Bash command to find where the plugin is installed:

```bash
find "$HOME/.claude/plugins/cache" -name "index.cjs" -path "*/pipedrive*/dist/index.cjs" 2>/dev/null | head -1
```

If nothing is found, fall back to checking the current working directory for `dist/index.cjs`.

If still not found, tell the user: "I couldn't locate the Pipedrive plugin bundle. Please ensure the plugin was installed with `claude plugins add`. The expected path is `~/.claude/plugins/cache/<marketplace>/pipedrive/<version>/dist/index.cjs`."

## Step 3: Read the current settings.json

Read the file at `~/.claude/settings.json` to get the current contents. You will add to it without removing any existing entries.

## Step 4: Write the MCP server configuration

Add the `pipedrive` entry under `mcpServers` in `~/.claude/settings.json`. The `command` must be `node`, the first arg must be the absolute path found in Step 2, and the `env` block must contain the literal values the user provided (not `${VAR}` references).

The resulting `mcpServers` block should look like:

```json
"mcpServers": {
  "pipedrive": {
    "command": "node",
    "args": ["/absolute/path/to/dist/index.cjs"],
    "env": {
      "PIPEDRIVE_API_TOKEN": "<token-from-user>",
      "PIPEDRIVE_DOMAIN": "<domain-from-user>",
      "MCP_TRANSPORT": "stdio",
      "PIPEDRIVE_RATE_LIMIT_MIN_TIME_MS": "250",
      "PIPEDRIVE_RATE_LIMIT_MAX_CONCURRENT": "2"
    }
  }
}
```

If `mcpServers` already exists in settings.json, merge the `pipedrive` entry into it without removing others. If it doesn't exist, add the key.

## Step 5: Confirm success

Tell the user:

> **Pipedrive MCP configured!** Restart Claude Code (or run `claude mcp restart`) for the Pipedrive tools to become available. You should see `mcp__pipedrive__*` tools in your tool list after restarting.
>
> If you need to update your credentials later, run `/pipedrive-setup` again.
