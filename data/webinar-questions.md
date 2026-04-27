# Webinar Questions Dataset

The 51 questions submitted by the audience during the *Claude for Legal Teams* webinar (Anthropic, April 2026), with their upvote counts, themes, and whether they were answered live.

This is a public dataset. Use it for research, journalism, product planning, or to map your own legal-AI roadmap against what 20,000 lawyers actually wanted to know.

The structured version is in [`webinar-questions.json`](webinar-questions.json). The full transcript is in [`webinar-transcript.txt`](webinar-transcript.txt).

---

## Headline numbers

- **Registered attendees**: 20,000+
- **Questions submitted**: 51
- **Total upvotes**: 2,470
- **Questions answered live**: 25 (49%)
- **Questions partially answered**: 11 (22%)
- **Questions not addressed**: 13 (25%)
- **Reactions / off-topic**: 2 (4%)

## Theme distribution

| Theme | Questions | Total upvotes | Share |
|-------|-----------|---------------|-------|
| Security & Privilege | 4 | 1,050 | 42% |
| Skills & Plugins | 13 | 511 | 21% |
| Integrations | 3 | 269 | 11% |
| Product Surfaces & Models | 7 | 213 | 9% |
| Adoption & Use Cases | 7 | 191 | 8% |
| Document Handling | 6 | 175 | 7% |
| Logistics | 5 | 165 | 7% |
| Accuracy & Verification | 4 | 161 | 7% |

Three signals worth holding from the rank-order:

1. **Security at 42%** dominates not because lawyers are paranoid, but because they are operationally serious. The audience already plans to use Claude; they're asking how to build the policy around it.
2. **Hallucinations at 7%** falling to fourth place is a maturity signal. The profession has internalized that the problem is solvable through workflow design and is now asking next-order questions about how to do that systematically.
3. **Skills/Plugins took 13 questions** because the vocabulary is genuinely confusing. Mark Pike's "don't use it out of the box" admission is the most honest framing of where the technology actually is.

A fourth signal worth a beat: **nobody asked whether AI would replace lawyers**. Not one of the 51 questions. That debate is over inside the profession.

---

## All 51 questions, ranked by upvotes

Legend:
- ✅ Answered live
- 🟡 Partially answered
- ❌ Not addressed in webinar (covered in this skill pack)
- ⚪ Reaction / off-topic

