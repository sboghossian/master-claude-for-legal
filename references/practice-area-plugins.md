# Practice-Area Plugins

In February 2026, Anthropic shipped one generic legal plugin. People dunked on it on LinkedIn — *the skills are too plain, how would this work for our department?* — and Anthropic agreed. In May 2026 they shipped twelve plugins instead of one, each pre-tailored to a practice area and each starting with a customization interview that adapts the skill bundle to the user's firm.

Mark Pike's framing in the May webinar: *"Don't use it out of the box. It's like buying an off-the-rack suit — it's gonna be better when you get it custom tailored."* Plugins are starting templates, not finished products.

This reference is the **lineup** — what each plugin contains and when to reach for it. For the customization ritual itself (the cold-start interview), see `references/cold-start-interview.md`. For how plugins compose with connectors, see `references/mcp-connector-catalog.md`.

---

## The taxonomy

Twelve plugins shipped in May 2026. Three groupings:

### In-house focused

- **Commercial Legal** — vendor and customer contracts, SOWs, order forms, DPAs, MSAs
- **Product Privacy** — product launches, privacy reviews, DPIA / PIA workflows, data-mapping
- **Employment** — offer letters, separation agreements, performance documentation, employment policies
- **Employment Governance** — board/comp-committee materials, equity, executive comp documentation
- **Regulatory** — regulatory monitoring, response drafting, agency correspondence, compliance memoranda
- **Corporate** — entity management, M&A diligence, board governance, financing transactions

### Law-firm and dual-purpose

- **Litigation** — case strategy, deposition prep, motion drafting, discovery workflows, transcript search
- **IP** — patent and trademark prosecution, IP diligence, freedom-to-operate, invention disclosure review

### Special-purpose

- **Law Student (Socratic Method)** — guardrailed for learning, not answering. Pushes the user toward reasoning rather than handing them conclusions. Useful for law students, summer associates, and lawyers learning a new area.
- **Legal Clinic** — built-in supervision flags for clinics serving real clients. Flags anything that needs attorney review before it goes out.
- **Legal Builder Hub** — recursive: a plugin for building plugins. Helps you author new skills, review community plugins for governance compliance, and remix existing ones to fit your firm.

> The exact plugin set is a snapshot of May 2026. Anthropic explicitly said this is "just a start" — Mark called out immigration, environmental, and tax as obvious next plugins. Check the current marketplace before assuming any plugin is or isn't shipped.

---

## How each plugin is structured

Inside any plugin, three kinds of artifacts:

1. **Skills** — markdown files. The procedures. Examples: `nda-review`, `tabular-review`, `clause-comparison`.
2. **Connectors** — the MCP integrations the skills expect to have available. Commercial Legal expects a CLM / contracts source; Litigation expects a research connector and an eDiscovery connector.
3. **Agents** — pre-wired multi-step workflows that compose skills + connectors into a role (e.g. "the morning intake clerk" that polls a folder, triages, drafts, and queues for review). See `references/managed-agents.md`.

All of it is plain markdown in a GitHub repo. Harry's emphasis in the demo: *"All of these folders hold plugins that are written in complete natural language, i.e. English."* If you can read a memo, you can read a plugin.

---

## Picking the right plugin

The honest answer: install more than one. They share a customization profile and they compose. An in-house lawyer doing employment work for a board comp committee will want both Employment and Employment Governance. A litigator working an IP case will want both Litigation and IP.

The wrong answer: try to find the One Perfect Plugin and install only that. Plugins are not exclusive. The marketplace cost of having three installed is the same as having one.

**Heuristic.** Install the plugin that matches your *primary* work this quarter first. Run the customization interview against your real firm and your real playbook. Use it for two weeks. Then install the second one.

---

## What the plugins are not

They are not legal advice and they are not finished. The legal-clinic plugin has supervision flags for a reason: every plugin assumes a qualified human is in the loop. If you are a non-lawyer and you install Litigation, the guardrails will route important decisions back to you for attorney review.

They are also not jurisdiction-aware out of the box. Mark admitted this directly in the May webinar: *"There's some US-centricity in the initial plugins."* The customization interview is where you tell it about your jurisdiction (Brazil, EU, England & Wales, etc.) and which local connectors to ground in.

---

## Where the cold-start interview fits

When you install any plugin, the first thing it does is open a customization conversation. *Two minutes upfront or maybe a full fifteen minutes,* Harry said, *is gonna eliminate some of those hallucinations that just generative AI makes in general with very little context.*

The interview asks:

- Are you a lawyer? A legal professional? A non-lawyer?
- Where do you sit (in-house, firm, public service, student)?
- Jurisdiction(s)?
- What specific practice modules do you work in (e.g. M&A inside Corporate, depositions inside Litigation)?
- What connectors are available?
- Any existing playbook to ground against?

The output is a `practice-profile.md` (or equivalent) saved locally. From that point on, the plugin loads this profile into context every session. You don't run the interview again — but you can update the profile any time by saying *"update my practice profile to reflect..."*.

Full pattern in `references/cold-start-interview.md`.

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

Lineup, descriptions, and customization framing drawn from the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). Plugin contents are open source; verify the current shape of any plugin against the live GitHub marketplace before relying on these descriptions for setup.
