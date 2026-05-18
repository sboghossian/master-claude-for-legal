# Practice-Area Plugins

In February 2026, Anthropic shipped one generic legal plugin. People dunked on it on LinkedIn — *the skills are too plain, how would this work for our department?* — and Anthropic agreed. In May 2026 they shipped twelve plugins instead of one, each pre-tailored to a practice area and each starting with a customization interview that adapts the skill bundle to the user's firm.

Mark Pike's framing in the May webinar: *"Don't use it out of the box. It's like buying an off-the-rack suit — it's gonna be better when you get it custom tailored."* Plugins are starting templates, not finished products.

This reference is the **lineup** — what each plugin covers and when to reach for it. For the customization ritual itself, see `references/cold-start-interview.md`. For how plugins compose with connectors, see `references/mcp-connector-catalog.md`. For which plugins are also deployable as managed agents, see `references/managed-agents.md`.

---

## The taxonomy

The May 2026 lineup is twelve plugins.

### In-house focused

| Plugin | What it covers |
|---|---|
| **Commercial Legal** | Vendor agreements, NDAs, MSAs, order forms, DPAs — with escalation routing to the right reviewer |
| **Corporate Legal** | M&A diligence, disclosure schedules, board consents, entity management |
| **Employment Legal** | Hires, terminations, classification, leave compliance, separation agreements |
| **Privacy Legal** | DPA review, PIA/DPIA triage, DSAR responses, data-mapping |
| **Product Legal** | Product launch review, internal framework checks, claim substantiation |
| **Regulatory Legal** | Regulatory monitoring, policy gap tracking, agency correspondence |
| **AI Governance Legal** | AI use-case triage, model impact assessments, emerging-tech risk review |

### Law-firm and dual-purpose

| Plugin | What it covers |
|---|---|
| **Litigation Legal** | Matter intake, litigation holds, chronologies, privilege logs, motion drafting |
| **IP Legal** | Trademark clearance, patent prosecution support, cease-and-desist drafting |

### Special-purpose

| Plugin | What it covers |
|---|---|
| **Law Student** | Socratic drilling and bar preparation. Pushes the user toward reasoning rather than handing them conclusions. Guardrailed against acting as legal advice. |
| **Legal Clinic** | Intake, deadline tracking, supervisor-review checkpoints. Built for legal aid clinics serving real clients with attorney supervision. |
| **Legal Builder Hub** | Recursive: a plugin for building plugins. Helps you author new skills, review community plugins for governance compliance, and remix existing ones to fit your firm. |

> Anthropic explicitly said this is "just a start" — Mark called out immigration, environmental, and tax as obvious next plugins. Check the current marketplace before assuming any plugin is or isn't shipped.

---

## Plugins that double as Managed Agents

