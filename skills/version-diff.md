---
name: version-diff
description: Produce a clause-level changelog across multiple versions of the same legal document. Identify which counterparty made which changes, flag substantive modifications, distinguish style edits from substance. Auto-invokes when the user mentions multiple versions, redlines, version comparison, "what changed between drafts," or points at a folder containing versioned files.
version: 1.0.0
---

# Version Diff

You are a senior associate doing version comparison across multiple drafts of a legal document. The goal is to produce a clean, clause-level changelog that distinguishes substantive changes from style edits, identifies which counterparty likely made each change, and surfaces the items the deal lawyer needs to focus on.

This skill answers Aishwarya Belle's webinar question: *"As a transactional lawyer, I struggle with tracking which version of the document reflects changes made by multiple parties. Can Claude give a comparison for that?"*

## Inputs

- A folder containing multiple versions of the same document (typical naming: `agreement-v1.docx`, `agreement-v2.docx`, etc., or `agreement-2026-04-01.docx`, etc.)
- Optionally: a CLAUDE.md or matter-context file describing the parties and the negotiation history (who returned which draft when)
- Optionally: the prior version's track changes if Word-format with track changes preserved

## Procedure

### Phase 1 — Plan

1. List the files in the target folder. Identify the versions and order them chronologically (by filename convention, file modification date, or explicit version number).
2. Extract the document name (the consistent base across versions).
3. If the user has not specified parties, ask: "Who are the parties to this agreement, and which one are we representing?"
4. If a CLAUDE.md or context file is present, read it for negotiation history.
5. Write `version-diff-plan.md` to the folder, describing the version sequence and the comparison plan.

### Phase 2 — Process (per version pair)

For each adjacent pair of versions (v1→v2, v2→v3, etc.):

1. Read both versions.
2. Identify changes by section / clause. For each change:
   - The clause name or section number
   - The before text (verbatim, with citation)
   - The after text (verbatim, with citation)
   - The classification: SUBSTANTIVE / STYLE / TYPO / FORMATTING
   - If SUBSTANTIVE: the legal effect of the change (who benefits, what shifted)
   - The probable originating party (based on whose interest the change serves; mark as [model inference])
3. Write `diff-v[N]-to-v[N+1].md` to the folder with the per-section changelog.

For long agreements (50+ pages), follow the long-document pattern in `references/long-documents.md`: decompose by section, parallel sub-process where possible, write intermediate files.

### Phase 3 — Synthesize

After processing all version pairs:

1. Read all per-version diff files.
2. Build a clause-level history: for each clause that was touched, show the evolution across versions.
3. Identify cross-version patterns:
   - Clauses that have been renegotiated repeatedly (these are deal points still in dispute)
   - Clauses that flipped (one party changed it, the other reverted, the first party changed it again)
   - Clauses where one party's position has hardened
4. Surface the items the deal lawyer should focus on for the next round.
5. Write the final report to `version-diff-report.md` per the output format.

## Output format

```
# Version Diff Report

**Document:** [agreement name]
**Versions analyzed:** [list]
**Date of analysis:** [today]
**Parties:** [list]
**We represent:** [party name]

## Negotiation timeline

| Version | Date | Returned by | Substantive changes | Style/typo only |
|---------|------|-------------|---------------------|-----------------|
| v1      | ...  | ...         | n/a (initial)       | —              |
| v2      | ...  | ...         | 4                   | 12             |
| v3      | ...  | ...         | 2                   | 6              |

## Open items as of latest version

[Bullet list of clauses still in active negotiation, ranked by importance to our side]

## Clause-level history

For each clause that has been touched across any version, show evolution:

### [Clause name, e.g., "§8 Indemnification — cap"]

| Version | Text (excerpt) | Effect | Originating party |
|---------|----------------|--------|-------------------|
| v1      | "...capped at the contract price" | Initial position favoring us | Us (drafter) |
| v2      | "...uncapped" | Removes cap; favors them | Counterparty |
| v3      | "...capped at 2× contract price" | Compromise position | Us |
| v4      | "...capped at 2× contract price plus fees" | Slightly broadened | Counterparty |

**Status:** Active negotiation. Recommend next-round position: [...]

[Repeat per touched clause]

## Style and formatting changes (summary)

[Brief summary of non-substantive changes; not detailed unless user requests]

## Recommended next-round priorities

1. [Clause name]: [our recommended position; rationale]
2. [Clause name]: [our recommended position; rationale]
3. [Clause name]: [our recommended position; rationale]
```

## Output requirements

- Cite the specific section and version for every quoted excerpt. Format: (v3 §8.2, p. 14).
- Quote source language verbatim. Paraphrases must not appear in quotation marks.
- Distinguish SUBSTANTIVE / STYLE / TYPO / FORMATTING with consistent rigor.
- Originating-party attributions are inferences. Mark them [model inference] unless the user has provided explicit negotiation history.
- For legal effect descriptions, be specific about who benefits. "This change benefits the counterparty by removing the cap" is more useful than "this change is favorable."
- If you cannot identify a change cleanly (e.g., the version had a substantial restructuring), describe the restructuring and identify the substantive impact rather than fabricating clean diffs.

## When to escalate

- If a version contains content that doesn't appear in the prior version OR the next version (suggesting a side draft was inserted), flag it. This may indicate a side letter or parallel negotiation thread.
- If the file naming or modification dates suggest the versions are not in the order they appear chronologically (e.g., a "v3" with an earlier mod date than "v2"), surface the inconsistency rather than guessing.
- If the document substance shifts in a way that suggests a different agreement structure (e.g., what was an asset purchase becomes a stock purchase), stop and confirm with the user.

## Composition with other skills

After running version-diff, common follow-ups:

- Use the `contract-redline` skill (legal plugin) to draft the next round's response
- Use the `citation-verifier` skill if the diff report will be shared externally
- Use the `meeting-brief` skill to prep the next negotiation call with the open items

## Connector requirements

- Read access to the folder containing the versions
- Write access to the same folder for diff files and the report

## Customization checklist for your firm

- [ ] Add your firm's clause-naming conventions if they differ from generic
- [ ] Adjust the negotiation timeline format to match your firm's deal-tracker conventions
- [ ] Add example reports showing how your firm writes version-diff outputs
- [ ] If your firm uses a specific deal taxonomy (e.g., "deal-critical / negotiable / boilerplate"), encode it

## Provenance

Starter version. MIT licensed. Part of master-claude-for-legal: github.com/sboghossian/master-claude-for-legal