| # | Author | Question | Theme | Votes | Status |
|---|--------|----------|-------|-------|--------|
| 1 | Jewel Seo | How is your team dealing with attorney-client privilege when using Claude? | Security | 372 | ✅ |
| 2 | Carrie Schultz | Will you be addressing security and best practices for security with Claude? | Security | 328 | ✅ |
| 3 | Rodney Younce | How do you keep all information — especially sensitive case files — confidential and secure? How do we explain to clients that their data is safely used? | Security | 200 | 🟡 |
| 4 | Julie Saliba | What security considerations should be taken into account when linking to inbox or using via MCP? | Security | 150 | 🟡 |
| 5 | Varinder Rehal | I didn't know there was a Cowork legal plugin — where can I find it and how do I implement it? | Skills | 137 | ✅ |
| 6 | Sipoura Barzideh | Will you be emailing out the links listed in the Docs section? | Logistics | 132 | ✅ |
| 7 | Anonymous | Legal work can involve 70+ page documents. With detailed context and tool calls, there's increased risk of context rot and degraded output. How does Claude's legal team address this? | Documents | 101 | ✅ |
| 8 | Michael Graham | Many firms don't use Gmail or even Outlook — work is stored in a document management or case management system. How can Claude read these files? Does it need a specific integration? | Integrations | 84 | 🟡 |
| 9 | Todd Taylor | I'm interested in building agents in Claude (we have a Team account) for legal work. How would that be different than a skill? | Skills | 79 | ❌ |
| 10 | Nikolaj Nielsen | Hallucinations are a significant concern in legal document review — often requiring full verification. How does Anthropic differentiate its models on mitigating hallucinations and ensuring reliability? | Verification | 70 | ✅ |
| 11 | Tian Luo | When should you use Haiku vs Sonnet vs Opus? | Product | 68 | ❌ |
| 12 | Tian Luo | Is there a reason to use Cowork over Claude Code? | Product | 68 | ✅ |
| 13 | Aviv Geron | What's your recommended methodology for verifying Claude's legal outputs before they go to senior reviewers or regulators? | Verification | 60 | ✅ |
| 14 | Aishwarya Belle | As a transactional lawyer, I struggle with tracking which version of the document reflects changes made by multiple parties. Can Claude give a comparison for that? | Documents | 57 | 🟡 |
| 15 | Rebecca Wright | It would be helpful to have a glossary of terms — e.g. what is a 'skill' vs a 'plugin' and how are they different? | Skills | 50 | ❌ |
| 16 | Brint Hiatt | Would appreciate steps on how to get started from the very beginning. I've never used Claude, only ChatGPT. | Adoption | 49 | 🟡 |
| 17 | Shivangi Agarwal | File types used so far are docx, md, csv. Does Claude have the same accuracy with PDFs of old sale deeds and illegible documents? What about JPEGs? | Documents | 44 | ❌ |
| 18 | Frances Winkler | Who builds these 'skills' — the team, or are they standardized generic skills? | Skills | 43 | ✅ |
| 19 | Anonymous | Can you explain how to use skills for more reliable output in legal work? | Skills | 40 | 🟡 |
| 20 | Andrew Amoranto | How do you roll out live artifacts (like dashboards) and other apps you build for yourself to the rest of the legal team? | Adoption | 39 | 🟡 |
| 21 | Gianni Carfi Pavia | How do you see the Claude Microsoft Word add-in combined with tailored skills changing how legal professionals work with contracts and other legal docs? | Integrations | 35 | ✅ |
| 22 | Tyler Niederwerder | Can Claude automatically decide which skills to use? | Skills | 33 | ✅ |
| 23 | Syed Ali Khan | Is information provided to / reviewed by Claude 'sandboxed' — i.e. not used to train or improve the product? | Security | 29 | ❌ |
| 24 | Anonymous | What's the time commitment and lift to get all of these (add-ins, plugins, Cowork, etc.) set up to operate at the level legal teams require? | Adoption | 26 | ❌ |
| 25 | Sofia Rodriguez | For contract review, what's the best way to structure a project or prompt to systematically review SOWs against a legal playbook? | Adoption | 24 | ❌ |
| 26 | Gareth Last | What skill, plugin, or schedule has Mark or his team created that has had a big impact? | Skills | 22 | ✅ |
| 27 | Merve Yilmaz | Any legal research tips? I'm currently replacing external counsel advice on product expansion. What guardrails should I build? | Adoption | 22 | ❌ |
| 28 | Valter Pasanen | Can you verify sources and paragraphs used in texts in detail when using Claude? | Verification | 21 | 🟡 |
| 29 | David Andrews | We're a small firm on a Team-level plan. Are the same features available to us as Enterprise? | Product | 20 | 🟡 |
| 30 | Christian Fleischmann | Is the Word plugin leveraging the playbooks created in Cowork? | Integrations | — | ✅ |
| 31 | Matthew Fitterer | Can skills be specific to a particular user, or are they always available / applicable organization-wide? | Skills | 16 | ✅ |
| 32 | Scott Ludwig | Will the docs be available after the presentation? | Logistics | 14 | ✅ |
| 33 | Vernicka Shaw | How do you do the redline? | Documents | 13 | ✅ |
| 34 | Alexis Hartwell-Gobeske | Can Claude be set up to provide automatic updates about filing deadlines, hearing dates, etc.? | Adoption | 13 | ❌ |
| 35 | Jack Kegelmeyer | Are the skills prepackaged? | Skills | 12 | ✅ |
| 36 | Ben Kühnel | What are the plans for the legal plugin in the future? Right now it 'only' consists of some Claude skills. | Skills | 12 | 🟡 |
| 37 | Robert Graham | How do you suggest feeding Claude skills that incorporate our voice, tone, and position? | Skills | 11 | ✅ |
| 38 | Ryan Malek | If you already have Claude Code configured with MCPs, Skills, Plugins, CLAUDE.md etc., is it better to use it for legal work or start over with Cowork? | Product | 11 | ❌ |
| 39 | Maddie Rana | Did you say 20K? | Logistics | 11 | ✅ |
| 40 | Priyanka Mehta | Do you rely completely on Claude's output, or do you need to recheck documents? | Verification | 10 | ✅ |
| 41 | Tom Harriman | What enterprise-level controls are in place for skills? Can they be provisioned to specific user groups? | Security | 9 | 🟡 |
| 42 | Orietta Blanco | Can Claude compare large volumes of documents that come in different formats? | Documents | 6 | ❌ |
| 43 | Romina Villarroel | Are there other plugins you recommend for a small law firm? | Product | 6 | ❌ |
| 44 | RUPESH SINGH | Excited for the session. | Logistics | 6 | ⚪ |
| 45 | Angela Bostick | Is Cowork part of Claude? | Product | 5 | ✅ |
| 46 | Montserrat Mazo | What mindset or process changes did they make to better organize information? | Adoption | 4 | ✅ |
| 47 | Aakash Sharma | Can I use legal skills with chat also? | Skills | 3 | ✅ |
| 48 | Andra Robinson | Is Cowork currently in beta? (Answered: Cowork is now GA.) | Product | 3 | ✅ |
| 49 | Vernicka Shaw | That's amazing. | Logistics | 2 | ⚪ |
| 50 | Melissa Lee | How do we get Claude to redline documents? | Documents | 2 | ✅ |
| 51 | Mary Prager | How do you shift from doing work individually in your own Claude instance to collaborating within the legal team and cross-functionally? | Adoption | — | ❌ |

