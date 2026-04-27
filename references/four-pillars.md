# The Four Pillars

How Claude actually knows your legal work. Mark Pike's framing from the April 2026 *Claude for Legal Teams* webinar, with operational notes added.

---

This is the spine. Almost everything else in this skill pack composes back to one of these four ideas. If you read only one reference document in this pack, read this one.

## Pillar 1 — Live data via Model Context Protocol (MCP)

Mark called MCP the "USB-C of AI." It is an open protocol that lets Claude connect to your live systems — matter management software (iManage, NetDocuments), CLM, Drive, Outlook, the Microsoft suite, Slack, calendar, your case management system if it speaks MCP.

The point is not that Claude *uploads* a snapshot of your work; it is that Claude *reads the same files your team does*, live. The redline that lands at 4 p.m. is visible to Claude at 4:01 p.m. without anyone reuploading anything. The deal calendar that updates when associate B moves the close date is the deal calendar Claude sees the next time you ask about scheduling. Stale snapshots are the regime AI was stuck in for three years. Live data is the regime that makes legal AI useful in production.

**What this pillar enables.** Any workflow that depends on freshness — meeting prep, deadline tracking, current matter status, "what's the latest on X." Any workflow that crosses systems — pulling email + calendar + drive + DMS into a single brief. Any workflow that has to answer questions about a moving target.

**What you have to do.** Wire connectors. Each connector is an OAuth-style grant that gives Claude standing read access to a system. Audit them. Set write actions to "needs approval" by default. See `references/mcp-hardening.md` for the full configuration.

**What this pillar does not do.** It does not magically make your DMS searchable if your DMS is a mess. The data quality problem is upstream of AI. Garbage in, garbage out, faster.

## Pillar 2 — Legal skills

A "skill" in this world is a markdown file that codifies a workflow your team already runs every week. NDA review. Contract redlining. Privilege log drafting. Matter intake. Clause library checks. Precedent search. Deal point analysis.

Mark's framing is the part to internalize: *"Claude doesn't just start from a blank page on the work you do hundreds of times a year. It pulls from that corpus of knowledge that you've created within your department."*

Skills make institutional muscle memory portable. The senior associate's playbook for negotiating limitation-of-liability clauses can be written as a 200-line markdown file. Then every junior associate has access to the senior associate's judgment, encoded. The skill becomes a force multiplier across the team.

Skills are *recursively buildable*. You can ask Claude to write skills for you. Hand it 15 examples of past redlines and say "extract the playbook." It will produce a 90% draft of the skill in 30 minutes, which you then clean up. This is the highest-leverage move in the entire stack and the part most firms haven't started doing.

**What this pillar enables.** Encoded firm style. Junior associates working at senior judgment quality. Workflows that would otherwise live in one person's head becoming a firm asset.

**What you have to do.** Write or remix at least one skill against a workflow you do every week. Then a second. Then a third. By month three you should have a personal library of ten to twenty.

**What this pillar does not do.** It does not replace judgment. Skills are scaffolding for routine; the judgment calls remain yours.

## Pillar 3 — Document comprehension

This is the pillar most people underrate.

Claude reads agreement structure the way a lawyer does. It tracks defined terms across exhibits and schedules. It understands that the indemnification clause references the limitation clause references the carve-out in Schedule 4. It explains in plain English what a clause actually does and flags exactly where the risk sits. It is not keyword search. It is not text summarization. It is structural comprehension of how legal documents hold together.

The capability gap here is enormous. "Summarize this MSA" is a trivial task. "Find every place this MSA's standard terms are quietly overridden by a side letter" is real lawyering work, and Pillar 3 is what makes it possible to delegate.

For long documents (50+ pages), Pillar 3 composes with the agentic harness — see `references/long-documents.md` for how to keep accuracy high across documents that don't fit cleanly in a single prompt.

**What this pillar enables.** Cross-reference tracking. Side-letter detection. Inconsistent-term flagging. Version-to-version diff at the clause level. Schedule mapping. The structural work that used to be associate-hours.

**What you have to do.** Use it in skills. When you write a skill, ask the model to read structure, not just text. The phrasing matters: "find every place X is overridden" gets better results than "summarize sections about X."

**What this pillar does not do.** It does not interpret novel legal questions. Comprehension is structural; interpretation is judgmental. Don't conflate them.

## Pillar 4 — Context across apps

The redline you ran in Word becomes a summary slide in PowerPoint, becomes an email draft in Outlook, becomes a follow-up calendar invite. Claude carries the context all the way through.

Mark's exact phrase: *"The work moves with you."*

This is the pillar that turns "AI tool" into "AI teammate." Without Pillar 4, every handoff between tools requires the user to restate context. With it, the work flows. A meeting brief generated in Cowork can be opened in Word, edited there, exported to PowerPoint as slides, and the model still knows what the meeting is about.

The "office agent trilogy" Mark named — Word, Excel, PowerPoint — plus email is where most knowledge work for lawyers happens. Claude has first-class presence in all four today.

**What this pillar enables.** End-to-end workflows that span tools. The final email at the end of a deal review is informed by the redline at the start. No re-explaining at any handoff.

**What you have to do.** Use the surfaces. Stop treating Claude.ai chat as the only place AI lives. Open the Word add-in. Open Cowork. Open the Excel and PowerPoint add-ins. Skills propagate across all of them.

**What this pillar does not do.** It does not replace your collaboration tools. Slack is still where the team coordinates *about* the work; Claude is where the work happens. The seam is intentional.

---

## The architectural bet

Mark said it on stage and it is worth quoting verbatim because it explains why Anthropic is building this way and not another way:

> *"You don't need to fine-tune models to give Claude the engineer a legal degree. Instead, you just need to give these tools access — the same tools that the lawyers use every day to get their work done. And that's what helps it become a great teammate within the legal context."*

The bet: a single capable model + the same surfaces lawyers already work in + skill/connector/matter customization in the layer above = legal AI. No fine-tuning. No bespoke legal model. The tailoring happens in the layer between the model and the user, not in the model weights.

This bet is correct *for transactional work, in-house legal departments, and most regulatory work*. It is partially correct for litigation (the Pillar 1 + Pillar 2 + Pillar 4 combination is strong; specialized eDiscovery tools still beat raw Claude on first-pass document review at very high volumes). It is partially correct for IP (raw Claude with good skills handles patent prioritization and prior art search well; specialized claim-drafting tools have edges in niche areas).

For most of legal practice, in 2026, the architectural bet is the right bet.

## How to use this reference

When you (the model) are responding to a user's request inside `master-claude-for-legal`:

1. Identify which pillar(s) the request touches.
2. If it spans multiple pillars, mention the composition explicitly. Pillar 1 + Pillar 4 = "live data flowing across surfaces" is a different shape than Pillar 2 + Pillar 3 = "encoded skill applied to structural comprehension of one doc."
3. If the user is asking *why* something works the way it does (e.g., "why does Cowork exist as a separate product"), the answer is almost always "to enable Pillar X better than chat could." Reach for the pillar.
4. Avoid jargon for jargon's sake. The pillars are a map, not a marketing slide. If the user is satisfied without hearing the framework, don't impose it.

## Source

Direct framing and quotes from Mark Pike, Anthropic legal product lead, *Claude for Legal Teams* webinar, April 2026.
