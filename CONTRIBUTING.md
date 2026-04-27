# Contributing to master-claude-for-legal

Thanks for considering a contribution. This pack only gets better with input from people doing legal work in different practice areas, jurisdictions, and firm sizes than the original maintainers.

## What we want

Roughly in priority order:

1. **New starter skills** for practice areas the pack doesn't cover. Immigration, family law, criminal defense, tax, employment, real estate, securities — any of these could use a starter skill written by someone who actually practices there.

2. **Translations of references** into other languages. Especially Spanish, Arabic, French, Portuguese, German, Mandarin, Japanese — given how globally the legal-AI category is being adopted. The vocabulary doc and setup checklist are the highest-value to translate first.

3. **Connector-specific guides** for legal-industry systems. iManage, NetDocuments, ProLaw, Clio, MyCase, PracticePanther, Worldox, Smokeball — anything where you've gotten Claude wired up and have battle-tested instructions. Drop these in `references/connectors/<system>.md`.

4. **Real-world skill remixes** — anonymized versions of skills you've customized for your firm. Even a single section showing "here's how we adapted nda-triage for our M&A practice" is useful.

5. **Updated dataset** when Anthropic publishes additional Q&A or runs follow-up legal webinars. We'll maintain `data/` going forward and welcome PRs with new sources.

6. **Anti-patterns from the wild.** If you've seen a legal-AI deployment fail in a way that's not in `references/anti-patterns.md`, write it up. We'll add it with attribution if you want or anonymously if you don't.

## What we don't want

- Vendor pitches. The pack is vendor-neutral on the legal-AI category. If your contribution exists primarily to drive adoption of a specific tool, it won't merge. (Including HAQQ — the maintainer's own product. Same standard applies.)
- Speculative "AI will replace lawyers" content. The pack is operational. Adoption is the assumed audience; abstract debate isn't.
- Content that copies someone else's work without attribution. If you're translating, citing, or building on another resource, credit it.
- Skills that don't include the verification boilerplate from `references/verification.md`. Citation discipline is non-negotiable in this pack.

## Format and style

### File format

- Markdown for all content
- YAML frontmatter on skills (with `name`, `description`, optional `version`)
- One file per skill or reference
- File names in kebab-case

### Voice and length

- Direct. Real specifics over abstractions.
- Honest about limits. If a skill or pattern doesn't always work, say so.
- Length-flexible. Reference docs run 800-3,000 words. Skills run 200-1,500 words. Templates run 1,000-3,000 words. We don't optimize for short or long; we optimize for "the right amount for the topic."
- No emojis unless used in the existing files. The pack is read by lawyers; the tone should be professional but not stiff.

### Citation discipline

If you reference a public source (a webinar, a court decision, a regulation, an academic paper), cite it specifically. Don't paraphrase facts you can't ground.

If you make claims based on your own deployment experience, label them as such ("in our deployments at [firm/context], we've observed...").

### Skill structure

All starter skills must include:

1. YAML frontmatter (name, description, version)
2. Role and goal paragraph
3. Inputs section
4. Procedure (numbered steps)
5. Output format (specific)
6. The verification boilerplate from `references/verification.md`
7. When to escalate / pause section
8. Composition with other skills (if applicable)
9. Connector requirements
10. Customization checklist
11. Provenance section

See existing skills (`skills/nda-triage.md`, etc.) for the pattern.

## Process

### For small changes (typo, factual correction, link update)

Open a PR directly. Include a one-line description.

### For new skills, references, or templates

1. Open an issue first to discuss the proposed addition. We may have feedback or know of similar work in flight.
2. Fork the repo. Branch off main.
3. Write your contribution following the format above.
4. Submit a PR. Reference the issue. Describe the use case and how you've tested.

### For translations

1. Open an issue describing which file you intend to translate and to which language.
2. Fork. Create the file at `references/<lang>/<filename>.md` (e.g., `references/es/four-pillars.md`).
3. Translate the content. Adapt jurisdictional references where appropriate (e.g., when translating to Spanish for a Latin-American audience, the example regulatory bodies should be regional).
4. Submit a PR.

### For dataset updates

1. Open an issue with the new source (link to the webinar, document, or other).
2. Fork. Update `data/` with the new transcript and structured questions/themes.
3. If a structural change is needed (new fields, new themes), discuss in the issue first.

## Review

PRs are reviewed for:

- **Accuracy** — claims should be grounded in source material or labeled as opinion
- **Format** — does it follow the structure of existing files?
- **Voice** — direct, honest, vendor-neutral
- **Verification discipline** — skills must include the boilerplate
- **Compatibility** — does it work in Cowork? Claude Code? Other Anthropic-format tools?

Most PRs get a response within 7 days. Active PRs that need iteration typically merge within 2-3 weeks. If a PR is sitting too long, ping the issue.

## License

All contributions are MIT-licensed by submission. By opening a PR, you confirm you have the right to submit the content under the MIT license, and you grant the project that license.

If your contribution incorporates other people's work (translations of public material, adaptations of widely-published patterns), you are responsible for ensuring the underlying material is compatible with MIT licensing or in the public domain.

## Code of conduct

Be civil. Disagree on substance, not personalities. Assume good faith from contributors who may be operating under different professional contexts than yours.

Legal-AI is a high-stakes category. The work we publish here may be used on real client matters. That doesn't mean the discussion has to be solemn, but it does mean we hold ourselves to a standard of accuracy and honesty.

If something in the pack is wrong, file an issue. We'd rather hear it.

## Questions

Open an issue or reach out via the contact info on the project README.

---

Thanks for contributing.

— The maintainers
