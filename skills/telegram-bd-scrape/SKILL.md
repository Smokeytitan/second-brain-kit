---
name: telegram-bd-scrape
version: 1.0.0
description: >
  Scrape BD conversations from Telegram Web into structured, deal-intake-ready
  transcript files using the Claude in Chrome browser extension. Use whenever
  the user says "scrape the telegram chat with [partner]", "pull the
  TG history with [company]", "get the telegram convo", "telegram transcript
  for [deal]", "scrape TG", "grab the telegram thread with [person]", or any
  variant asking to extract a Telegram conversation into a transcript. Primary
  consumer: your deal-intake workflow (deal-context transcripts digested into
  needs, objections, stack, timeline); also works as a general-purpose
  Telegram export. Requires Claude in Chrome connected and the user ALREADY
  logged into Telegram Web (web.telegram.org) in that Chrome profile; login is
  a one-time manual step the user does themselves. Saves output to
  `Resources/TG-Transcripts/YYYY-MM-DD-{chat-slug}.md`. Read-only: never
  sends, reacts, or writes into Telegram.
---

# Telegram BD Scrape (browser extraction)

## What this builds
A dated markdown transcript of one named Telegram chat, extracted through
Telegram Web in the user's own logged-in Chrome session. No API keys, no bot,
no MCP connector: the Claude in Chrome extension opens the chat, scrolls back
through history, and harvests messages from the DOM. The output file drops
straight into your deal-intake workflow as an uploaded transcript. Optionally
append a BD digest (needs, objections, stack, timeline, next steps) that
mirrors your deal-brief fields.

Replaces the manual copy-paste workflow most teams assume is the only option.

## Step 1: Preflight
1. Confirm Claude in Chrome tools are available (load via ToolSearch if
   deferred). If the extension is not connected, stop and tell the user to
   connect it.
2. Navigate to `https://web.telegram.org/`. Note which client loads: `/k/` or
   `/a/` in the URL (different DOMs, see Gotchas).
3. Detect logged-out state: a QR code, "Log in to Telegram" text, or a
   phone-number field means not logged in. If logged out, STOP. Tell the user
   to log in themselves in that Chrome window and re-run. Claude NEVER
   touches the QR code, phone number, or 2FA code, and never asks the user to
   relay them. Login is one-time; the session persists across runs.
4. First scrape of any chat: warn the user that opening a chat in Telegram
   Web marks it as read on their account. Get an OK before proceeding.

## Step 2: Locate the chat
1. Use Telegram Web's search box for the partner, company, or person name the
   user gave.
2. If multiple plausible matches (a DM plus a group, or several groups), list
   them and ask the user which one. Do not guess.
3. Open the chat and confirm the exact chat title (and participant count for
   groups) back to the user before extracting anything.

## Step 3: Scope
Ask for a date range before scrolling. Accepted answers: explicit range
("May 1 to today"), "last N weeks", or "since we last scraped". For "since
last scraped", check `Resources/TG-Transcripts/` for existing files matching
the chat slug and use the latest covered end date in the metadata header as
the new start.

Never default to an unbounded full-history scrape, especially of large
groups. If the user insists on full history, estimate the scroll budget first
and get confirmation.

## Step 4: Extraction loop (the hard part)
Telegram Web virtualizes the message list: only messages near the viewport
exist in the DOM. Messages scrolled past are destroyed. You cannot read the
whole chat in one pass.

Protocol:
1. Start at the bottom (newest). Extract visible messages with
   `get_page_text` or `read_page` (text extraction, not screenshots: faster
   and cheaper than vision).
2. Scroll UP one viewport-sized increment (keyboard PageUp or a scroll action
   on the message container). Wait for the lazy-load batch to render (roughly
   20 to 40 messages per batch), then extract again.
3. Accumulate messages across passes. Dedupe by the tuple (sender, timestamp,
   first ~50 chars of text), since consecutive scroll windows overlap
   heavily.
4. Use Telegram's date-separator headers ("June 12", "May 3") as scroll
   anchors: they tell you how deep you are and resolve full dates for
   time-only timestamps.
5. Stop when you cross the start of the requested date range, or hit the top
   of the chat history.

