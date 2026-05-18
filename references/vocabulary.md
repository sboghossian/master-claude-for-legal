# Vocabulary

The glossary the webinar should have started with. Rebecca Wright's question — *"can we get a glossary?"* — was the most-upvoted moment of plain-English honesty in the room. This is it.

Two reading paths. **Quick** is the table — keep it open while you work. **Deep** is the prose underneath each entry, with usage notes and pitfalls.

---

## Quick reference

| Term | One-line definition |
|------|---------------------|
| **Skill** | A markdown file that teaches Claude how to do one specific thing |
| **Plugin** | A bundle of skills shipped together |
| **Connector (MCP)** | A live data pipe to one of your systems (DMS, email, calendar, etc.) |
| **Cowork** | Anthropic's collaborative desktop application; the IDE for non-engineers |
| **Claude Code** | Terminal-based CLI; same engine as Cowork, developer surface |
| **Claude.ai chat** | Browser chat; quickest entry point, lowest ceiling |
| **Claude for Word / Excel / PowerPoint** | Sidebar inside the respective Office app |
| **Project** | A shared workspace inside Claude with files, instructions, connectors |
| **Artifact** | A live output (dashboard, doc, mini-app) generated in conversation |
| **Agent** | A long-running, multi-step automation that uses skills + connectors |
| **MCP** | Model Context Protocol — the open standard for connectors |
| **RBAC** | Role-based access control; available on Enterprise tier |
| **Compliance API** | Audit log + DLP + eDiscovery export; Enterprise tier |
| **Schedule** | A recurring task (cron-like); Pamela's regulatory tracker uses this |
| **CLAUDE.md** | A project-level instruction file Claude reads before responding |
| **Managed agent** | A multi-step, always-on agent hosted by Anthropic (not your laptop); see `references/managed-agents.md` |
| **Cold-start interview** | The customization Q&A that runs the first time you install a plugin; produces a practice profile |
| **Practice profile** | The markdown file the cold-start interview produces; loaded into every session that uses the plugin |
| **Plugin marketplace** | The Cowork surface (Customize → Plugins) where you browse and install plugins |
| **Tabular review** | Many docs, same schema, output as spreadsheet with cell-level citations; see `skills/tabular-review.md` |
| **Practice-area plugin** | One of the 12 plugins shipped in May 2026, pre-tailored to a legal practice area |

---

## Skill

A markdown file. That is it. About 100-300 lines of plain English describing how to do one workflow — review an SOW against a playbook, draft a privilege log, triage incoming NDAs.

**Format.** Frontmatter at the top with a `name` and `description`, then the body. The description matters because Claude uses it to decide when to invoke the skill automatically. A good description names the workflow, the trigger conditions, and the inputs.

**Where they live.**

- *Personal skills* — only you can see and use them.
- *Team skills* — shared with one or more colleagues.
- *Org marketplace skills* — curated by an admin, available across the firm.

**How they get invoked.** Three ways: by slash command (`/triage-nda`), by name in plain text ("use the NDA triage skill"), or automatically when the user's request matches the skill's description ("there are five NDAs in the inbox folder, what should we do" — Claude infers the right skill).

**The most important thing about skills.** Mark Pike's quote: *"You wouldn't wear a suit you bought off the rack."* The legal plugin's skills are starting templates. They are not finished products. The leverage is in the customization. See `references/skill-authoring.md` for authoring; `references/cold-start-interview.md` for the customization onboarding ritual that runs the first time you install a plugin.

## Plugin

A bundle of skills shipped together as a single installable unit. The Anthropic legal plugins are plugins. Your firm's internal skill library, once you have one, is also a plugin — a private one.

**Install path.** Cowork → Customize → Plugins → Anthropic and Partners → [pick plugin] → Install. Two minutes.

**Plugins can include.** Skills (the main payload), connectors (less common), CLAUDE.md instructions, scheduled tasks (rare), agents (added May 2026).

**Plugins cannot include.** Account credentials, model weights, or anything binary. They are markdown + config, full stop.

**The May 2026 lineup.** Twelve practice-area plugins replaced the single generic legal plugin: Commercial Legal, Corporate Legal, Employment Legal, Privacy Legal, Product Legal, Regulatory Legal, AI Governance Legal, IP Legal, Litigation Legal, Law Student, Legal Clinic, Legal Builder Hub. Four (Commercial, Corporate, Litigation, Product) are also deployable as managed agents via cookbooks. See `references/practice-area-plugins.md` for the full lineup; see `references/cold-start-interview.md` for the customization ritual each plugin opens with.

