# Master Claude for Legal

> A community skill pack for legal teams. Set up Claude correctly for privileged work, customize the legal plugin without faceplanting, and ship your first production legal automation in an afternoon.
>
> Maintained by [HAQQ Legal AI](https://haqq.ai). MIT licensed. Contributions welcome.

---

## What this is

A self-contained skill pack you can install into Claude (Cowork, Claude Code, or any tool that supports Anthropic-format skills) that teaches the model — and you — how to do legal work with Claude *the way Anthropic's own legal team does it*, without the rough edges they didn't get to in their April 2026 webinar.

It comes in four layers.

**Reference docs** (`references/`). Ten short documents covering the four-pillar architecture, the four privilege layers, the vocabulary that confused the room at the webinar, MCP permission hardening, citation verification, the agentic harness pattern for long documents, practice-area patterns (transactional, litigation, IP, regulatory, in-house), the four-hour setup checklist, a skill-authoring guide, and a list of anti-patterns we've seen blow up in production.

**Starter skills** (`skills/`). Five working skills you can install today: NDA triage, multi-party version diff, meeting brief, citation verifier, status synthesis (the "Friday newsletter" pattern). Each is a markdown file. Each is meant to be remixed for your firm — the legal plugin Anthropic ships is a starting template, not a finished product, and so is this.

**Templates** (`templates/`). Three documents most firms need but few have written: a firm AI policy template, a one-page client-facing explainer for "how does our firm handle your data when using AI," and a vendor security questionnaire for assessing legal-AI tools.

**Data** (`data/`). The full transcript of the source webinar, the 51 audience questions with upvotes and themes, and machine-readable JSON for anyone who wants to do their own analysis. We did the work of cleaning the transcript and structuring the question dataset; this directory is the raw output of that effort.

## Why this exists

In April 2026, Anthropic ran a webinar called *Claude for Legal Teams*. Twenty thousand lawyers registered. The chat submitted 51 questions and upvoted them 2,470 times. About half got answered live. The other half didn't, and several of the answers that did happen were partial.

We built this skill pack as the long version of that webinar. The full conversation, with the parts we think were missing.

The companion long-form analysis is at [haqq.ai/blog/claude-for-legal-teams-questions-answered](https://haqq.ai/blog/claude-for-legal-teams-questions-answered) — a piece that goes through the rank-ordered questions, what Mark Pike and Maggie Russo said live, and where we'd add to the answers.

## How this was built — the full process

For people who want to know how the analysis was done (and want to do their own version against future Anthropic webinars), here is the actual workflow.

### Step 1 — The artifact that started it

A friend of the project shared a Claude artifact organizing the webinar's 51 questions by theme — built in React/TSX, color-coded by concern, sortable by upvote count. That artifact had the raw data: every question text, author, upvote count, and theme classification. The full source of that file is at the bottom of this README for transparency.

That artifact was the input. Everything else in this repo was downstream of it.

### Step 2 — Downloading the source webinar

The webinar was hosted on Anthropic's Goldcast on-demand library. The recording URL was a CloudFront-backed MP4. We used `yt-dlp` against the Goldcast page URL; it discovered the underlying CDN URL automatically. Total download: 808 MB, 57 minutes of video, runtime 3,407 seconds.

```bash
yt-dlp -o "webinar.%(ext)s" "https://anthropic.ondemand.goldcast.io/on-demand/<id>"
```

### Step 3 — Extracting and transcribing the audio

Audio was extracted to 16 kHz mono PCM WAV (the format `whisper.cpp` expects):

```bash
ffmpeg -y -i webinar.mp4 -ar 16000 -ac 1 -c:a pcm_s16le audio.wav
```

The resulting file was 104 MB. Transcription used `whisper.cpp` with the `small.en` model on Apple Silicon (Metal-accelerated). The `small.en` model is the right tradeoff for legal terminology — `base.en` was fast but missed some legal-specific names; `medium.en` was overkill for a clear, single-microphone webinar.

```bash
whisper-cli -m ggml-small.en.bin -f audio.wav -of transcript -otxt -ovtt
```

Transcription completed in about three minutes. Output: 591 lines of plain text plus a .vtt file with timestamps. Both are in `data/`.

### Step 4 — Diffing live answers against audience questions

With the transcript in hand, we mapped each of the 51 questions to its status in the live session:

- **Answered live** — Mark or Maggie addressed the question substantively
- **Partially answered** — touched on but not fully answered
- **Not addressed** — did not get answered live (these are the questions our skill pack covers)

Of the 51:

- 25 answered live (49%)
- 11 partially answered (22%)
- 13 not addressed (25%)
- 2 reactions/off-topic (4%)

The mapping is in `data/webinar-questions.json` and the human-readable version with full text is at `data/webinar-questions.md`.

### Step 5 — Writing the long-form blog post

A 12,800-word analysis ran through the rank-ordered themes, the four pillars Mark Pike presented, the four-layer privilege framework, the long-document architecture (Cowork's agentic harness vs. chat's "lost in the middle" problem), the document-zoo gaps the webinar didn't address (scanned PDFs, JPEGs, multi-version diffs), the integration reality, the surface comparison (Cowork vs Code vs Chat vs Word), the practice-area expansion (transactional / litigation / IP / regulatory / in-house), the four-hour setup checklist, and the unanswered questions.

The blog post is the long-form companion to this skill pack. The skill pack is the operational version.

### Step 6 — Building the skill pack

Every reference document, starter skill, and template in this repo was extracted from the same source material. Where the webinar said something useful, we cited Mark or Maggie verbatim. Where the webinar didn't go deep enough, we expanded with patterns we've seen across HAQQ's deployments to ~9,800 firms. Where the webinar didn't address a question at all, we wrote the answer.

The provenance trail is preserved. Every reference doc has a "Source" line at the bottom. Every starter skill has a "Provenance" section. The data directory has the raw inputs anyone can use to verify our work or build a different version.

### What you can do with this approach

If Anthropic runs another legal webinar — or any vertical-specific one — the same workflow applies:

1. Get the recording (Goldcast, YouTube, etc. — `yt-dlp` handles most)
2. Extract audio (`ffmpeg`)
3. Transcribe (`whisper.cpp` with `small.en` for English)
4. Diff what was answered against what was asked
5. Build a skill pack for the gaps

The whole cycle, end-to-end, takes about a day. The hard part isn't the technical work; it's the editorial judgment about what to include and how to organize it. That part scales linearly with the writer's experience in the vertical.

We're sharing the workflow because the legal-AI category needs more people doing this kind of analysis, not fewer. If you want to do it for your own vertical and want help scaffolding the skill pack format, open an issue.

---

## Who this is for

- **In-house legal teams** at companies where IT is willing to enable Cowork and connectors but the legal team is on its own to figure out skills and policy.
- **Law firms** of any size considering or piloting Claude for legal work, especially the small and mid-size firms whose case-management vendors haven't shipped MCP connectors yet.
- **Legal ops leads** building the plumbing other people will use.
- **Solo practitioners** who want to start with the legal plugin and need a sane on-ramp.
- **Researchers and journalists** writing about the legal-AI category — the `data/` directory is the dataset.
- **Other legal-tech vendors** building on top of Claude — the references and starter skills are MIT-licensed reference material you can fork and adapt.

It is *not* a substitute for legal advice on AI policy at your specific firm. The privilege framework, the policy template, and the client explainer are starting points. Run them past your own counsel.

## Install

Three options depending on your tooling.

### Cowork (recommended for legal teams)

```bash
git clone https://github.com/sboghossian/master-claude-for-legal.git
cd master-claude-for-legal
# Open Cowork → Customize → Plugins → Local → Add → point to this directory
```

The skills in `skills/` will appear under your personal plugins. Reference docs in `references/` are accessible to Claude within any Cowork session — just mention them by name (e.g., "use the privilege-layers reference") and Claude will load the relevant file.

### Claude Code

```bash
git clone https://github.com/sboghossian/master-claude-for-legal.git ~/.claude/plugins/master-claude-for-legal
# Then in any Claude Code session:
/plugin install master-claude-for-legal
```

### Manual / other Anthropic-format tools

The skill format is plain markdown with YAML frontmatter. Any tool that reads Anthropic-format skills can use these files directly. Drop the `skills/` directory into your tool's skills path.

## Quickstart

If you've never used Claude on legal work before, do these four things in order. Each takes about an hour. After four hours of intentional practice you will have one production-quality skill running on your real work.

1. **Read** [`references/setup-checklist.md`](references/setup-checklist.md) — the four-hour walkthrough from zero to first useful skill.
2. **Install** the legal plugin (Cowork → Customize → Plugins → Anthropic and Partners → Legal → Install).
3. **Pick** one skill from `skills/` that matches a workflow you do every week.
4. **Remix** it for your firm's voice using [`references/skill-authoring.md`](references/skill-authoring.md) as a guide.

That's it. The rest of the references are for when you hit specific problems (privilege configuration, long documents, verification, etc.) and want to go deeper.

## What's in the box

```
master-claude-for-legal/
├── README.md                         (this file)
├── SKILL.md                          (the meta-skill — Claude's entry point)
├── LICENSE                           (MIT)
├── CONTRIBUTING.md
├── .gitignore
├── references/
│   ├── four-pillars.md               Architecture: how Claude knows your legal work
│   ├── privilege-layers.md           Four-layer privilege framework + configuration
│   ├── vocabulary.md                 Skills, plugins, connectors, Cowork, Code, etc.
│   ├── mcp-hardening.md              Permission grids and connector security
│   ├── verification.md               Citation grounding and hallucination control
│   ├── long-documents.md             The 70+ page problem and the agentic harness
│   ├── practice-areas.md             Transactional / Litigation / IP / Reg / In-house
│   ├── setup-checklist.md            Four-hour zero-to-first-skill walkthrough
│   ├── skill-authoring.md            How to write a great legal skill
│   └── anti-patterns.md              What not to do (with real examples)
├── skills/
│   ├── nda-triage.md                 Triage incoming NDAs against a firm playbook
│   ├── version-diff.md               Multi-party clause-level changelog
│   ├── meeting-brief.md              Calendar + email + drive → meeting briefing
│   ├── citation-verifier.md          Round-trip every quoted source
│   └── status-synthesis.md           Friday newsletter / weekly status pattern
├── templates/
│   ├── firm-ai-policy.md             Starter policy you can adopt and edit
│   ├── client-data-explainer.md      One-pager for clients asking about AI use
│   └── vendor-security-questionnaire.md   Assess any legal-AI tool
└── data/
    ├── webinar-transcript.txt        Full transcript of the source webinar (57 min)
    ├── webinar-transcript.vtt        Same, with timestamps
    ├── webinar-questions.json        Structured dataset of all 51 questions
    └── webinar-questions.md          Human-readable version with rankings + status
```

## The data directory

`data/` is here because we believe the underlying material that informed this pack should be public. It includes:

- **Full webinar transcript** — produced via whisper.cpp small.en model, ~10,000 words, 591 lines. Lightly cleaned but not editorialized.
- **51-question dataset** — every audience question with author, upvote count, theme classification, and answer status (answered live / partially / not addressed / reaction).
- **Theme analysis** — the rank-order, distribution, and signal interpretation.

Use it for:

- Verifying anything we wrote against the source material
- Doing your own analysis (different cut, different priorities, different vertical mapping)
- Building visualizations and reports
- Academic or journalistic research on the state of legal-AI in 2026

The structured JSON in `data/webinar-questions.json` is designed for programmatic use. The human-readable version in `data/webinar-questions.md` is for reading.

## Compatibility

Built and tested against:

- Claude Cowork (GA as of April 2026)
- Claude Code (any recent version)
- Claude for Word add-in
- Anthropic Team and Enterprise plans
- The Anthropic legal plugin (this skill pack composes with it; it does not replace it)

Not tested against (but should work):

- Other AI tools that load Anthropic-format skills
- Self-hosted MCP server setups
- Older Claude versions (skills format is forward-compatible)

## Contributing

Pull requests welcome. We particularly want:

- New starter skills for practice areas we haven't covered (immigration, family law, criminal defense, etc.)
- Translations of references into other languages (especially Spanish, Arabic, French, given the global lawyer audience)
- Connector-specific guides (iManage, NetDocuments, Clio, etc.) showing how to wire those systems for legal work
- Real-world examples of skills you've remixed for your own firm — anonymized
- Updates to the question dataset if Anthropic publishes additional Q&A or runs follow-up webinars

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines.

## License

MIT. Use it. Fork it. Sell services around it. Ship it inside your own product. We only ask that the original authorship attribution stays in the README.

The webinar transcript and question dataset are derived from a publicly accessible Anthropic recording and are shared here for educational and research purposes under the same MIT license.

## A word on what this is and isn't

This skill pack is *opinionated*. We've watched a lot of legal teams adopt AI well and badly, and the references reflect choices we've made about what good looks like. You may disagree. That's fine — the files are markdown, edit them.

It is also *honest about limits*. There are workflows where general-purpose Claude is good enough and a legal-specific platform is overkill. There are workflows where the opposite is true. We name both. The references include explicit "use raw Claude here" and "use a specialized platform here" calls where they're warranted.

We make a legal-AI platform ourselves ([HAQQ](https://haqq.ai)) and we use it. We didn't write this to upsell you to it. We wrote it because we think the gap between "what Anthropic's legal team can demo on stage" and "what the median firm can wire up on Monday morning" is too wide, and the open-source community can close it faster than any single vendor can.

If you find this useful, the most helpful thing you can do is share what you remix back. The best skills get incorporated into the next version with attribution.

— The HAQQ team
