# Anti-patterns

What not to do, with real examples. The compressed version of every "we've seen this go badly" story.

---

This reference is opinionated. Disagree with any of it freely; the files are markdown and you can edit them. But these are the patterns that produce the worst outcomes in legal-AI deployments, and they are common enough that we wrote them down.

## 1. Doing real work on the consumer tier

**The mistake.** A partner uses Claude.ai on the free tier (or personal Pro account) for actual matter work. The free tier defaults to using inputs to improve the model. Privilege exposure is contractual, not technical.

**The cost.** This is what produced the Heppner ruling — non-attorney, consumer plan, no privacy controls, fabricated citations filed in court. The issue wasn't the model; it was the tier.

**The fix.** Move to Team or Enterprise. Cost is trivial; exposure is enormous. See `references/privilege-layers.md` Layer 1.

## 2. Wiring personal accounts to firm work

**The mistake.** A lawyer authenticates Claude to their personal Gmail or personal Drive because it's faster than waiting for IT to provision firm OAuth. Then they do firm work through that connector.

**The cost.** Audit trail lives in two systems. The firm's compliance lead cannot answer "what did Claude read about Matter X" because half the answer is in a partner's personal Google account. When the personal account is eventually compromised (it will be), firm matter data is in the breach.

**The fix.** Always use firm-issued accounts for firm work. Always. See `references/mcp-hardening.md`.

## 3. Setting all connector actions to "always allow"

**The mistake.** During setup, the lawyer clicks through the connector permission grid and accepts defaults. The defaults frequently include "always allow" on send, modify, and delete actions because the vendor optimizes for demo smoothness.

**The cost.** Any model error or prompt-injection event can cause the connector to send, modify, or delete without human approval. Real incident: a skill misinterpreted a "send the draft" instruction and emailed a sensitive memo to the entire firm distribution list.

**The fix.** Five-minute hardening. Read actions: always allow. Mutating actions: needs approval. See `references/mcp-hardening.md`.

## 4. Using the legal plugin out of the box

**The mistake.** A firm installs Anthropic's legal plugin and uses the skills as-is on real client work.

**The cost.** Mark Pike said it himself: *"You wouldn't wear a suit you bought off the rack."* The starter skills encode Anthropic's general-counsel theory of how a legal workflow should run. Not your firm's. The output is mediocre, the firm concludes "AI doesn't work for legal," and the project shelves.

**The fix.** Customize the skills before using them on real work. The recursive trick (feed Claude examples, let it draft the skill, edit) gets you 90% of the way in 30 minutes. See `references/skill-authoring.md`.

## 5. Skipping citation verification

**The mistake.** The lawyer asks Claude for case law on a question, accepts the response with citations, pastes the citations into a brief.

**The cost.** Two thousand-plus court filings to date have been documented as containing hallucinated citations. The mechanism is almost always: the model produced citations, the lawyer didn't verify, the court read the cited cases and found they didn't say what was claimed.

The Mode 1 failure (cite doesn't exist) is rare with modern grounded retrieval. Mode 2 (cite exists but doesn't say what the model claims) is common.

**The fix.** Round-trip every quoted phrase against the source. Use the `citation-verifier` skill in this pack or build an equivalent. See `references/verification.md`.

## 6. Long-document analysis in chat

**The mistake.** The lawyer pastes a 200-page contract into Claude.ai chat and asks for a comprehensive review.

**The cost.** Context rot. The model produces a summary that misses Schedule 4. Schedule 4 contains the cross-reference that flips the indemnification logic. The lawyer doesn't catch the omission. The deal closes with the bad indemnification structure intact.

**The fix.** Use Cowork with a properly decomposed skill. See `references/long-documents.md`.

## 7. The synthesis trap

**The mistake.** A long-document skill processes 15 sections of a contract independently and stitches the outputs together without a synthesis step. Section 3 uses one definition of "material adverse change"; section 9 uses another; the final output contradicts itself.

**The cost.** The lawyer has to read the whole thing to catch the inconsistency, which defeats the purpose of the skill. Or worse, they don't catch it and the inconsistency makes it into client-facing work.

**The fix.** Add an explicit synthesis stage to long-document skills. The stage reads all section outputs, reconciles, produces one coherent document with citations. See `references/long-documents.md`.

## 8. Mixing matters in a single project

**The mistake.** A lawyer uses one Cowork project for all their work because it's easier than creating a new project per matter. The project ends up containing files, conversations, and connector traffic from a dozen different clients.

**The cost.** Layer 4 privilege gap. If a regulator or court asks "show me everything Claude saw about Matter X," the answer requires extracting the relevant subset from a polluted project. If a conflicts issue arises mid-engagement, you may not be able to cleanly remove the conflicted data.

**The fix.** One project per matter. Discipline pays off the first time you have to demonstrate scope to a regulator. See `references/privilege-layers.md` Layer 4.

## 9. The "I'll customize it later" plan

**The mistake.** The firm installs the legal plugin, sets up connectors, runs a few demos, declares victory, and never gets around to writing or remixing skills for the firm's actual practice.

**The cost.** Six months later, the AI investment has produced no measurable productivity gain. The firm concludes "AI doesn't work for us." Pre-empts the actual investment they would have needed to make.

**The fix.** The four-hour walkthrough in `references/setup-checklist.md`. Build at least one customized skill that runs on real work in the first week. The first skill is the wedge.

## 10. Auto-sending without human review