## Connector (MCP)

A live data pipe. The bridge between Claude and one of your systems.

**Common connectors.** Microsoft 365, Google Workspace, Slack, Notion, Linear/Jira. The May 2026 launch added a named legal-specific catalog spanning CLM (Ironclad, DocuSign, Definely), DMS (iManage, NetDocuments, Box), deal rooms (Datasite, Box), eDiscovery (Relativity, Everlaw, Consilio), legal research (Thomson Reuters CoCounsel, Midpage, Trellis, Legal Data Hunter), legal AI (Harvey, Solve Intelligence, Lawve AI, the L Suite's Lloyd and TopCounsel), and access-to-justice (Free Law Project, Descrybe, Courtroom5, BoardWise).

For the **what's available** question, see `references/mcp-connector-catalog.md`. For the **how to wire it safely** question, see `references/mcp-hardening.md`.

**How they authenticate.** OAuth, almost always. You log in to the system once; Claude has standing access until you revoke.

**Permission grid.** Every connector has actions: read, send, modify, delete. Each action can be set to *always allow*, *needs approval*, *blocked*, or *custom*. **Default rule for legal use: read actions can be always-allow; everything that mutates state (send, modify, delete) should be needs-approval.** See `references/mcp-hardening.md`.

**The thing nobody mentions.** Connectors are persistent. They are not one-shot pulls. Treat them like OAuth grants — audit them, revoke them when not in use, never authenticate personal accounts to firm tools.

## Cowork

Anthropic's collaborative desktop application. Generally available as of April 2026. Where most legal work belongs.

**What makes it different from chat.**

- File system access. Claude can open folders, read files, write files, run things in parallel.
- Agentic harness. Long-running, multi-step tasks that decompose work into bounded sub-tasks. See `references/long-documents.md`.
- Skills + plugins. Cowork is the surface where you install and run skills.
- Projects. Shared workspaces for team collaboration on a matter or initiative.
- Schedules. Recurring tasks, like Pamela's regulatory tracker (2 hours/day → automated).

**When to use Cowork instead of chat.** Any time the work touches more than one file, more than one tool, or more than one step. If you find yourself copy-pasting between a chat window and a doc, you should be in Cowork.

## Claude Code

Terminal-based CLI. Same engine as Cowork. For engineers and technically-comfortable lawyers.

**Maggie's analogy.** Code is to Cowork what a terminal is to an IDE. Same capability, different surface, different audience.

**When to use Code over Cowork.** Almost never, if you're a lawyer. Cowork has everything Code has, with a UI. The exception: if your firm has engineers who built infrastructure on Code (skills, MCPs, CLAUDE.md), and they're handing it over to you, you can stay on Code or migrate to Cowork without losing anything. The skills and configs are plain text files; they work in either surface.

## Claude.ai chat

The browser-based chat interface. The quickest way to start using Claude. Also the lowest ceiling.

**When chat is enough.**

- Ad-hoc questions
- First drafts of an email or memo
- Research conversations on a single topic
- Quick translations or summaries
- Anything that fits in one conversation and one window

**When chat hits its ceiling.**

- Long documents (50+ pages — context rot starts here)
- Multi-step workflows
- File operations
- Anything you'd want to schedule
- Anything that requires the model to use a real file system

For legal work, chat is a starting place. Move to Cowork as soon as the work warrants it.

## Claude for Word / Excel / PowerPoint / Outlook

Sidebar add-ins inside Microsoft Office. As of May 2026, all four surfaces are live and share **cross-surface context preservation** — the same Claude session, plugin, and practice profile follow you across all four apps without losing matter context.

- **Word** — redlining and drafting with track changes. The most legal-relevant surface.
- **Excel** — tabular review output, spreadsheet-resident analysis. Demoed in May with 30 M&A contracts.
- **PowerPoint** — board briefings, client updates, matter summaries from the matter record.
- **Outlook** — draft email in your voice. Never auto-send.

**Install path.** Word → Add-ins → Claude. Sign in once; the auth shares across all four apps.

**What it shares with Cowork.** Skills, plugins, practice profile, conversation context. The skill you wrote in Cowork is available in the Word add-in. The playbook you trained against an iManage matter folder works when you're staring at a docx in Word. Pillar 4 in action.

**Requirements.** A paid Microsoft 365 license. Office 2019 or earlier won't work. If your firm is still on Office 2019, this is on the IT roadmap.

For the full cross-surface context model, see `references/microsoft-365.md`.

## Project

A shared workspace inside Claude. Holds:

- Files (uploaded or live via connectors)
- Instructions (a CLAUDE.md or equivalent)
- Connector configurations
- Conversation history (for those with access)

**Use projects per-matter.** One project = one matter. This is the deployment-layer move that maps Claude onto your firm's privilege model. See `references/privilege-layers.md`.

**Sharing.** Add team members directly. Their access mirrors what you'd give a colleague on a matter team.

## Artifact

A live output Claude generates inside a conversation. Could be a dashboard, a small app, an interactive document. You can interact with it right there.

**Examples.**

- A filterable table of incoming NDAs with risk scores
- A small calculator for damages estimation
- A regulatory dashboard summarizing this week's developments
- A meeting brief that updates when new context arrives

**Sharing artifacts.** Host at a stable URL. Schedule the underlying generation to run on a cron. Link from your team Slack or wiki. The pattern is "the artifact is the deliverable; the chat is the workshop." Don't ask people to read your prompts.

## Agent

A long-running, multi-step automation that uses one or more skills and connectors to complete a task end-to-end.

**Agent vs skill.** A skill is the procedure ("how to triage an NDA"). An agent is the role ("the NDA intake clerk who runs every morning, triages anything new, drafts the redlines, and emails the team").

In Cowork, when you wire a scheduled task that uses a skill against a watched folder and produces an artifact, you have built an agent. You may not have used the word.

## Managed agent

A subtype of agent that's hosted by Anthropic (or your firm's managed deployment) rather than running on the user's laptop. Always-on. Event-triggered or scheduled. Composes multiple skills + connectors into a workflow.

