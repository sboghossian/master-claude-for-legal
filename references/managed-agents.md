# Managed Agents

A skill is a procedure. A managed agent is a *role*. The skill says "how to triage an incoming NDA." The managed agent is "the NDA intake clerk who runs every morning, watches the inbox, triages new ones against the playbook, drafts the redlines, queues them for review, and tells the team."

Anthropic announced managed agents alongside the May 2026 legal stack. Mark Pike's framing: *"Many of these plugins also work as managed agents. They can help with multi-step matters end-to-end, they can look out on the horizon and be always-on — so that they can see if a new law has come out and trigger a workflow based on that, or if a development happens in a case it can alert you to that and let you know that it needs to kick off something from your playbook."*

The shift this represents: from Claude-as-tool (you go to it) to Claude-as-coworker (it operates in the background, and brings you something).

---

## What makes an agent "managed"

Three properties, all of which a managed agent has and a regular skill doesn't:

1. **Always-on.** It runs without you invoking it. Scheduled, event-triggered, or polling — but never waiting for the user to type a prompt.
2. **Multi-step.** It composes skills and connectors into a sequence. Read the inbox → triage → draft → file → notify. Not just one skill.
3. **Externally hosted execution.** "Managed" specifically means Anthropic (or your firm's deployment) runs the agent in their infrastructure, not on the user's laptop. Pamela's regulatory tracker doesn't stop running when her MacBook lid is closed.

---

## The four cookbook-deployable plugins

Four of the twelve practice-area plugins ship as **cookbooks** deployable through the Claude API as managed agents:

- **Commercial Legal** — vendor contract review, NDA triage, MSA negotiation support
- **Corporate Legal** — M&A diligence, board governance, transactional support
- **Litigation Legal** — matter intake, holds, chronologies, privilege logs
- **Product Legal** — launch review, framework checks, claim substantiation

The other eight plugins (Privacy Legal, Employment Legal, Regulatory Legal, AI Governance Legal, IP Legal, Law Student, Legal Clinic, Legal Builder Hub) are Cowork-resident at launch. That lineup may expand.

If you want one of the four cookbook plugins running 24/7 as a managed agent, this is the deployment route. If you want one of the other eight as a managed agent, you'll need to either wait for the cookbook or build your own composition.

---

## When to use a managed agent vs. a scheduled task

Both run on a schedule. The distinction:

| Scheduled task | Managed agent |
|---|---|
| Runs one skill on a cron | Composes multiple skills + connectors |
| Lives in Cowork on your machine | Runs in Anthropic-managed infra |
| Stops when your laptop sleeps | Always-on |
| Best for: digests, simple summaries | Best for: end-to-end matter workflows |

The cron-friendly daily regulatory digest is a *scheduled task*. The "monitor every regulatory body, when a relevant change drops, alert the right team, pull our playbook for the affected practice area, draft a position memo, and ping the GC if it's material" is a *managed agent*.

---

## Five legal patterns that want to be managed agents

### 1. Regulatory horizon-scanner

Polls relevant regulatory bodies (EU AI Act updates, FTC actions, state PIPL changes, sector-specific regulators). On a hit, classifies relevance to the firm's matter portfolio, drafts a position note, alerts the right partner.

The May webinar called this out specifically — Mark's colleague Pamela on the regulatory operations team. Originally she spent two hours a morning reading regulatory news. Now the agent produces a custom newspaper she reads with coffee.

### 2. M&A diligence intake clerk

Watches the deal-room connector (Box, ShareFile, virtual data rooms). When new documents arrive, runs the tabular-review skill against the firm's diligence checklist. Updates the running Excel. Flags high-severity findings to the deal lead.

### 3. Litigation case monitor

Polls court dockets (PACER, state court systems, Free Law Project). On a docket event relevant to an active case — new filing, ruling, scheduling order — alerts the case team and pre-drafts the response based on the case playbook.

### 4. NDA intake and triage

Watches a shared inbox or folder for incoming NDAs. Triages each against the firm's NDA playbook. Drafts redlines in track-changes. Queues for attorney review. Sends approved versions back to requesters.

### 5. Compliance certification cycle

Quarterly: pulls relevant controls from the GRC system, runs a self-assessment skill against current evidence, identifies gaps, drafts remediation tasks, files with the compliance team's tracker.

---

## What managed agents are not

They are not autonomous decision-makers. Every legal-relevant output passes through a human review gate before it becomes a real action. The pattern is *draft and queue*, never *draft and send*.

They are not a replacement for the matter team. The agent handles the busy work (intake, triage, first drafts, alerts). The lawyers handle judgment.

They are not a way to deploy legal work to non-lawyers. Anthropic explicitly built guardrails into the legal plugins (the unauthorized-practice-of-law flag, the citation-verification requirement, the privilege header insertion) so that an agent can't accidentally produce something that looks like advice to a non-lawyer audience.

---

## The human-in-the-loop pattern

Every managed agent should be designed around three checkpoints:

1. **Trigger checkpoint.** What event fires the agent? A schedule, an inbox arrival, a docket event, a connector webhook. The trigger should be specific enough that you can audit "why did this run?".
2. **Decision checkpoint.** At least one step where the agent stops and asks. For drafts, the question is "does this match the playbook?". For sends, the question is "send now or queue for partner review?".
3. **Logging checkpoint.** Every action logged with enough context to reconstruct what the agent saw, what it decided, and what output it produced. For privileged work, the audit trail is not optional.

If a managed agent doesn't have all three, it's not ready for client-touching work. It might be ready for internal-tooling work; review carefully before promoting.

---

## Privilege and confidentiality posture

Managed agents inherit the privilege posture of the underlying Claude tier and the connectors they use. Two questions to answer before deploying a managed agent in privileged territory:

1. **Where does the agent execute?** Anthropic-managed infrastructure or your firm's deployment. Confirm against the four-layer privilege framework (`references/privilege-layers.md`).
2. **What does it touch?** Read-only connectors are safe defaults. Anything that mutates state — sends, modifications, deletions — should require human approval inside the agent definition, not just at the connector permission level.

The MCP hardening rules (`references/mcp-hardening.md`) apply to managed agents twice over: once at the connector layer, once at the agent definition. Belt and suspenders.

---

## Deploying a managed agent: the four steps

1. **Write the skills it needs.** An agent is a composition of skills. If the skills don't exist as standalone, the agent will be brittle.
2. **Verify the connectors.** Test each connector the agent will use against a representative dataset before wiring them into the agent. Don't debug at agent runtime.
3. **Run it manually for two weeks.** Treat the agent like a new associate. You shadow every run, review every output, correct every mistake. Each correction goes into the playbook the agent reads.
4. **Promote to always-on.** Only after the two-week shadow period. The agent should now be making zero corrections per week. Schedule it. Review weekly. Audit monthly.

Skipping step 3 is the most common deployment failure. Don't.

---

## When *not* to build a managed agent

- The work happens less than once a week.
- The work is highly judgment-laden and the playbook can't be written down.
- The work touches a system whose state mutations can't be reversed (financial transactions, court filings, regulatory submissions). For these, build the agent up to the *draft* step; never let it execute the submission.
- Your firm hasn't yet built the underlying skills. Build skills first; compose them into agents second.

---

## Source

Concept and framing from the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike). Pamela's regulatory tracker pattern is referenced in both the April and May webinars; the multi-step composition framing is new in May. Operational guidance (four deployment steps, three checkpoints, when-not-to) is HAQQ deployment experience rather than direct webinar content.