Message handling rules:
- Reply-quotes: capture the quoted context inline, e.g.
  `(replying to Sender: "quoted snippet")`.
- Media and attachments: record placeholders: `[photo]`,
  `[document: filename.pdf]`, `[voice note]`, `[video]`, `[sticker]`. Do not
  attempt to download media.
- Edited messages: take the latest text; note `(edited)` if visible.
- Service messages (joins, leaves, pins, title changes): skip unless the user
  asks for them.
- Forwarded messages: prefix with `[forwarded from X]`.

If extraction stalls (same messages returned twice in a row, scroll not
advancing) or the DOM structure fights back twice, stop and re-plan (try the
other client via the URL, try `javascript_tool` to read the message container
directly) rather than retrying the same way.

## Step 5: Write the transcript
Save one markdown file: `Resources/TG-Transcripts/YYYY-MM-DD-{chat-slug}.md`
(today's date, lowercase hyphenated chat name). Never overwrite an existing
file: append `-v2`, `-v3` if regenerating.

File structure:
1. Metadata header: chat name, participants, date range covered, scraped-at
   timestamp, message count, and coverage note (complete, or partial with the
   reason).
2. Messages in chronological order (oldest first), one per line:
   `**[YYYY-MM-DD HH:MM] Sender:** message text`

This exact format is what a downstream deal-intake workflow consumes as an
uploaded transcript. Do not add commentary between messages.

After writing, present the transcript file to the user.

## Step 6: Optional BD digest
Offer (do not force) a `## BD Digest` section appended after the transcript,
mirroring your deal-brief fields:
- Needs and pain points
- Objections and concerns
- Technical context: stack, platforms, vendors mentioned
- Timeline signals
- Next-step commitments

Every digest line must cite the timestamp of the message it traces to, e.g.
`(2026-06-14 09:32)`. No inferred claims without a traceable message.

## Guardrails (firm)
- Only scrape chats the user explicitly names. Never enumerate, browse, or
  bulk-export the chat list.
- These are conversations with external partners: transcript files are
  internal-confidential deal context. Never paste transcript content into
  external-facing documents; your deal-workflow's grounding rules govern what
  reaches a deck.
- Personal or non-BD chats are out of scope even if asked casually. If a chat
  looks personal (a friend, a non-work group), confirm intent before
  extracting.
- Read-only on the user's account. Never send messages, react, delete, or
  intentionally mark-as-read. Opening a chat inherently marks it read: warn
  before the first scrape of any chat (Step 1.4).
- No credentials handling, ever. No QR, no phone number, no 2FA code, no
  session tokens. If Telegram logs the user out mid-scrape, stop and hand
  back to the user.

## Gotchas
- Virtualized DOM: messages vanish from the DOM once scrolled past. Extract
  after EVERY scroll increment; you cannot go back and re-read without
  scrolling back down.
- Two Telegram Web clients: `/k/` (WebK) and `/a/` (WebA) have different DOM
  structures. Detect which loaded from the URL and adapt selectors; `/a/` is
  generally easier to parse. If one fights you, switch by editing the URL
  path.
- Timestamps show time-only ("14:32") for recent messages. Resolve full dates
  from the date-separator headers as you scroll; never guess a date.
- Lazy-load batches are roughly 20 to 40 messages. Text extraction
  (`get_page_text`) beats screenshots: no vision cost, no OCR errors on
  usernames.
- Long chats: budget scrolls up front (estimate messages per week times weeks
  requested, divided by ~30 per batch). If the budget runs out, stop and
  report partial coverage honestly in the metadata header rather than
  silently truncating.
- Scroll position can jump when Telegram loads a batch: re-anchor on the
  nearest date separator after each load instead of counting scroll
  increments.
- If a step fails twice, stop and re-plan rather than retrying the same way.
- Honor the user's writing preferences in output files (for example, if they
  ban em and en dashes, use colons, commas, parentheses, or plain hyphens).

## Cross-references
- Deal-intake workflow: the transcript format in Step 5 is the expected
  upload shape; the digest in Step 6 maps to your deal-brief fields.
- Sibling skill `telegram-scanner` produces a prioritized daily digest across
  many chats; hand off here when a single thread needs a full export.