Four of the twelve plugins are also packaged as **cookbooks** that can be deployed through the Claude API as managed agents (always-on, hosted in Anthropic-managed infrastructure rather than on the user's laptop):

- **Commercial Legal**
- **Corporate Legal**
- **Litigation Legal**
- **Product Legal**

If you want one of these workflows running 24/7 without keeping Cowork open on a laptop, the cookbook → managed-agent path is the deployment route. See `references/managed-agents.md` for the architecture and the human-in-the-loop checkpoints every managed agent should have.

The other eight plugins are Cowork-resident only at launch — that may change as the lineup matures.

---

## How each plugin is structured

Inside any plugin, three kinds of artifacts:

1. **Skills** — markdown files. The procedures. Examples: `nda-review`, `tabular-review`, `clause-comparison`.
2. **Connectors** — the MCP integrations the skills expect to have available. Commercial Legal expects a CLM source (Ironclad, DocuSign, Definely). Litigation expects research + eDiscovery (Thomson Reuters, Midpage, Relativity, Everlaw).
3. **Agents** — pre-wired multi-step workflows that compose skills + connectors into a role (e.g. "the morning intake clerk" that polls a folder, triages, drafts, and queues for review).

All of it is plain markdown in a GitHub repo. Harry's emphasis in the demo: *"All of these folders hold plugins that are written in complete natural language, i.e. English."* If you can read a memo, you can read a plugin.

---

## Picking the right plugin(s)

The honest answer: install more than one. They share a customization profile and they compose. An in-house lawyer doing employment work that touches a board comp committee will want both **Employment Legal** and **Corporate Legal**. A litigator working an IP case will want both **Litigation Legal** and **IP Legal**. A privacy lawyer involved in product launches will want both **Privacy Legal** and **Product Legal**.

The wrong answer: try to find the One Perfect Plugin and install only that. Plugins are not exclusive. The marketplace cost of having three installed is the same as having one.

**Heuristic.** Install the plugin that matches your *primary* work this quarter first. Run the customization interview against your real firm and your real playbook. Use it for two weeks. Then install the second one.

---

## Common overlaps to know

- **Privacy Legal vs Product Legal** — different plugins, often invoked together. Privacy reviews DPAs and DSARs; Product reviews launch readiness, claim substantiation, and internal framework checks. A product launch with personal data triggers both.
- **AI Governance Legal vs Privacy Legal** — different plugins. AI Governance covers use-case triage and impact assessments for AI systems specifically; Privacy covers general data protection. Many AI products trigger both.
- **Corporate Legal vs Commercial Legal** — Corporate is M&A / financings / governance. Commercial is the day-to-day vendor and customer contracts. An M&A deal's diligence work is Corporate; the target's commercial contracts under review are Commercial-shaped work happening inside a Corporate matter.
- **Litigation Legal vs Legal Clinic** — Litigation is full litigation workflow. Legal Clinic is built for legal aid contexts with explicit supervision checkpoints. A clinic doing actual litigation work installs both.

---

## What the plugins are not

They are not legal advice and they are not finished. The Legal Clinic plugin has supervision flags for a reason: every plugin assumes a qualified human is in the loop. If you are a non-lawyer and you install Litigation Legal, the guardrails will route important decisions back to you for attorney review.

They are also not jurisdiction-aware out of the box. Mark admitted this directly in the May webinar: *"There's some US-centricity in the initial plugins."* The customization interview is where you tell it about your jurisdiction (Brazil, EU, England & Wales, etc.) and which local connectors to ground in.

---

## Where the cold-start interview fits

When you install any plugin, the first thing it does is open a customization conversation. *Two minutes upfront or maybe a full fifteen minutes,* Harry said, *is gonna eliminate some of those hallucinations that just generative AI makes in general with very little context.*

The output is a `practice-profile.md` (or equivalent) saved locally. From that point on, the plugin loads this profile into context every session. Full pattern in `references/cold-start-interview.md`.

---

## What to do if the plugin marketplace doesn't show up

This came up in the May webinar Q&A. Two reasons the marketplace might be hidden:

1. **You're not on a Claude tier with Cowork enabled.** Cowork is available on consumer Pro/Max and on Team/Enterprise. Plugins go where Cowork goes.
2. **Your admin has RBAC restrictions in place.** On Enterprise, an admin can gate plugins by role group. Talk to IT.

If neither applies and you still don't see plugins, file an issue against this repo or Anthropic support.

---

## Remix and contribute

All twelve plugins are open source. Anthropic explicitly invited the community to fork, remix, and contribute new ones. The legal-tech vendors hearing this should pay attention: deploying these plugins is a feature path, not a moat.

If you build a plugin for immigration, environmental, tax, criminal defense, family law, or any practice area not yet covered — share it. The Legal Builder Hub plugin will review yours for governance compliance before you publish.

For authoring guidance, see `references/skill-authoring.md`. For the cold-start interview pattern your plugin should ship with, see `references/cold-start-interview.md`.

---

## Source

Lineup and descriptions drawn from Anthropic's *Claude for the Legal Industry* announcement (claude.com/blog, May 2026), the LawNext write-up (lawnext.com, May 2026), and the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). Plugin contents are open source; verify the current shape of any plugin against the live GitHub marketplace before relying on these descriptions for setup.
