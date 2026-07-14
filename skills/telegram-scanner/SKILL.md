---
name: telegram-scanner
version: 1.1.0
description: >
  Personalized daily Telegram digest via Telegram Web in the user's logged-in
  Chrome session (Claude in Chrome extension; no API keys, no bot). SETUP mode
  interviews the user on which chats matter (or a widescan of unreads) and
  saves a config; RUN mode scans those chats since the last run and writes one
  prioritized digest. Triggers: "set up my telegram scanner", "run my telegram
  digest", "what did I miss on telegram", "scan my telegram", "telegram
  catch-up". Requires the user already logged into web.telegram.org in that
  Chrome profile. Read-only: never sends, reacts, or writes into Telegram.
---

# Telegram Scanner

No config file, or user says "set up" → run the Setup interview. Config exists → run the digest. Small change requests → edit the config, don't redo the interview.

## Setup interview
Preflight first (below). With the user's OK, read only the chat-list sidebar (names, folders, unread badges; open nothing) so you can offer real options. Then ask one question at a time:

1. **Coverage**: a priority list of named chats, a widescan of everything unread (capped, default 15 chats/run), or hybrid (deep-scan priority list + shallow widescan of the rest).
2. **The list**: show sidebar names, let them pick and rank. Which chats are read-every-message vs headline-only?
3. **Importance**: people, topics, keywords to always surface; noise to always bury (stickers, GM chains, price talk, bots).
4. **Mark-as-read**: opening a chat marks it read on their account. OK, or keep a do-not-open list (those get reported by unread count only)?
5. **Digest and cadence**: length (default one screen), what time of day. Offer to schedule the run if a scheduling tool is available.

Save answers to `telegram-scanner/config.md` in the working folder: mode, max chats/run, priority chats with depth, do-not-open list, surface/bury rules, digest prefs, and a `last_run` cursor. Confirm the setup back in one plain sentence.

## Run
**Preflight**: confirm Claude in Chrome is connected (ToolSearch if deferred). Open `https://web.telegram.org/`. If logged out (QR code, phone field): STOP and tell the user to log in themselves. Never touch QR, phone number, or 2FA.

**Scan list**: load config and cursor (no cursor = last 24h). Read the sidebar without opening chats. Order: priority-full, priority-headlines, then unread chats by badge count until the cap. Never open do-not-open chats. Nothing new? Write a one-line digest and stop.

**Extract per chat**: Telegram Web virtualizes the DOM: messages vanish once scrolled past, so extract after every scroll. Start at the bottom, extract visible messages with `get_page_text` (text, not screenshots), scroll up one viewport, wait for the lazy-load batch (~20-40 msgs), extract again. Dedupe on (sender, timestamp, first ~50 chars). Resolve dates from the date-separator headers; never guess. Stop at the cursor, the "Unread messages" divider, or the scroll budget: ~10 scrolls for full-depth chats, ~3 for headlines/widescan; note partial coverage honestly. Media as `[photo]` / `[document: name]`; forwards as `[forwarded from X]`. If a chat stalls twice, switch client (`/k/` ↔ `/a/` in the URL, different DOMs) or read the container with `javascript_tool`; if it still fights, mark "could not scan" and move on.

**Write the digest** to `telegram-scanner/digests/YYYY-MM-DD-digest.md` (never overwrite; append `-v2`):

```
# Telegram Digest: {date} | {window} | {N} chats
## Needs your reply    - **{Chat}** {Sender}: "{quote}" (HH:MM)
## Worth knowing       - **{Chat}**: {1-2 lines}
## Headlines           - **{Chat}**: {one line}
## Skipped / buried    {filters applied + do-not-open unread counts}
```

Every needs-reply item cites a real message and timestamp; no inferred urgency. Direct questions to the user outrank channel broadcasts. Apply surface/bury rules; when in doubt, cut. Then update the cursor, present the file, and say the single most important item in 2-3 lines.

## Guardrails
- The config is the consent boundary: never open chats outside it; the do-not-open list is absolute.
- Read-only: never send, react, delete, or forward. Mark-as-read must have been accepted in setup.
- No credentials handling, ever. Logged out mid-run: stop and hand back.
- Digest content is confidential: save only where the config says; never quote personal chats verbatim — if widescan sweeps one in, ask whether to add it to do-not-open.
- Budget the whole run: if it's going long, finish priority chats properly and headline the rest.
- Honor writing preferences from the config (for example, if the user bans em and en dashes in output files, use colons, commas, parentheses, or hyphens).

## Cross-references
Sibling skill `telegram-bd-scrape` exports a full transcript of one chat: hand off there if the digest surfaces a thread the user wants in full.
