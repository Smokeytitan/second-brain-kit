# Second Brain Kit

A portable, fully generic AI second-brain system for product marketers (or anyone who runs on projects, launches, and stakeholders). Built for Claude Desktop's Cowork mode: a folder of markdown files Claude reads, updates, and acts on, plus 25+ workflow skills and agent specs that automate the recurring work.

Everything in this repo is a template. No company data — fill in the placeholders (`{LIKE_THIS}`) with your own company, products, people, and tool IDs.

## What's in here

```
second-brain-kit/
├── README.md              ← you are here
├── starter-vault/         ← copy this to start your vault (PARA structure)
│   ├── CLAUDE.md          ← Claude's context file — customize first
│   ├── Dashboard.md       ← your command center
│   └── Resources/         ← project + area templates
├── memory/                ← two-tier memory system (empty templates)
│   ├── glossary.md        ← acronyms, nicknames, codenames, writing prefs
│   ├── decisions.md       ← running decisions log
│   ├── people/            ← one profile per key collaborator
│   ├── projects/          ← deep context per project
│   ├── areas/             ← ongoing responsibility trackers
│   └── context/           ← company, legal flags, stakeholders, meetings
├── skills/                ← 25+ genericized Claude skills
└── agents/                ← scheduled-task and agent-roster specs
```

## Setup (~20 minutes)

### 1. Install Claude Desktop + Cowork
Download from [claude.ai/download](https://claude.ai/download), open Cowork in the left sidebar.

### 2. Create your vault
Copy `starter-vault/` contents into a folder called **Second Brain** — either in Google Drive (with [Drive for Desktop](https://www.google.com/drive/download/) so it syncs locally) or a plain local folder. Point Cowork at that folder.

### 3. Customize CLAUDE.md
Fill in your name, role, projects, and areas. This is the single highest-leverage file — Claude reads it every session.

### 4. Copy in the memory system
Copy `memory/` into your vault. Fill in `glossary.md` and `context/company.md` first; add people profiles as you go (or run the `memory-buildout` skill to generate them from existing context).

### 5. Add skills
Upload skills from `skills/` to Claude (Settings → Capabilities → Skills), starting with the ones matching your daily work. Each SKILL.md has placeholders to fill in (channel IDs, list IDs, folder IDs) — search for `{` in the file to find them.

### 6. Connect your tools
Settings → Connectors: calendar, Drive/file storage, Slack (or your chat tool), project tracker (ClickUp/Linear/etc.), GitHub as relevant. Skills degrade gracefully when a connector is missing, but the daily brief and meeting prep need calendar + docs at minimum.

### 7. Schedule the recurring agents
See `agents/` for specs. Start with one scheduled task (e.g. a weekly release radar or daily brief) and add more once the first is reliable.

## The system in one paragraph

The vault is PARA (Projects / Areas / Resources / Archives) with a Dashboard on top. The `memory/` directory is Claude's long-term knowledge: who people are, what projects mean, what words mean, what's been decided, and what legal/brand rules constrain content. Skills encode repeatable workflows (drafting, launches, competitive intel, status updates) so output quality doesn't depend on remembering the process. Scheduled agents run the recurring workflows on a cadence and deliver briefs. You talk to Claude in plain language; the files are the state.

## Care and feeding

- Date-stamp running notes; newest first.
- When something goes wrong twice, write the lesson into the relevant skill or memory file — the system should learn.
- Re-run a quarterly interview (see `skills/interview-agent/`) to evolve the agent roster instead of letting it drift.
- Keep `memory/context/legal-flags.md` current; every content skill checks it.
