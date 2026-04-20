# Pipedrive MCP Plugin for ClaudeCoWork

Full Pipedrive CRM integration for Claude Code. Manage deals, contacts, organizations, leads, activities, and more directly from Claude.

## Installation (ClaudeCoWork Plugin)

### 1. Add the CADTALK marketplace (one-time, if not already added)

In `~/.claude/settings.json`, add under `extraKnownMarketplaces`:

```json
"cadtalk-plugins": {
  "source": {
    "source": "github",
    "repo": "jeffbrickler/mcp-pipedrive"
  }
}
```

### 2. Install the plugin

```bash
claude plugins add pipedrive@cadtalk-plugins
```

### 3. Configure credentials

In Claude Code, run:

```
/pipedrive-setup
```

The setup wizard asks for your Pipedrive API token and domain, then configures everything automatically.

### 4. Restart Claude Code

Restart or run `claude mcp restart` to activate the Pipedrive tools.

---

## Available Tools

After setup, Claude has access to 100+ Pipedrive tools including:

- **Deals**: create, update, search, move through pipeline stages, mark won/lost
- **Contacts**: create, search, merge, list activities
- **Organizations**: create, search, list deals and contacts
- **Leads**: create, update, search, manage labels
- **Activities**: create, schedule, mark done
- **Pipelines & Stages**: list, get statistics
- **Notes**: create, update, list
- **Files**: upload, download, attach to deals

## Team Distribution

Share these steps with CADTALK teammates:

```
1. Add to ~/.claude/settings.json under extraKnownMarketplaces:
   "cadtalk-plugins": { "source": { "source": "github", "repo": "jeffbrickler/mcp-pipedrive" } }

2. claude plugins add pipedrive@cadtalk-plugins

3. In Claude Code: /pipedrive-setup
   (Enter your personal Pipedrive API token and domain when prompted)

4. Restart Claude Code
```

Each team member uses their own Pipedrive API token.

## Development

To rebuild the bundle after source changes:

```bash
npm install
npm run bundle
git add -f dist/index.cjs
git commit -m "chore: rebuild bundle"
git push
```

Team members reinstall to get the update:

```bash
claude plugins update pipedrive@cadtalk-plugins
```
