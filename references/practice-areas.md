# Practice Areas

The same Claude architecture, five different daily lives. The webinar's demos were transactional; this reference expands to the practice areas the demos didn't cover.

---

## Transactional

The demos' center of gravity. NDA triage, contract redlining, MSA review against a firm playbook, deal point analysis, clause library checks.

**The shape.** Two-to-four hour reviews collapse to twenty-to-thirty minutes when the skill is written well. The Word add-in is where the actual redlining happens, with Cowork serving as the orchestrator for upstream work (intake, triage, classification) and downstream work (email drafting, follow-up).

**Specific Anthropic-team examples from the webinar.**

- Contract review with redlines in 20 minutes (Mark Pike's team)
- Privacy impact assessments compressed from hours to 20-30 minutes
- NDA triage with risk classification (red/yellow/green) and per-clause flagged review

**Highest-leverage starting point.** Privacy impact assessments. Structurally a transactional task (defined input, defined output, repeating template) that takes most legal teams hours to do well. If you're at a tech-adjacent in-house team, this is your first skill.

**Pillars in play.**

- Pillar 1 (live data) — connects to the CLM and the deal data room
- Pillar 2 (skills) — encoded firm playbook
- Pillar 3 (document comprehension) — defined-term tracking, side-letter detection
- Pillar 4 (context across apps) — Cowork → Word → Outlook → calendar

**Skills in this pack that apply.** `nda-triage`, `version-diff`, `meeting-brief` (for deal-team prep), `citation-verifier` (for the rare M&A brief).

---

## Litigation

Mark mentioned his litigation team using Claude for two things specifically: *transcript search* and *expert prep*. There's a third application — discovery review — that he didn't mention but that's high-leverage in production today.

### Transcript search

Historically one of the most expensive billable activities in litigation. A typical large case generates thousands of pages of deposition transcripts. The associate's job is to read every page, mark relevant Q-and-A, assemble a binder of citations.

This is exactly the kind of needle-in-haystack work that AI does well — search the corpus, find every place a witness contradicted themselves, every place an admission was made, every place a defined term was used inconsistently.

**Pattern.** Cowork + a skill that processes transcripts in chunks, indexes Q-and-A by topic and witness, surfaces inconsistencies. Use the long-document pattern from `references/long-documents.md`.

**Output.** A working binder of relevant excerpts with line citations, sortable by issue, witness, or date.

**Time saved.** Days to hours.

### Expert prep

Litigation teams routinely have to prepare experts for testimony — feed them the case background, key documents, expected lines of questioning, opposing-side arguments. Mark's team is using Claude for the synthesis: read the case file, the expert's prior depositions on similar topics, the opposing expert's reports, produce a prep memo.

**Pattern.** Same shape as a meeting brief, just for a much higher-stakes meeting. The `meeting-brief` starter skill in this pack adapts.

**Output.** A focused memo orienting the expert to what they need to know and what they're likely to be asked.

### Discovery review

Not in Mark's webinar mention but worth surfacing.

When you have ten thousand documents to review for privilege and responsiveness, the standard tooling today is keyword-based eDiscovery platforms (Relativity, Everlaw) that produce noisy results and require armies of contract attorneys for first-pass review.

A well-written skill in Cowork can do meaningful first-pass tagging — privilege, responsiveness, document type, key witness, date range — at a fraction of the cost. Not as a replacement for the eDiscovery platform yet, but as a layer on top that prioritizes the human reviewer's queue.

**Pattern.** Cowork with a parallel-processing skill that reads documents from a watched folder, tags them with structured metadata, writes the tags to disk. Schedule the skill to run overnight on new batches. Human reviews start with the highest-priority queue.

**Time saved.** First-pass review effort drops 60-80% in firms running this pattern. The remaining 20-40% is humans validating and handling edge cases.

### Real-time trial assist

Andrew the paralegal at the elder abuse trial. Tool sat at counsel's table, pulled cross-examination angles in real time, surfaced prior testimony that contradicted current answers.

**Pattern.** API-based; not a packaged skill. A custom tool built against the Anthropic API for the specific case. This is currently bespoke work; expect packaged solutions to emerge in the next 18 months.

**When to consider it.** High-stakes trials where the cost of building a custom tool is justified by the marginal value of real-time AI assist. Most often pro bono cases (where the small team is fighting BigLaw) or matters where the firm has the engineering muscle to build it once and reuse for similar cases.

---

## Intellectual property

Mark's IP team example from the webinar: patent prioritization. Scanning product briefs and GitHub repositories to surface patentable ideas. The model reads engineering work in real time and flags moments when something patentable was created.

### Patent prioritization

**Pattern.** Cowork with connectors to GitHub, Drive (product specs), and Slack (engineering channels). A skill that runs on a schedule, scans for new technical disclosures, classifies novelty, surfaces a shortlist for the IP team.

**Pillars.** Pillar 1 doing heavy lifting — the model is reading the engineering work happen, not summarizing a stale snapshot.

**Output.** A weekly digest of patentable ideas with novelty assessment, prior-art links, and recommended priority.

### Prior art search

The pattern: structured search across patent databases, scientific literature, and product disclosures to identify prior art for a claim or invention.

**Connector requirements.** USPTO/EPO patent search if available; Google Scholar via API; product database connectors.

**Reality check.** Specialized IP-search platforms (PatSnap, Innography, etc.) currently outperform raw Claude with skills on prior art recall. Use them. Claude with skills is a useful complement for narrative analysis once you have the candidate prior art set.

### Claim drafting and prosecution

The pattern: drafting claim language from a technical disclosure, comparing claim sets across versions, responding to office actions.

**Reality check.** This is the most jurisdiction-specific work in legal AI. US patent practice is different from European patent practice. EPO opposition procedures differ from JPO. The skill you write must encode the relevant jurisdiction's claim conventions.

**Composition.** Often pairs with patent docketing systems via MCP. The bottleneck is connector availability for the specific docketing tool your firm uses.

---

## Regulatory

Pamela's regulatory tracker is the canonical example. Two hours a day of manual synthesis became a scheduled job. The deliverable improved. The whole legal team gets to read it instead of just one analyst.

### Daily regulatory digest

**Pattern.** Cowork + scheduled task + connectors to news feeds, government databases, and internal compliance docs. A skill that runs every morning, reads the day's regulatory developments, synthesizes against the firm's practice areas, publishes to a Google Site or Slack channel.

**Pillars.** All four. Live data via MCP (Pillar 1), encoded skill that knows what counts as relevant to the firm (Pillar 2), structural understanding of regulatory text (Pillar 3), and the published artifact lives where the team consumes it (Pillar 4).

**Output.** Daily newsletter readable in 5 minutes, replacing 2 hours of analyst work.

The starter `status-synthesis` skill in this pack is a generalization of this pattern. Adapt for regulatory work by changing the input sources and the output format.

### Compliance gap analysis

**Pattern.** Cowork with a skill that takes a regulatory framework (e.g., GDPR, HIPAA, a specific FDA guidance) and a description of firm practice, identifies gaps, prioritizes by risk and likelihood of enforcement.

**Output.** A gap matrix with remediation recommendations.

### Client alert drafting

**Pattern.** When a regulatory development affects clients, draft a client alert in the firm's voice. Skill that reads the development, identifies the affected client industries, drafts the alert, formats per firm style.

**Time saved.** Client alerts drop from 2-3 hours to 20-30 minutes. Lawyer review focuses on accuracy and partner sign-off.

---

## In-house counsel

Mark himself is essentially in-house — he's a lawyer at a tech company. His most relatable example, and the one he closed the webinar with, was the Friday newsletter.

### Status synthesis (the Friday newsletter)

Every Friday, Mark's team sends a "what we did this week" update to cross-functional stakeholders. He hated doing it manually. He fed Claude past newsletters as the high-bar template, pointed it at the team's Slack and ticket activity, asked it to produce the draft.

His specific instruction: *"Don't be sycophantic and just tell me we did your work. Show me what's actually impactful based on what other people are saying."*

**Pattern.** Cowork + Slack and Linear/Jira connectors + scheduled task running Friday morning. Skill produces a draft based on the week's actual activity. Human edits and sends.

**Time saved.** 1 hour to minutes.

The starter `status-synthesis` skill in this pack is built for this exact pattern.

### Cross-functional translation

In-house counsel routinely translates between legal, product, engineering, and executive language. AI is good at this.

**Pattern.** Skill that takes input in one register (e.g., a legal memo, a regulatory finding) and produces output in another (e.g., an exec summary, a product-team explainer). Often paired with a connector to the relevant platform (Slack, Notion, Linear).

**Time saved.** Hours per week of context-switching effort.

### Contract intake routing

The pattern: incoming contracts get triaged by type, urgency, and risk; routed to the right reviewer; tracked in a queue.

**Pattern.** Cowork + connector to email or shared inbox + skill that classifies and routes. Composes with `nda-triage` for the NDA stream.

**Output.** A clean queue per reviewer, with risk-flagged items at the top.

### PIA generation

Privacy impact assessments. Same as the transactional pattern but with regulatory framework awareness (GDPR, CCPA, sectoral rules).

---

## Choosing where to start

If your firm or team is just beginning, pick one workflow per practice area:

- **Transactional →** NDA triage. Highest volume, clearest playbook.
- **Litigation →** Transcript search. Highest cost reduction per hour invested.
- **IP →** Patent prioritization. Highest leverage from connecting to engineering work.
- **Regulatory →** Daily digest. Most visible win for the team.
- **In-house →** Status synthesis. Most relatable; everyone has one of these.

Build one. Ship it. Iterate. Then build the second.

## How to use this reference

When the user describes a workflow that doesn't immediately map to a starter skill in this pack:

1. Identify the practice area.
2. Pull the relevant section above.
3. Suggest the closest starter skill from this pack and how to adapt it.
4. If no skill in this pack applies, walk through what a custom skill would look like — input sources, processing steps, output format.
5. Be honest about reality checks. Some workflows (deep IP prior art, large eDiscovery) have specialized tooling that beats raw Claude today.

## Source

Anthropic team examples come from Mark Pike's webinar and the Pamela regulatory-tracker story Maggie Russo told. Discovery review, claim drafting, and the cross-functional patterns are HAQQ's editorial expansion based on observed deployments.
