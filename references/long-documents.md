# Long Documents

The 70-page problem and how to solve it.

---

Anonymous, 101 votes, fifth-most-upvoted question of the *Claude for Legal Teams* webinar:

> *"Legal work can involve 70+ page documents. With detailed context and tool calls, there's increased risk of context rot and degraded output. How does Claude's legal team address this?"*

This reference is the long answer.

## The problem is real

Large language models have a phenomenon called *lost in the middle*: as you stuff more text into a single conversation, the model's accuracy on details from the middle of that text degrades. This happens even with a million-token context window. The model can technically *see* everything; it doesn't reliably *attend* to everything.

For legal work — where the difference between paragraph 27 of an exhibit and paragraph 28 can be the entire question — this is not theoretical. A model that misses paragraph 27 because it was buried in the middle of a 200-page filing has produced a defective work product.

## Why chat hits a wall

Chat interfaces (Claude.ai, ChatGPT) operate in a regime where the entire document goes into one prompt. Either it fits in the context window or it doesn't. Either way, the model gets one pass.

For documents under ~50 pages of dense legal text, this is fine. For documents above that threshold, accuracy degrades visibly. Above ~100 pages, you should expect material omissions.

This is the regime that produces stories like "the AI summarized the contract but missed the cross-reference in Schedule 4 that flipped the indemnification logic." Schedule 4 was buried in the middle of the prompt. The model didn't attend to it. The summary was wrong.

## How Cowork solves it

Mark Pike's answer in the webinar:

> *"This is why I love Cowork so much. The agentic harness allows things to swarm and make plans and execute and distill a lot of the mass corpus of knowledge that legal teams often have or encounter in their work and make sense of it all. In chat-based or turn-based conversations with AI, yeah, you get that degradation. But when Claude is able to make use of a local computer, store files in the right places, and use these types of file systems, we've really found that it's able to keep up and it doesn't have that context rot."*

Translation. Cowork gives Claude access to a real file system on your laptop. Claude does what a human associate would do:

1. Open the document
2. Split the analysis into stages
3. Write intermediate notes to disk
4. Read the notes back when needed
5. Use parallel sub-processes for independent sub-tasks
6. Synthesize at the end

The 70-page document doesn't have to live in the prompt; it lives on disk, and the model fetches the parts it needs when it needs them.

This is what the word "agentic" actually means in this context. Not "AI that does things" — that's the marketing version. The technical version is "AI that uses tools, including a file system, to extend its working memory beyond what fits in a single prompt."

## The pattern

A long-document skill in Cowork should follow this shape:

```
1. PLAN
   Read the table of contents or first 3 pages of the document.
   Decompose the document into bounded sections.
   Write a plan to plan.md describing the sections and the analysis to apply per section.

2. PROCESS (in parallel where possible)
   For each section:
     - Read the section
     - Apply the per-section analysis
     - Write the result to section-N-notes.md
     - Cite paragraphs and page numbers in the notes

3. SYNTHESIZE
   Read all section-N-notes.md files.
   Reconcile inconsistencies.
   Produce the final output with citations to the original document
     (Source §X.Y, p. Z) — see references/verification.md for the format.

4. VERIFY
   For every claim in the final output, confirm the cited section actually
   contains the claimed content. Flag any claim that fails this check.
```

Skills in this pack that use this pattern: `skills/version-diff.md`, `skills/nda-triage.md` (for NDAs over 20 pages), and any skill that involves analyzing long contracts, briefs, or filings.

## The synthesis trap

The agentic harness is not magic. A skill that calls itself fifteen times to analyze fifteen sections can drift in style or reach inconsistent conclusions across sections. Section 3 might use one definition of "material adverse change" while section 9 uses another, and the synthesis stage has to catch and reconcile that.

If you write your own skills for long-document work, plan for the synthesis stage explicitly. The pattern:

