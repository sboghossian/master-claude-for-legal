# The Cold-Start Interview

When you install a practice-area plugin in May 2026 or later, the first thing it does is open a customization conversation. Two to fifteen minutes of structured Q&A that produces a `practice-profile.md` file. From that point forward, every session loads the profile and the plugin behaves as if it knows your firm.

Harry's framing in the May webinar: *"Even two minutes upfront or maybe a full fifteen minute interview is gonna eliminate some of those hallucinations that just generative AI makes in general with very little context."*

This is the customization ritual. It is the single highest-leverage thing a legal team can do when adopting Claude. Skipping it produces the off-the-rack-suit experience the early plugin shipped with.

---

## What the interview asks

Six categories of question. Not all plugins ask every question; the more focused the plugin, the shorter the interview. The Legal Clinic plugin asks more supervision questions; the Corporate plugin asks more deal-structure questions; etc.

### 1. Who you are

- Lawyer, legal professional, or non-lawyer?
- In-house, law firm, public service / clinic, or law student?
- Years of practice; familiarity with AI tools.

The "who you are" answers determine which guardrails activate. A non-lawyer answer enables the unauthorized-practice-of-law flag, which prevents the plugin from producing anything that reads like legal advice without an attorney-review step.

### 2. Where you work

- Company / firm name.
- Industry (especially relevant for in-house Commercial and Product Privacy plugins).
- Geographic jurisdictions (US-wide, state-specific, EU, UK, Brazil, etc.).
- Regulatory frameworks that apply (HIPAA, GDPR, BSA/AML, FedRAMP, sectoral US regulators).

The jurisdiction answer is the one that fixes the "US-centric out of the box" problem Mark Pike acknowledged. If you tell the interview you practice in Brazil, the plugin will route research questions to Brazilian connectors and use Brazilian legal vocabulary.

### 3. What you do

- Practice modules within the plugin (e.g. inside Corporate: M&A, financings, board governance, entity management).
- Recurring workflows (NDA triage, deal-room diligence, board materials, regulatory monitoring).
- Frequency / volume of each workflow.

This is what tells the plugin which skills to surface. If you mark M&A as your primary practice module, the tabular-review and deal-room skills come to the front.

### 4. Your playbook

- Existing written playbook? Point me at it.
- No written playbook? Walk through your typical position on three or four recurring issues, or point me at a folder of past decisions and I'll synthesize one.

This was Mark Pike's most actionable advice in the May Q&A: *"A lot of times the like body of decisions that exist inside of a company are scattered in disparate sources. And Claude is so good at helping make sense of all those and helping you create a playbook."* If you don't have a playbook, the interview can build one from past matters in a Slack channel, a Drive folder, or an iManage workspace.

### 5. Your connectors

- Which MCP connectors are wired (Box, iManage, Slack, Outlook, Google Workspace, Harvey, Thomson Reuters, etc.)?
- Read-only or write-enabled?
- Any connectors-of-interest that aren't yet wired but should be flagged?

The plugin uses the connector answer to decide what skills can run end-to-end vs. which ones will need manual drag-and-drop. If you have Box wired, tabular-review can run on a deal room. If you don't, the skill stops at "drag your contracts here."

### 6. Your voice

- Communication style with executives, clients, opposing counsel.
- Length preferences (terse / standard / detailed memo).
- Slack vs email defaults; emoji conventions; formality.
- Any sample emails or memos to ground on.

The voice section is what makes Claude's drafts feel like your drafts. Mark used the example of being a former Slack product counsel: Claude knows his emoji preferences when posting to deal channels.

---

## Quick vs full

Most plugins offer two interview lengths.

**Quick (2-3 minutes).** Asks the minimum to avoid surface-level hallucinations: who you are, where you sit, jurisdictions, primary practice modules. Defaults everything else. Good when you want to demo or try the plugin same-day.

**Full (10-15 minutes).** Asks all six categories. Produces a much more useful practice profile. The right answer for any plugin you'll actually use in production.

