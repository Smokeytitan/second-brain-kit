```yaml
doc_type: BP
normative: true
requires: []
```

# LinkedIn Post Template (v1.0)

**Status:** Active v1.0
**Owner:** {SKILL_OWNER} — the person on your team who maintains this skill
**Consumers:** RUN-linkedin-ghostwriter-workflow-prd-v1.0
**Change control:** PR Review

**Scope:** Platform constraints, company-level content standards, and post type defaults.
Person-specific voice, vocabulary, and patterns live in the voice profile, not here.

---

## 1. Platform Constraints

### Length
- **Target:** 150–300 words
- **Hard max:** 350 words
- **First line:** Under 15 words. This is the only text visible in the feed before "...see more."

### Structure Options

Choose the structure that best matches the person's voice profile:

- **Narrative:** Short paragraphs (2–4 sentences each), conversational flow. Best for perspectives, reflections, milestone stories.
- **List:** Opening line + emoji-bullet or dash-bullet breakdown. Best for announcements with multiple components.
- **Hybrid:** Opening narrative paragraph + bullet breakdown + closing line. Best for announcements that need both context and specifics.

### Link Algorithm
LinkedIn deprioritizes posts with links in the body. Default: recommend placing links in the first comment. Override only if the person's voice profile shows consistent in-body link usage.

---

## 2. Post Type Defaults

Starting-point structures. The person's voice profile always overrides these.

### Announcement
- Hybrid structure (narrative + bullets)
- Lead with the news, not the backstory
- "What this means" bullets
- Tag relevant people/companies
- Forward energy close

### Milestone
- Narrative structure
- Lead with the achievement
- Brief context on why it matters
- Credit the team or collaborators
- Enthusiastic but grounded tone

### Perspective
- Narrative structure
- Lead with the take/observation
- 2–3 supporting points
- Can be shorter (150–200 words)

### Welcome
- Hybrid structure
- Name the people/company joining
- What they bring (capabilities, not just titles)
- Forward-looking energy
- Tag the people being welcomed

---

## 3. Company Terminology

<!-- Fill in your own terminology system. These rules apply to all company content,
regardless of person or platform. The examples below show the kinds of distinctions
worth codifying. -->

| Term | Usage | Notes |
|---|---|---|
| **{COMPANY}** | The company, the team, decisions made by people | Always use when a human entity is the actor |
| **{PLATFORM}** | The product/network/infrastructure itself | Use when referring to the platform, not the people |
| **{PRODUCT_NAME}** | Canonical casing and spelling of each product | Gloss on first use: "{PRODUCT_NAME} (a one-line plain-English explanation)" |
| **{DEPRECATED_NAME}** | Never use | Note the replacement term |
| **{AUDIENCE_TERM_1}** | e.g., "Builder" = developer building on your platform | Define each audience role term once |
| **{AUDIENCE_TERM_2}** | e.g., "Customer" = end user of a product built on your platform | |
| **{AUDIENCE_TERM_3}** | e.g., "Institution" = enterprise evaluating your company | |

Every coined or technical term gets a plain-English gloss on first use. If the reader has to Google a term, the sentence failed.

---

## 4. Confidence Rules

- **Declarative on what's shipped and measured.** "We process $X today." "Our latest release cut settlement time from minutes to seconds." State with full confidence.
- **Measured on what's coming.** "We expect..." or "The trajectory suggests..." Leave room to be wrong on forecasts without losing authority on facts.

---

## 5. Writing Mechanics

1. **No em dashes.** Use commas, colons, or new sentences. No exceptions.
2. **Numbers to the front.** Open paragraphs with quantified claims. "$3.4B+ in processed volume" as a paragraph opener, not buried mid-sentence.
3. **Chain arguments across sentences.** Echo key nouns across sentence boundaries. Don't stack disconnected claims and expect the reader to assemble them.
4. **Vary sentence rhythm.** Short declarative punch, then longer explanation, then short punch. Avoid strings of uniform medium-length sentences.
5. **Active voice.** "The platform settles transactions," not "Settlement is achieved by the platform."
6. **No noun stacks longer than three words without a verb.** "Cross-platform settlement infrastructure" needs a verb.
7. **Short paragraphs. Scannable headers.**
8. **Lead with business-outcome vocabulary** (cost, revenue, customers, time, volume, reliability). Technical terms come second, only when the reader needs them.

---

## 6. Banned Words & Constructions

### The test
If removing the word doesn't change what the sentence actually communicates, it is filler. Cut it.

### Corporate filler
synergy, leverage (as noun), holistic, world-class, solutions, robust (as empty adjective), ecosystem (as filler), streamline, optimize (without specifying what), end-to-end (standalone, without specifying the endpoints), empower, facilitate, accelerate (standalone), drive (as in "drive innovation"), foster, champion, revolutionary, game-changing, disruptive, paradigm shift, the future of [industry]

### Empty superlatives
cutting-edge, next-generation, best-in-class, industry-leading, state-of-the-art, unparalleled, unprecedented (unless genuinely first-ever), first-of-its-kind (unless verifiable), unique (as standalone adjective)

### Overused boosters
clearly, obviously, undeniably, undoubtedly, certainly, fundamentally, simply put, the reality is, the fact is, make no mistake, without question, there is no doubt. Use only when the claim is genuinely self-evident. If you need "clearly" to make something clear, it isn't.

### Marketing verbs that say nothing
unlock, unleash, supercharge, turbocharge, reimagine, revolutionize, transform (standalone), disrupt, power (as in "powering the future"), fuel (as in "fueling growth"), spearhead, pioneer (as verb), democratize (without specifics)

### Seamless family
seamless (standalone, without explaining what friction was removed), frictionless, effortless, plug-and-play (unless literally true), turnkey, out-of-the-box, zero-touch, one-stop-shop

### Banned constructions
- Em dashes. Use commas, colons, or new sentences.
- "Not only this, but that" and variants ("X isn't only Y. It's actually Z").
- Passive voice where active is possible.
- "It's worth noting that..." / "It should be noted that..." / "Interestingly..." Just say the thing.
- "In order to" (use "to").
- "At the end of the day" / "When all is said and done" / "Moving forward" / "Going forward"

---

## 7. Person-Deferred Rules

These rules defer to the person's voice profile. Apply the voice profile first. If the voice profile has no observed pattern for a given rule, use these defaults.

### Opening
- Default: Lead with a declarative statement carrying the news or vision.
- No engagement bait ("You won't believe...", "Hot take:", "Unpopular opinion:").
- No rhetorical questions unless the person uses them in their samples.

### Closing
- Default: Forward-looking energy or people tags.
- No engagement-bait CTAs ("Agree?", "What do you think?") unless the person uses them.

### Emoji
- Default: Do not add emojis.
- Only use if the person uses them in their samples. Match their style exactly.

### Hashtags
- Default: 0–3 max.
- Only include if the person uses hashtags in their samples.
- If they don't use hashtags, don't add any.

### People Tagging
- Default: Tag relevant people when the post references them.
- Match the person's tagging behavior from their samples.
