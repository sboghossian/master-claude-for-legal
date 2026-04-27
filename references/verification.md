# Verification

How to keep your firm off the naughty list.

Over 2,000 court filings have been documented to date as containing hallucinated citations. This is not a hypothetical problem. It is a regular occurrence at firms that have not built a verification workflow into their AI use, and it ends careers.

This reference is the verification workflow.

---

## The two failure modes

There are two distinct ways AI legal output goes wrong, and they require different mitigations. Most firms only build defenses against the first one.

**Failure mode 1: cited source does not exist.** The model invents a Westlaw cite, a statute number, a case name. This is the failure mode everyone talks about and the one that's mostly addressed by grounding the model in real case law via tools and connectors.

**Failure mode 2: cited source exists but doesn't say what the model claims.** The model produces a real cite, then summarizes it incorrectly. The associate pastes the cite into the brief without verifying. The court reads the cited case, finds the holding doesn't support the proposition, and the firm gets sanctioned.

Mode 2 is harder to catch because the citation looks legitimate. It's also more common at firms that have moved past the consumer-tier panic and now use enterprise AI with grounded retrieval — they've solved Mode 1 and assume verification is done.

It is not.

## The verification workflow

Apply this to every legal output that contains citations or quoted material:

### Step 1 — Source level

For every claim the model makes, identify the specific source it came from. Not "the contract"; not "the case file"; not "Westlaw." A specific paragraph, a specific page, a specific document.

If the model cannot point to a source, mark the claim as **model-only**. Model-only claims are useful as drafting starting points; they are not citable.

### Step 2 — Cite verification (Mode 1)

For each cited source, verify that the source exists. Click the link. Open the case. Check the case reporter. If the model says *Smith v. Jones, 123 F.3d 456 (9th Cir. 2018)*, search Westlaw or Lexis or the relevant reporter for *Smith v. Jones, 123 F.3d 456*. If you can't find it, the cite is fabricated. Strike it.

Modern Claude with proper retrieval rarely fabricates cites in this Mode-1 sense. But "rarely" is not "never," and your workflow should not depend on the model being right about whether a case exists.

### Step 3 — Quote verification (Mode 2)

This is the step everyone skips. Don't.

For every passage the model quotes from a source, the quoted text must appear *verbatim* in the source. Open the source. Use Cmd-F or Ctrl-F. Find the exact phrase. If the source uses "the parties shall" and the model quoted "the parties must," that is a paraphrase, not a quote, and your brief should not use quotation marks around it.

For non-quoted assertions about a source, verify the source actually supports the assertion. If the model says "the court held that X," read the holding section of the case. If X isn't actually in the holding (or worse, the holding contradicts X), the assertion is wrong.

### Step 4 — Reasoning verification

The model can produce a chain of reasoning that is internally coherent but breaks down at one specific link. Check the links.

A common failure: the model applies a rule from one jurisdiction to a fact pattern in another jurisdiction, where the rule doesn't apply. Read the model's reasoning step by step. Each step should follow from the previous one *and* be supported by the relevant authority for *the relevant jurisdiction*.

### Step 5 — Final review

Read the output as if you were the partner who would sign it. Would you sign this? If not, identify what's missing and either fix it or send it back to the model with the specific gap called out.

---

## The leverage move: round-trip the quotes automatically

Manual verification at every step is expensive. The leverage move is to automate Step 3.

Build (or use) a `citation-verifier` skill that:

1. Reads the model's output
2. Extracts every quoted phrase
3. Searches the cited source for the verbatim phrase
4. Flags any quote where the source doesn't contain the phrase verbatim
5. Surfaces the flags to the human reviewer

Now the human's job is not "verify everything." It is "verify the flags." This collapses the verification cost from "read the entire output and then read the entire source" to "read the output and check the items the system itself doesn't trust."

The starter `citation-verifier` skill in this pack does this. See `skills/citation-verifier.md`.

## The leverage move: paragraph-level annotations