**The honest recommendation.** Run Quick for the first thirty minutes to see if the plugin fits your workflow. If it does, run Full before you do any real work. Don't run Full first — you'll burn a half-hour on a plugin you might end up uninstalling.

---

## What the interview produces

A markdown file on your local machine — typically `practice-profile.md` or similar, named per the plugin. The file is loaded into every session running that plugin. It's plain text. You can edit it directly any time, or you can ask Claude to update it (*"update my practice profile — we just rolled out a new vendor MSA template"*).

Example structure (simplified):

```markdown
---
plugin: corporate-legal
last_updated: 2026-05-16
---

## Role
In-house counsel, M&A focus
Plutoco — data infrastructure company, ~600 FTE

## Jurisdictions
Primary: US (Delaware corporate, California operating)
Active matters in: NY, TX, EU (GDPR-relevant)

## Practice modules
- M&A buy-side diligence (primary)
- Commercial contracts (secondary)
- Board governance (light)

## Active deal context
Project Anvil — acquiring Acme Data Assets, Q3 close

## Playbook
- See /firm-playbooks/m-and-a.md for deal terms
- See /firm-playbooks/commercial.md for NDAs/MSAs

## Connectors
- Box (deal rooms, read-write)
- Slack (write needs-approval)
- Outlook (drafts only, never auto-send)
- iManage (read-only)

## Voice
- Direct, brief with deal lead
- Standard memo format for board materials
- Use bullet points; avoid hedging language
```

Treat this file the way you treat a CLAUDE.md or a senior associate's "how I like things done" doc. It's living documentation; it improves with use.

---

## When the interview goes wrong

Three failure modes worth flagging.

### "It assumed the wrong jurisdiction"

The plugin defaulted to US even though you practice in Brazil. Cause: you ran Quick and skipped the jurisdictions question, or the question's wording didn't surface your practice geography.

**Fix.** Open `practice-profile.md` directly and add the jurisdiction. Alternatively, in conversation: *"Update my practice profile — I primarily practice Brazilian commercial law, ground research in Brazilian sources."*

### "It's too generic, the skills still feel off-the-rack"

You ran Full but answered the playbook question with "I don't have one yet." Without a playbook, the plugin defaults to industry-standard positions. Those will feel generic because they are.

**Fix.** Point the plugin at any folder containing past decisions — closed matters, Slack channels with deal-team discussions, archived email threads. Ask it to synthesize a playbook from what's there. Review and edit. This takes one to three hours and is the single biggest customization win.

### "The voice still sounds like Claude, not me"

You skipped the voice section, or you answered it abstractly ("professional but warm" tells the plugin nothing).

**Fix.** Give it three or four concrete samples — past emails to executives, past memos, past Slack messages on deal channels. *"Match this voice."* The grounding sample is worth more than any adjective description.

---

## When to re-run the interview

You don't re-run the full interview from scratch. You update the practice profile.

Re-update the profile when:

- You change roles (in-house → law firm, or change practice groups).
- Your firm adopts a new connector or retires an old one.
- A new jurisdiction enters your matter portfolio.
- Your playbook materially changes (new vendor MSA template, new deal-term standard).
- You realize the plugin keeps assuming something stale (this is a signal the profile is out of date).

The update is conversational: *"My practice profile says we use BoxCorp's NDA template, but we switched to Ironclad's standard last month. Update."* Claude rewrites the file.

---

## The deeper point

The cold-start interview is the move that turns Claude from a smart tool into a coworker who knows your job. Harry's framing — *"Claude is your coworker, not just a tool that you're using or a chatbot"* — only works if you've done the upfront customization.

Skip it, and you'll have a smart chatbot that hallucinates because it has no context. Run it well, and you'll have something that drafts in your voice, references your playbook, and gets sharper every week.

The legal teams that get the most out of Claude are not the ones who buy the most expensive tier or wire the most connectors. They are the ones who spend fifteen minutes upfront answering the interview honestly, and then maintain the practice profile like they'd maintain any other piece of firm infrastructure.

---

## Source

Interview structure and quotes from the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). Failure modes and the "Quick first, Full before production" recommendation are HAQQ deployment patterns, not direct webinar content.