---

## Where unanswered questions are answered

The 13 questions Anthropic ran out of time for are addressed in the references and skills of this pack.

| Unanswered question | Where it's covered in this pack |
|---------------------|--------------------------------|
| Todd Taylor — Agents vs skills | `references/vocabulary.md` |
| Tian Luo — Haiku/Sonnet/Opus | `references/verification.md` (model selection note) |
| Rebecca Wright — Glossary | `references/vocabulary.md` (the entire reference) |
| Shivangi Agarwal — PDFs/JPEGs/old deeds | `references/practice-areas.md` (OCR pipeline note), `references/anti-patterns.md` |
| Syed Ali Khan — Sandboxing/training | `references/privilege-layers.md` Layer 1 |
| Anonymous — Setup time/lift | `references/setup-checklist.md` (full 4-hour walkthrough) |
| Sofia Rodriguez — SOW playbook | `references/skill-authoring.md` (skill structure pattern) |
| Merve Yilmaz — Replacing external counsel | `references/verification.md` (research guardrails) |
| Alexis Hartwell-Gobeske — Filing deadline auto-updates | `references/practice-areas.md` (regulatory/litigation patterns) |
| Ryan Malek — Migrating from Code to Cowork | `references/vocabulary.md` (Cowork vs Code section) |
| Orietta Blanco — Mixed-format batch comparison | `references/long-documents.md`, `skills/version-diff.md` |
| Romina Villarroel — Plugins for small firms | `references/practice-areas.md` (small-firm path) |
| Mary Prager — Team collaboration in Claude | `references/vocabulary.md` (Projects), `skills/status-synthesis.md` |

---

## License

This dataset is licensed under MIT, same as the rest of this repository. Use freely. Attribution to the source webinar (Anthropic) and to this skill pack (HAQQ Legal AI) appreciated but not required.