**The mistake.** A skill is configured to draft and send emails on the lawyer's behalf, with no human-in-the-loop step. The model misinterprets a thread and sends a draft that looks legitimate but contains a damaging miscommunication.

**The cost.** The firm has now sent the wrong thing to a client. The model wasn't malicious; it just got it wrong. The recipient doesn't know it was AI-drafted. The firm has to explain.

**The fix.** Never auto-send. Producing email and Slack drafts is fine; sending is the user's call. The two-second approval click is cheap insurance.

## 11. Using AI on degraded scanned documents without OCR

**The mistake.** The lawyer has a 50-page scanned PDF of an old sale deed (faded photocopy, smudged dates, marginal handwriting). They drop it into Claude and ask for analysis.

**The cost.** The vision model attempts OCR on the fly with significant errors. Dates get misread. Critical letters get swapped. The lawyer relies on the analysis, and the analysis is wrong.

**The fix.** Run a dedicated OCR pipeline upstream (Adobe Acrobat, AWS Textract, Tesseract for budget). Feed cleaned text to Claude. For property and IP work where this is the bulk of the evidence, this is non-negotiable. See `references/practice-areas.md`.

## 12. Treating Claude's training data as case law

**The mistake.** A lawyer asks Claude for case law on a question without grounding the request in a real legal-research connector (Westlaw, Lexis). The model recalls cases from training data, which may be outdated, fabricated, or wrong for the relevant jurisdiction.

**The cost.** This is the original hallucination failure mode. The cases either don't exist or don't apply.

**The fix.** Use grounded retrieval for legal research. If you don't have a Westlaw or Lexis connector, qualify every recall: *"Find cases on this question in the Westlaw connector. If you cannot access Westlaw, say so explicitly rather than recalling from memory."*

## 13. The "AI replaced my associate" announcement

**The mistake.** A partner declares publicly that AI has replaced an associate's role. Headcount-cutting framing. Junior lawyers in the firm conclude the firm is hostile to their development.

**The cost.** Talent flight. The lawyers leaving are the ones with options — usually the strongest. The firm ends up with worse human talent and the same AI tooling, which is not the upgrade it intended.

**The fix.** Frame AI as augmentation, not replacement. The work that was previously the associate's bulk effort (reading every page, marking every relevant excerpt, drafting first passes) is now AI's bulk effort. The associate moves to higher-judgment work earlier in their career. This framing is also accurate. Use it.

## 14. Skipping the policy

**The mistake.** The firm rolls out Claude without a written AI use policy. Lawyers improvise. Admins cannot answer client questions about how the firm uses AI.

**The cost.** Inconsistent practice across the firm. When a client asks "how does your firm handle our data when using AI," the answer is different depending on which lawyer they ask, and some of those answers are wrong. Eventually a client gets a wrong answer, escalates, and the firm has a problem.

**The fix.** Adopt a written AI policy. The starter template is at `templates/firm-ai-policy.md`. Edit it; have your counsel review it; circulate it; train against it.

## 15. Mistaking demos for production

**The mistake.** The firm sees an impressive demo of Claude doing contract review in 20 minutes. They commit budget based on the demo. They deploy. Real-world performance is 50% of demo.

**The cost.** Disappointment. Pilot ends without renewal. The firm concludes "AI is overpromised."

**The fix.** Demos run on curated documents with pre-tested skills. Real-world performance is bounded by the worst document in your corpus and the time you've invested in skill customization. Set expectations accordingly. The Mark Pike "20 minutes" number is real *for his firm with their skills*. Your firm with the off-the-shelf skills should expect 60 minutes for the same workflow until you've customized.

## 16. Building a custom platform when you don't have to

**The mistake.** A small or mid-size firm decides to build their own legal-AI platform on top of the Anthropic API instead of using Claude with skills + connectors.

**The cost.** Six months of engineering effort. A platform that does 60% of what Cowork does out of the box. Ongoing maintenance burden the firm can't afford.

**The fix.** Use Cowork. Customize through skills. Build only the parts that don't exist (specific connectors to your bespoke systems). The buy-vs-build calculus tilts strongly toward buy for any firm that doesn't have an engineering team. See `references/practice-areas.md` for when bespoke makes sense (rare; mostly trial-tech for high-stakes litigation).

## 17. Single-vendor lock-in mindset

**The mistake.** The firm picks Claude and refuses to evaluate competing tools. Or picks a competitor and refuses to evaluate Claude. Or picks a legal-specific platform and refuses to use raw Claude even when it's the better tool for a specific job.

**The cost.** The firm misses category leaders. AI tooling is moving fast; the leader in 2026 may not be the leader in 2027. A firm with portable skills (markdown files, MCP-based connectors) can move; a firm with deep tool-specific lock-in cannot.

**The fix.** Keep your skills in markdown. Keep your connectors MCP-based. Treat the tooling as composable. The skill you write today should be portable to whatever tool wins next year.

---

## How to use this reference

When the user is about to make one of these mistakes:

1. Don't lecture. Surface the specific anti-pattern with the cost.
2. Offer the fix concretely.
3. If the fix involves a different reference doc, point to it.
4. Don't be preachy. The user has agency. Your job is to surface the risk; theirs is to decide.

When the user describes a problem they've already encountered, identify which anti-pattern produced it. Most legal-AI failures map onto one of these.

## Source

Patterns observed across HAQQ deployments to ~9,800 firms, the Heppner ruling, public reporting on hallucinated court filings, and the *Claude for Legal Teams* webinar's editorial subtext.
