# SOP: Post-Restart MCP Tool Loading

After every Claude Code restart, MCP tools come up as **deferred** — they appear by name but their schemas aren't loaded into the session until explicitly fetched. Trying to call them before loading throws `tool not found` even though `claude mcp list` shows the server as connected.

## Fix: one-line ToolSearch after every restart

For our standard ad-production stack, run this immediately on session start:

```
ToolSearch(query="select:mcp__atlascloud__atlas_upload_media,mcp__atlascloud__atlas_generate_image,mcp__atlascloud__atlas_generate_video,mcp__atlascloud__atlas_get_prediction,mcp__atlascloud__atlas_list_models,mcp__atlascloud__atlas_get_model_info,mcp__higgsfield__balance,mcp__higgsfield__media_upload,mcp__higgsfield__media_confirm,mcp__higgsfield__generate_image,mcp__higgsfield__generate_video,mcp__higgsfield__job_status,mcp__higgsfield__models_explore,mcp__claude_ai_Slack__slack_send_message,mcp__claude_ai_Slack__slack_search_channels", max_results=20)
```

This force-loads the schemas for: Atlas Cloud (upload + image + video + polling + model info), Higgsfield (balance + media + image + video + jobs + models), and Slack (send + search).

## What stays at user scope (persistent across restarts)
- `atlascloud` (npx-y atlascloud-mcp) — registered with API key in env
- `higgsfield` (HTTP MCP)
- `playwright` (npx @playwright/mcp@latest)

## Verification command
```bash
claude mcp list | grep -E "atlascloud|higgsfield|playwright|Slack"
```
All should show `✓ Connected`.

## If a tool still won't load
1. Check if the MCP server is actually responding: `claude mcp list`
2. If "Failed to connect", remove and re-register at user scope: `claude mcp remove [name]; claude mcp add -s user [name] -- [command]`
3. If still failing, restart Claude Code and try ToolSearch again — sometimes the first restart after MCP changes drops the schema fetch.
