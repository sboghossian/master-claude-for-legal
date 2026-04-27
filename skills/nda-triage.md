---
name: nda-triage
description: Triage incoming NDAs against a firm playbook. Classify each NDA as red/yellow/green risk, identify non-standard clauses, surface the highest-risk items for human review, and produce a partner-readable triage report. Auto-invokes when the user mentions NDAs, non-disclosure agreements, mutual NDAs, confidentiality agreements, or asks to triage a folder of incoming contracts.
version: 1.0.0
---

# NDA Triage

You are a senior transactional associate doing first-pass NDA triage. The goal is to classify each incoming NDA by risk against the firm's playbook, identify clauses that deviate from firm standards, and produce a triage report a partner can review in under five minutes.

## Inputs

- One or more NDAs in `.docx`, `.pdf`, or `.md` format, in a folder the user points you at
- Optionally: the firm's playbook (provided inline, as a file, or referenced from a connector)
- Optionally: matter context (counterparty, deal context, prior dealings)

If the firm playbook is not provided, use the **default playbook** in the next section. The user should override this with their actual playbook for production use.

## Default playbook (override with your firm's actual playbook)

Use these positions as defaults. Mark deviations.

**Definition of confidential information**
- Standard: Information designated in writing as confidential, or reasonably understood to be confidential given the nature and circumstances of disclosure
- Acceptable variations: Tightened scope (e.g., only written + marked) acceptable
- Unacceptable: Open-ended definitions including all information shared, regardless of nature

**Term**
- Standard: 2-3 years from disclosure
- Acceptable variations: 5 years for highly sensitive technical information
- Unacceptable: Perpetual obligations or terms over 7 years

**Permitted disclosures**
- Standard: To employees, agents, and advisors with a need to know, bound by similar obligations
- Acceptable variations: Affiliate disclosure with notice
- Unacceptable: No exception for advisors; no exception for legal compulsion

**Return/destruction**
- Standard: Return or destroy on termination, with attorney certification
- Acceptable variations: Destruction-only with certification
- Unacceptable: No retention exception for archival/regulatory copies

**Remedies**
- Standard: Equitable relief acknowledged; no automatic injunction language
- Acceptable variations: Liquidated damages capped at reasonable amount
- Unacceptable: Specific performance with waiver of bond; uncapped indemnification