Configure your skills to annotate every claim with the specific paragraph or page it comes from. The annotation should be in the output, not buried in metadata.

Format the model produces:

> Indemnification is capped at the contract price. (MSA §8.2, p. 14)

Format the model should not produce:

> Indemnification is capped at the contract price. (See contract.)

The first version is a verification handle. The second version is a gesture at one.

In your skill prompts, write:

> Cite the specific section, paragraph, and page (where applicable) for every assertion that comes from a provided source. Use the format: (Source §X.Y, p. Z). Do not use vague references like "the contract" or "the case file."

This phrasing nudges the model into citation-shaped output and makes Step 1 of the verification workflow nearly free.

## The skill-prompt boilerplate

Every legal skill in this pack — and every skill you write — should include verification instructions in the prompt. The boilerplate:

```
Output requirements:
- Cite the specific section, paragraph, or page for every factual assertion grounded in a source. Format: (Source §X.Y, p. Z).
- Distinguish source-grounded claims from model-only inferences. Mark model-only claims with the prefix [model inference].
- Quote source language verbatim. If you paraphrase, do not use quotation marks.
- For legal propositions, cite the controlling authority for the relevant jurisdiction.
- If you cannot ground a claim in a provided source, say so explicitly rather than fabricating a citation.
```

Paste this into the bottom of any legal skill you write. It does most of the verification heavy lifting upstream of the human review.

## When verification is non-negotiable

Some outputs require full human verification at every step regardless of automation:

- **Court filings.** Every cite, every quote, every assertion. No shortcuts.
- **Regulatory submissions.** Same.
- **Legal opinions to clients.** Same.
- **Client-facing memos that will be relied on.** Same.

For these, the AI output is a draft. The verification is the lawyering. Don't compress it.

## When lighter verification is acceptable

Some outputs can use a lighter touch:

- **Internal status updates.** Verify high-stakes claims; sample-check others.
- **First-pass document review for relevance.** Verify the model's calls on a sample (10-20%); accept the rest if the sample looks good.
- **Drafts that will go through a senior lawyer's full review anyway.** The senior review is the verification.

The principle: match verification rigor to downstream consequence.

## What model and surface to use

For verification-heavy work:

- **Use Sonnet at minimum.** Haiku is too prone to surface-level errors for legal work where the cite has to be right.
- **Use Opus for novel reasoning.** When the work involves applying a doctrine to a new fact pattern, Opus is worth the cost.
- **Use Cowork over chat.** The agentic harness lets the model decompose long-document analysis into bounded steps, which reduces context-rot-driven errors.
- **Use grounded retrieval.** If you have access to Westlaw, Lexis, or another legal-research connector, use it. Don't ask the model to recall case law from training data.

## The Heppner case

For the curious: the Heppner ruling Mark mentioned in the webinar involved a non-attorney user on a consumer-grade plan whose AI produced fabricated citations and the user filed them. The lessons compound:

- Wrong tier (consumer plan, no enterprise privacy)
- Wrong user (non-attorney working at their own direction)
- Wrong workflow (no citation verification before filing)
- Wrong tooling (consumer Claude.ai didn't surface citations as first-class artifacts)

Each layer compounded. A firm that has its tier, its user, its workflow, and its tooling correct does not have a Heppner problem.

## How to use this reference

When the user produces or asks about a legal output that will be relied on:

1. Confirm the verification workflow is in their loop. Don't assume.
2. Surface the citation-verifier skill if the output has citations.
3. Reach for the skill-prompt boilerplate if the user is writing or remixing a skill.
4. Match rigor to consequence. Court filings need the full workflow; internal drafts need lighter verification.
5. Be honest if the model is operating in a regime where verification is hard. If the user is in chat-only Claude on a 200-page document, citation accuracy will degrade. Suggest the move to Cowork.

## Source

The 2,000+ filings number is from Mark Pike's webinar reference. Verification methodology is HAQQ's editorial expansion based on observed deployments and incident response.
