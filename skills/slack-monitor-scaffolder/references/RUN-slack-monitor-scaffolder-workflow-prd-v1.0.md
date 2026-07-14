```yaml
doc_type: RUN
normative: true
requires: []
```

# RUN — Slack Monitor Scaffolder Workflow
**Version:** 1.0.0
**Skill:** slack-monitor-scaffolder

---

## Overview

This RUN file scaffolds a complete Slack channel monitor: a `prompt.md` (the Claude prompt
executed on cron), a shell launcher script, a crontab registration, and a smoke test.

Two monitor modes are supported:

| Mode | Trigger | Behavior | Model |
|------|---------|----------|-------|
| **All-messages** | "monitor #channel" | Forward every new human message as a digest | Haiku |
| **Conditional** | "monitor #channel for X" | Only notify when a message matches criteria | Haiku |

---

## Step 1 — Resolve inputs

Extract the following from the user's request:

| Variable | Source | Default |
|----------|--------|---------|
| `CHANNEL_NAME` | User's message (strip leading `#`) | — required |
| `CHANNEL_ID` | User's message if provided explicitly | Resolve via `slack_search_channels` |
| `FILTER` | Text after "for" in "monitor #channel for X" | `"all messages"` if absent |
| `NOTIFY_ID` | User's message if provided | `{DEFAULT_NOTIFY_USER_ID}` — your own Slack user ID |
| `FREQUENCY` | User's message if provided | `0 * * * *` (hourly) |

If `CHANNEL_ID` is not provided explicitly, call `slack_search_channels` with the channel name
to resolve it. If `NOTIFY` was given as a display name rather than a Slack user ID, call
`slack_search_users` to resolve it.

**Mode determination:**
- If `FILTER` = `"all messages"` (or no filter was given) → **All-messages mode**
- Otherwise → **Conditional mode**

---

## Step 2 — Derive slug

Strip common prefixes from `CHANNEL_NAME`, then normalize:

1. Remove leading `team-`, `wg-`, `feed-` prefix (one removal only) — adapt to your workspace's channel-prefix conventions
2. Replace spaces, `#`, and underscores with hyphens
3. Lowercase

Examples:
- `team-platform-eng` → `platform-eng`
- `wg-product-core` → `product-core`
- `feed-deploy-announcements` → `deploy-announcements`
- `general` → `general`

This value is `{slug}` used throughout the remaining steps.

---

## Step 3 — Write prompt.md

Write the file to `scripts/{slug}-monitor/prompt.md`.

Create the parent directory if it does not exist (the Write tool handles this automatically).

### All-messages template

```
You are a silent monitoring agent. Follow these steps exactly and output only a one-line status.

CHANNEL: {CHANNEL_ID}  (#{CHANNEL_NAME})
NOTIFY:  {NOTIFY_ID}
STATE:   $REPO_DIR/scripts/{slug}-monitor/.state.json

Step 1 — Load state.
Read STATE. If file doesn't exist or has no last_ts, use last_ts = (current Unix time − 3600).

Step 2 — Fetch messages.
Call slack_read_channel with channel_id={CHANNEL_ID}. Filter the results to messages where:
- ts (as float) > last_ts (as float)
- subtype is absent or null (no join/leave/bot events)
- bot_id is absent

Sort matching messages oldest-first.

Step 3 — If no new messages, print "No new messages." and stop. Do NOT write the state file.

Step 4 — Format digest.
Build a Slack message:
  *:bell: #{CHANNEL_NAME} — {n} new message(s)*
  • {YYYY-MM-DD HH:MM UTC}  <@USER_ID>: {text, truncated to 300 chars with … if longer}
  (one bullet per message)
  _<https://{YOUR_WORKSPACE}.slack.com/archives/{CHANNEL_ID}|Open channel>_

Step 5 — Send DM.
Call slack_send_message with channel={NOTIFY_ID} and the digest text.

Step 6 — Save state.
Write STATE with last_ts = ts of the newest message (as a string).

Step 7 — Print "Sent DM: {n} message(s)." and stop.
```

### Conditional template

Same as All-messages template, but replace Step 3 and insert a Step 2b:

```
Step 2b — Apply filter.
Review each new message. Keep only messages that {FILTER_INSTRUCTION}.
If no messages pass the filter, print "No matching messages." and stop. Do NOT write the state file.

Step 3 — If no messages remain after filtering, stop (already handled in 2b).
```

`FILTER_INSTRUCTION` is a plain-English translation of `FILTER`. Examples:
- "for deploy announcements" → "mentions a deploy, release, or service going live"
- "for incidents" → "describes an incident, outage, or service degradation"
- "for mentions of our product" → "explicitly mentions the product name, its API, or its known aliases"

The filtered message list (not the full list) is used for the digest in Step 4.
The state is updated to the newest message's ts regardless of whether it passed the filter,
so already-seen messages are not re-evaluated on the next run.

---

## Step 4 — Write shell script

Write the launcher to `scripts/run-{slug}-monitor.sh` with the following content:

```bash
#!/usr/bin/env bash
# Cron: {FREQUENCY} $REPO_DIR/scripts/run-{slug}-monitor.sh
set -euo pipefail

REPO_DIR="$(cd "$(dirname "$0")/../.." && pwd)"
CLAUDE_BIN="$(which claude)"
PROMPT="$REPO_DIR/scripts/{slug}-monitor/prompt.md"
LOG_FILE="$REPO_DIR/logs/{slug}-monitor.log"

mkdir -p "$(dirname "$LOG_FILE")"
[[ -f "$REPO_DIR/.env.local" ]] && set -a && source "$REPO_DIR/.env.local" && set +a

exec "$CLAUDE_BIN" \
  --print \
  --model claude-haiku-4-5-20251001 \
  --allowedTools "mcp__claude_ai_Slack__slack_read_channel,mcp__claude_ai_Slack__slack_send_message,Read,Write" \
  < "$PROMPT" >> "$LOG_FILE" 2>&1
```

After writing, make the file executable:
```bash
chmod +x scripts/run-{slug}-monitor.sh
```

---

## Step 5 — Register crontab entry

Append the cron entry to the user's crontab:

```bash
(crontab -l 2>/dev/null; echo "{FREQUENCY} $REPO_DIR/scripts/run-{slug}-monitor.sh") | crontab -
```

---

## Step 6 — Smoke test

Run the prompt once to confirm it executes without error:

```bash
$(which claude) \
  --print --model claude-haiku-4-5-20251001 \
  --allowedTools "mcp__claude_ai_Slack__slack_read_channel,mcp__claude_ai_Slack__slack_send_message,Read,Write" \
  < scripts/{slug}-monitor/prompt.md
```

Expected output: `No new messages.` or `No matching messages.` or `Sent DM: N message(s).`

Capture the result for the Step 7 report.

---

## Step 7 — Report

Print a confirmation summary:

```
✓ Created: scripts/{slug}-monitor/prompt.md
✓ Created: scripts/run-{slug}-monitor.sh
✓ Crontab: {FREQUENCY} run-{slug}-monitor.sh
✓ Smoke test: {result}
Model: claude-haiku-4-5-20251001 — change --model in the .sh for more nuanced filtering
```