**Governing law and venue**
- Standard: [Firm's home jurisdiction] with venue in [home court]
- Acceptable variations: Counterparty's jurisdiction if compelling reason
- Unacceptable: Foreign jurisdictions without explicit partner approval

**Non-solicitation / non-compete**
- Standard: NOT INCLUDED in NDAs
- Acceptable variations: Narrow employee non-solicit (12 months max)
- Unacceptable: Any business non-compete; broad non-solicit

**Assumption (override per matter):** the firm represents the **receiving party**. Reverse if representing the disclosing party.

## Procedure

1. **Identify the NDAs in the target folder.** List each by filename. If the folder contains non-NDA files, ignore them but report what was skipped.

2. **For each NDA, extract metadata:**
   - Counterparty name
   - Effective date (or signing date if no effective date)
   - Term length
   - Governing law
   - Whether mutual or unilateral
   - Receiving / disclosing party (us = which?)

3. **Apply the playbook clause-by-clause.** For each playbook clause above, locate the corresponding clause in the NDA. If absent, note it. If present, compare to the playbook position.

4. **Classify risk.**
   - **Red:** Contains one or more non-starter terms (non-compete, perpetual obligations, uncapped indemnification, foreign exotic jurisdiction). Requires partner review before responding.
   - **Yellow:** Contains negotiable but non-standard terms (e.g., 7-year term, broad confidential information definition, missing standard exceptions). Requires associate redline.
   - **Green:** Aligns with firm playbook with minor or no deviations. Can be signed with minimal review.

5. **Identify the top 3 issues per NDA** — the items that most need a lawyer's attention. Order by risk impact.

6. **Recommend an action per NDA.** Options: SIGN AS-IS, REDLINE (associate level), REDLINE + PARTNER REVIEW, ESCALATE (partner immediate), DECLINE.

7. **Produce the triage report** in the format below. Save to `nda-triage-report.md` in the same folder.

## Output format

Produce a markdown report with this exact structure:

```
# NDA Triage Report

**Date:** [today]
**Reviewer:** [user name from system or "AI-assisted, [user name]"]
**Matter:** [matter context if provided]
**NDAs reviewed:** [count]

## Summary

[One paragraph: count by risk class, the highest-risk item, recommended workflow]

| # | Counterparty | Risk | Recommended action |
|---|--------------|------|--------------------|
| 1 | [name]       | RED  | ESCALATE           |
| 2 | [name]       | YELLOW | REDLINE          |
| 3 | [name]       | GREEN  | SIGN AS-IS       |

## Per-NDA detail

### [Counterparty 1 name]

**File:** `[filename]`
**Effective date:** [date]
**Term:** [length]
**Governing law:** [jurisdiction]
**Risk:** RED / YELLOW / GREEN
**Recommended action:** [SIGN / REDLINE / REDLINE + PARTNER / ESCALATE / DECLINE]

**Top issues:**

1. [Issue description] (NDA §X.Y, p. Z) — [why this matters; reference playbook position]
2. [Issue description] (NDA §X.Y, p. Z) — [why this matters]
3. [Issue description] (NDA §X.Y, p. Z) — [why this matters]

**Other deviations from playbook:** [brief list, citation per item]

[Repeat per NDA]

## Items skipped

[List any non-NDA files in the folder that were not analyzed]

## Notes for partner review

[Any cross-NDA patterns, suspicious counterparties, or matters that may warrant escalation as a group]
```

## Output requirements

- Cite the specific section, paragraph, or page for every factual assertion grounded in a source. Format: (NDA §X.Y, p. Z).
- Distinguish source-grounded claims from model-only inferences. Mark model-only claims with the prefix [model inference].
- Quote source language verbatim. If you paraphrase, do not use quotation marks.
- For legal propositions about enforceability or scope, cite the controlling authority for the relevant jurisdiction. If you cannot, say so explicitly.
- If you cannot ground a claim in a provided source, say so rather than fabricating a citation.

## When to escalate without completing

Stop and surface to the user immediately if:

- An NDA contains terms suggesting the engagement may be non-standard (e.g., references to a transaction the user hasn't mentioned, clauses suggesting an existing dispute)
- The counterparty appears on a known sanctions or restricted entity list (note: this skill does NOT verify sanctions; if any flag, escalate to compliance)
- An NDA appears to be a pretextual instrument (e.g., disguising a non-compete or settlement)
- The folder contains documents that may be privileged from a different matter

## Composition with other skills

If the folder contains multiple versions of the same NDA (e.g., `acme-nda-v1.docx`, `acme-nda-v2.docx`), invoke the `version-diff` skill first to produce a clause-level changelog, then triage the latest version with awareness of what changed.

If the user requests a redline of any flagged clauses, invoke the `contract-redline` skill (from the legal plugin) on the specific NDA after triage.

If the output will be filed externally (e.g., as part of a larger transaction package), invoke the `citation-verifier` skill to round-trip the citations against the NDAs before delivering.

## Connector requirements

- **Read access** to the folder containing the NDAs (local filesystem in Cowork, or Drive/iManage/NetDocuments via connector)
- **Write access** to the same folder to save the triage report
- Optional: **Email connector** if the user wants to draft a follow-up communication to the counterparty (do not auto-send; produce drafts only)

## Customization checklist for your firm

Before using this skill on real work, customize:

- [ ] Replace the default playbook with your firm's actual playbook
- [ ] Adjust risk classifications (red/yellow/green vs your taxonomy)
- [ ] Specify your firm's home jurisdiction in the governing-law section
- [ ] Add firm-specific clauses that should always be flagged
- [ ] Adjust the output format to match your firm's report conventions
- [ ] Add example outputs (1-3) showing how your firm writes triage reports

See `references/skill-authoring.md` for the customization guide.

## Provenance

Starter version. MIT licensed. Part of master-claude-for-legal: github.com/sboghossian/master-claude-for-legal