The distinction: a scheduled task in Cowork runs when your laptop is awake. A managed agent runs in Anthropic-managed infrastructure and doesn't care whether your laptop is open.

See `references/managed-agents.md` for the deployment patterns and the human-in-the-loop checkpoints every managed agent should have.

## MCP

Model Context Protocol. The open standard for connectors. Mark called it the "USB-C of AI."

The relevance to lawyers: MCP is what makes Pillar 1 (live data) work. Any system that speaks MCP can be wired to Claude. Increasingly, that's most enterprise SaaS.

For non-engineers: you do not need to understand MCP at the protocol level. You just need to know that a "connector" is an MCP integration, and that systems with first-class MCP support are easier to wire than systems without.

## RBAC

Role-based access control. Available on Claude Enterprise.

What it lets you do: scope skills, plugins, and connectors to specific user groups. Real Estate group sees Real Estate skills. Litigation sees Litigation. The transactional team can't accidentally invoke a litigation-specific skill on a deal.

If your firm has multiple practice groups that need information barriers, RBAC is the reason to upgrade from Team to Enterprise.

## Compliance API

Enterprise-tier feature. Programmatic access to:

- Audit logs (who did what, when)
- Data loss prevention hooks
- eDiscovery export

What it's for: regulatory inquiries, internal investigations, compliance reviews. If a regulator asks "show me everything Claude saw about Matter X between dates A and B," the Compliance API is how you answer.

## Schedule

A recurring task. Pamela's regulatory tracker is the canonical example. Runs every day at 8 a.m., reads the morning's regulatory news, synthesizes, publishes to a Google Site.

**Set up via.** `/schedule` slash command in Cowork.

**Common schedules for legal teams.**

- Daily regulatory digest
- Weekly newsletter / status synthesis
- Twice-daily filing deadline check
- End-of-week timesheet reconciliation
- Monthly contract renewal alerts

If you have a piece of recurring work that takes more than 30 minutes a week and is structurally amenable to AI, it's a candidate for a schedule.

## CLAUDE.md

A project-level instruction file. When Claude opens a project, it reads CLAUDE.md first to understand the context.

**What goes in it.**

- Project description
- Conventions and house style
- Key terminology
- Important constraints (privilege scope, regulatory framework, jurisdiction)
- Pointers to related skills and references

For a legal matter, your CLAUDE.md might say: *"This is matter 24-1827, a stock purchase transaction. Buyer is X, seller is Y, target is Z. The deal team is on this Slack channel. The DMS folder is at this iManage path. Apply the firm's standard transactional playbook unless overridden in conversation."*

---

## How to use this reference

When the user uses one of these terms ambiguously, or asks "what is X," reach for this file before answering. The whole webinar audience was confused by the vocabulary. Don't replicate that confusion.

When introducing a term in conversation, use the one-line definition first. Drop into the deep version only if the user asks or the situation needs it.

## Source

Vocabulary derived from the *Claude for Legal Teams* webinar (Anthropic, April 2026), Anthropic public documentation, and HAQQ's deployment notes across legal customers.