> "After processing all sections, read every section's notes file. Identify any place where two sections describe the same concept differently. Reconcile by referring to the original document and citing the controlling section. Produce one coherent output, not fifteen partial ones."

Without this instruction, you get fifteen partial answers stitched together. With it, you get one coherent output.

## When chat is enough vs when you need Cowork

| Document length | Recommended surface | Why |
|------------------|---------------------|-----|
| Under 20 pages | Chat or Cowork | Chat works fine; Cowork is overkill |
| 20-50 pages | Chat with care, or Cowork | Watch for missed details if using chat |
| 50-100 pages | Cowork | Context rot starts compounding |
| 100-200 pages | Cowork (agentic skill required) | Single-prompt accuracy not reliable |
| 200+ pages | Cowork + decomposition + synthesis | The full pattern above |

For a 50-page MSA review, chat will probably work. For an 800-page bond indenture or a thousand-page deposition transcript, you must be in Cowork with a properly decomposed skill.

## Specific applications

### Multi-volume discovery

The pattern: a folder of documents, each potentially long, where the question is "find all the documents responsive to X" or "identify the documents privileged on grounds Y." Use Cowork. Process each document independently. Tag with classifications and citations. Synthesize at the end into a queue prioritized for human review.

### Deposition transcripts

The pattern: a long transcript where the question is "find every place this witness contradicted themselves" or "find every admission relevant to issue X." Use Cowork. Process the transcript in chronological chunks. Build a running index of relevant Q-and-A pairs with line citations. Synthesize into a usable binder for trial.

### Long contracts (M&A, indentures, complex MSAs)

The pattern: 100-500 pages of deal documents, often with cross-referencing schedules and exhibits. Use Cowork with a `version-diff` or `comprehensive-review` skill. Decompose by section. Track defined terms across sections. Produce a clause-level changelog or risk matrix at the end.

### Regulatory filings and rulemakings

The pattern: a long agency rulemaking where the question is "what changed compared to the prior version" or "what does this rule mean for our practice." Use Cowork. Process by section. Compare to prior versions if available. Surface the changes that affect specific firm practices.

### Expert reports

The pattern: a long technical or financial expert report where the question is "where are the weak points" or "what's the strongest counter-argument." Use Cowork. Process by methodology section. Identify assumptions, citations, and gaps. Produce a memo orienting the cross-examining lawyer.

## Composing with verification

Long-document analysis composes naturally with the verification workflow (see `references/verification.md`).

Because each section's notes file already contains citations to the original document, the final synthesis inherits those citations. The citation-verifier skill can then round-trip every quoted phrase against the original document and flag mismatches. The verification cost is bounded.

## What Cowork doesn't fix

Cowork's agentic harness solves *attention* problems (context rot). It does not solve *judgment* problems.

If the document requires legal judgment — interpreting a novel clause, applying a doctrine to a new fact pattern — the model will produce a draft, but the lawyer still has to review the judgment calls. The harness makes the routine work tractable; it doesn't replace the judgment.

It also doesn't solve *quality of source* problems. If the document is a degraded scanned PDF and the OCR layer mangled paragraph 14, the model's analysis of paragraph 14 will be wrong regardless of how well-decomposed the skill is. See `references/practice-areas.md` for the OCR pipeline note and `references/anti-patterns.md` for what not to do with scanned material.

## How to use this reference

When the user's task involves a document over 50 pages:

1. Recommend Cowork over chat. Be explicit about why.
2. Recommend the decompose-process-synthesize pattern.
3. If a relevant skill in this pack applies (`version-diff`, etc.), point to it.
4. If the user is writing their own skill, walk through the pattern with them.
5. Surface the synthesis trap. It's the most common failure mode in long-document skills.

## Source

Mark Pike's webinar answer; the lost-in-the-middle phenomenon is documented in the LLM literature; the decompose-process-synthesize pattern is HAQQ's standard architecture for long-document skills in production.
