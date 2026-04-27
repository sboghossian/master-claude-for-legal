# Skill Authoring

How to write a great legal skill. The format, the patterns, and the recursive trick that gets you from blank page to production in 30 minutes.

---

## What a skill actually is

A markdown file. Frontmatter at the top with `name` and `description`. Body underneath. About 100-300 lines for a typical legal skill. Mark Pike's: *"Just these plain text files that incorporate things like how you would typically do an NDA triage."*

That is the entire format. Everything in this guide is about what to put inside the file.

## The frontmatter

```yaml
---
name: my-skill-name
description: One line that names the workflow, the trigger conditions, and the inputs.
---
```

Two fields that matter.

**`name`** — kebab-case, unique within your skill library. Used for slash invocation (`/my-skill-name`) and identification.

**`description`** — this is where most skill authors underinvest. Claude uses this to decide whether to auto-invoke your skill when the user says something. A vague description means the skill won't get invoked when it should. A precise one means it will.

Bad description: *"NDA triage skill"*

Good description: *"Triage incoming NDAs against a firm playbook, classify each NDA as red/yellow/green risk, identify non-standard clauses, and surface a short list of the highest-risk items for human review. Auto-invokes when the user mentions NDAs, non-disclosure agreements, mutual NDAs, confidentiality agreements, or asks to triage incoming contracts in a folder."*

The good description names the action, the output, and the trigger phrases. Write yours that way.

## The body — the four parts

Every legal skill has four parts. Get the order right and the skill works.

### Part 1 — Role and goal

Open with one paragraph telling Claude who it is in this skill and what the goal is.

> You are a senior transactional associate at [firm name] doing first-pass NDA triage. The goal is to classify each incoming NDA by risk, identify clauses that deviate from the firm's standard playbook, and produce a short triage report that a partner can review in under five minutes.

This anchors the model. Without it, the model's tone and rigor drift across runs.

### Part 2 — Inputs and assumptions

Tell Claude what to expect as input and what to assume.

> ## Inputs
>
> - One or more NDAs in `.docx`, `.pdf`, or `.md` format
> - Optionally: the firm playbook (provided inline or as a file)
> - Optionally: matter context (client name, deal context, prior dealings)
>
> ## Assumptions
>
> - The firm's playbook positions are the defaults. Deviations require flagging.
> - The firm represents the receiving party unless stated otherwise.
> - Confidentiality terms over 5 years are non-standard and must be flagged.

The assumptions section is where you encode firm-specific defaults. Every skill should have one.

### Part 3 — Procedure

This is the bulk of the skill. Walk through what Claude should do, step by step. Number the steps. Be specific.

> ## Procedure
>
> 1. **Read the NDA.** Identify the parties, the effective date, the term, and the governing law.
>
> 2. **Apply the playbook.** For each of the following clauses, compare the NDA's language to the firm's standard position:
>    - Definition of confidential information
>    - Permitted disclosures
>    - Term and survival
>    - Return/destruction obligations
>    - Remedies and equitable relief
>    - Jurisdiction and venue
>
> 3. **Classify risk.**
>    - Red: contains terms that are non-starters (e.g., uncapped indemnification, broad non-compete, perpetual obligations)
>    - Yellow: contains negotiable but non-standard terms
>    - Green: aligns with the firm playbook with minor or no deviations
>
> 4. **Produce the triage output.** Use the format below.

The procedure is a recipe. The model follows it. If the procedure is vague, the output is vague.

### Part 4 — Output format

Specify exactly what the output should look like. Don't leave this to the model.

> ## Output format
>
> Produce a markdown report with the following structure:
>
> ```
> # NDA Triage Report
>
> **Date:** [today]
> **Reviewer:** [user name]
> **NDAs reviewed:** [count]
>
> ## Summary
>
> Brief paragraph: how many in each risk class, key concerns.
>
> ## Per-NDA detail
>
> For each NDA:
>
> ### [Counterparty name]
> **Risk:** Red / Yellow / Green
> **Key issues:**
> - Issue 1 (NDA §X.Y, p. Z)
> - Issue 2 (NDA §X.Y, p. Z)
> **Recommended action:** [redline / accept / escalate]
> ```

The output format is what makes the skill consistent across runs. Pin it down.

## The verification boilerplate

Every legal skill should include this at the bottom. It is the part that keeps your firm off the naughty list. See `references/verification.md`.

```
## Output requirements

- Cite the specific section, paragraph, or page for every factual assertion grounded in a source. Format: (Source §X.Y, p. Z).
- Distinguish source-grounded claims from model-only inferences. Mark model-only claims with the prefix [model inference].
- Quote source language verbatim. If you paraphrase, do not use quotation marks.
- For legal propositions, cite the controlling authority for the relevant jurisdiction.
- If you cannot ground a claim in a provided source, say so explicitly rather than fabricating a citation.
```

Paste this verbatim into every skill. Don't try to write your own version each time.

## The recursive trick

You don't have to write a skill from scratch. The fastest path is to let Claude write the first draft.

The procedure:

1. Open Cowork. Create a project for skill authoring.
2. Drop 10-20 examples of past work that the skill should automate. Past NDA reviews. Past redlines. Past meeting briefs. Whatever the skill targets.
3. Ask Claude: *"Read these examples. Identify the pattern. Write a skill in the standard four-part format (role, inputs, procedure, output) that would automate this work. Use the standard verification boilerplate at the bottom."*
4. Claude produces a 90% draft.
5. You edit. Most edits are small — adjusting tone, adding firm-specific defaults, sharpening the procedure.
6. Save. Test on real work.

This is Mark's recursive trick (he used it for the legal plugin) and it works. A skill that would take you 4 hours to write from scratch takes 30 minutes from examples.

## Voice, tone, and firm style

Robert Graham's question — *"How do you suggest feeding Claude skills that incorporate our voice, tone, and position?"* — is the customization question.

Three places to encode firm voice:

1. **In the role paragraph.** "You are a senior associate at [firm], known for [trait — e.g., 'concise, plain-English memos that avoid hedging']."
2. **In the assumptions section.** "The firm uses plain English. Avoid Latin phrases unless the term has no English equivalent. Don't say 'inter alia'; say 'including.'"
3. **In examples.** Include 1-3 short examples of the firm's preferred output format inside the skill itself. The model will mimic them.

The most effective customization is example-based. If you want the skill to write the way your firm writes, give it three examples of how your firm writes.

## Composition with other skills

Skills can call other skills.

Pattern: `nda-triage` calls `version-diff` when it detects multiple versions of the same NDA, then `citation-verifier` on the final output. The orchestrator skill is short; the heavy lifting happens in the called skills.

To compose, name the called skills explicitly in your procedure:

> 4. If the folder contains multiple versions of the same NDA, invoke the `version-diff` skill to produce a clause-level changelog before triaging.

Claude will dispatch correctly when the skill name matches an installed skill.

This is how you build a firm's *skill library* — small, focused skills that compose into bigger workflows. Don't write monolithic skills that do everything. Write small ones that do one thing well, and orchestrate them.

## Connectors in skills

If a skill needs to read or write through a connector, name the connector explicitly.

> 1. Read incoming emails from the past 24 hours via the **Outlook** connector. Filter for messages with attached `.docx` files where the subject line contains "NDA" or "non-disclosure."

The model will use the named connector. If the connector isn't installed or the skill is run somewhere it isn't available, the skill will fail gracefully and tell the user.

## Skill scope: personal, team, org

When you save a skill, you choose its scope:

- **Personal** — only you see it
- **Team** — you and named teammates
- **Org marketplace** — the whole organization, curated by an admin

Use personal for early drafts. Move to team when you want one or two colleagues to test. Move to org marketplace once the skill is stable and the firm wants to standardize on it.

The org marketplace is where the firm's institutional knowledge lives. If your firm has 50+ lawyers and no one is curating an org marketplace, your firm is leaking compounding leverage. See `references/practice-areas.md` for the rollout pattern.

## Sharing externally

For skills your firm wants to publish (e.g., to GitHub, like this very pack), the format is the same. Drop the skill into a public repository with a README explaining what it does and how to install it.

License consideration: most legal-AI work is collaborative across firms. We license this pack MIT for that reason. Your firm's view may differ; consult counsel.

## Anti-patterns

Things to avoid in skill writing:

- **Vague procedure.** "Review the NDA carefully." What does that mean? Replace with specific steps.
- **No output format.** "Produce a report." What kind of report? Pin it down.
- **Firm-specific assumptions left implicit.** "Apply the firm's standard playbook." Which playbook? Inline it or reference it.
- **Missing verification boilerplate.** Skills without it produce uncited assertions, which is how filings end up on the naughty list.
- **Monolithic scope.** A skill that does everything does nothing well. Decompose.
- **Stale skills.** Skills you wrote six months ago and haven't touched since may not reflect current firm practice. Schedule periodic reviews.

See `references/anti-patterns.md` for examples of each.

## Iteration loop

Skills are not write-once. They are written, used, edited, used, edited. The iteration cadence:

- **First version** (30 min): produced from examples via the recursive trick.
- **First real run** (15 min): use on a real document. Identify gaps.
- **Second version** (15 min): edit to close gaps.
- **Stable use** (1-2 weeks): use daily. Note frustrations. Don't edit yet.
- **Periodic refinement** (every 2-4 weeks): batch the noted frustrations into one editing session.

Don't edit after every run. The single-edit-per-run mode produces churn without consolidation. Batch the edits.

## Testing skills

Before you put a skill in front of a teammate or push it to the org marketplace, run it on:

- A typical case (the workflow it was designed for)
- An edge case (an unusual document, a missing input, a malformed source)
- A negative case (a document that *shouldn't* trigger the skill — does the skill correctly decline or escalate?)

If the skill handles all three, it's ready. If it fails on edge or negative cases, fix those before sharing.

## Versioning skills

When you make a meaningful change to a skill, note it. A simple convention:

```yaml
---
name: nda-triage
description: ...
version: 0.4.2
last_updated: 2026-04-27
---
```

The version field isn't enforced; it's documentation. It helps you and your teammates know whether you're using the current skill.

## How to use this reference

When the user is writing or remixing a skill:

1. Walk through the four-part structure (role, inputs, procedure, output).
2. Surface the verification boilerplate. Don't let them ship a legal skill without it.
3. Recommend the recursive trick if they're starting from scratch.
4. If they have one skill working and are about to write a second, talk through composition. The library effect is more valuable than any single monolithic skill.
5. Help them pick a scope (personal/team/org). Most early skills should start personal.

## Source

Skill format derived from Anthropic's published skill documentation; the recursive trick is Mark Pike's; voice/tone customization patterns are HAQQ's editorial expansion.
