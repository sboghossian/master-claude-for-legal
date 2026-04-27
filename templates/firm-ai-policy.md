# [Firm Name] AI Use Policy

**Status:** Template — adopt and edit. Run past your own counsel before deploying.

**Effective date:** [date]
**Owner:** [General counsel / managing partner / IT lead — name and role]
**Review cadence:** Annual minimum; ad-hoc on regulatory developments

---

## 1. Purpose

This policy establishes how [Firm Name] uses generative AI tools (including but not limited to Claude, ChatGPT, Gemini, Copilot, and similar) in the practice of law. It covers permissible uses, mandatory configurations, prohibited uses, supervisory expectations, and client communication standards.

The policy is binding on all attorneys, paralegals, staff, and contractors who access firm systems or do firm work using AI tools.

## 2. Scope

This policy applies to:

- All AI tools accessed through firm-issued accounts or licenses
- All AI-assisted work product produced for or on behalf of firm clients
- AI tools used on personal devices when conducting firm work
- AI tools embedded in third-party platforms (e.g., Microsoft 365 Copilot, document management AI features)

It does not apply to:

- Personal use of AI on personal time, unrelated to firm work
- Spell-check, autocomplete, and similar embedded productivity features that do not transmit content outside the firm-controlled environment

## 3. Permissible uses

The following uses are permitted, subject to the configurations in §5 and the verification requirements in §6:

- Document review and analysis (contracts, transcripts, filings, regulatory text)
- Drafting first-pass work product (memos, briefs, correspondence, redlines)
- Legal research, provided cited sources are independently verified
- Client communication drafts (subject to attorney review before sending)
- Internal status communications (newsletters, briefings, summaries)
- Practice management workflows (intake routing, deadline tracking, schedule optimization)
- Cross-functional translation between legal language and other domains

## 4. Prohibited uses

The following uses are prohibited:

- **Use of free or consumer-tier AI accounts** for any work involving client data or confidential firm information. All work must occur on commercial-tier accounts (Team, Enterprise, or equivalent contractual tier with no-training-on-inputs guarantees).
- **Use of personal AI accounts** for firm work, regardless of tier.
- **Connecting personal email, drive, or messaging accounts** to firm-licensed AI tools, or vice versa.
- **Filing or sending AI-generated work product without independent attorney verification** of all citations, quotations, and factual claims.
- **Use of AI to generate content for matters where the firm is conflicted** without explicit conflict-clearance documentation.
- **Sharing AI outputs externally** (with clients, opposing counsel, regulators) without partner or general counsel review for matters above [threshold — e.g., $X in dispute, novel legal questions, regulatory submissions].
- **Use of AI tools for matters subject to client AI-restriction clauses** in the engagement letter without obtaining client consent.

## 5. Required configuration

### 5.1 Tier

All firm AI use must be on a commercial tier with contractual no-training-on-inputs guarantees. The current approved tools and tiers are:

| Tool | Approved tier | Account provisioning |
|------|---------------|----------------------|
| Claude | Team or Enterprise | IT provisions firm-issued accounts |
| [Other approved tools] | [tier] | [process] |

### 5.2 Connectors

Connectors (MCP integrations, plugin connections, third-party API links) must be:

- Authenticated to firm-issued accounts only (never personal)
- Configured with the firm's standard permission grid (read = always allow; send/modify/delete = needs approval)
- Documented in the connector inventory maintained by [role/team]
- Reviewed quarterly by [role/team]

### 5.3 Sandboxing

For agentic work (Cowork, Claude Code, equivalent surfaces), AI sessions must operate within firm-managed directories. AI tools must not be granted unrestricted file system access on personal partitions or volumes containing non-firm data.

### 5.4 Project / matter scoping

Each matter requiring AI use must have a corresponding project or workspace within the AI tool, named per the firm's matter-ID convention. Cross-matter mixing of AI sessions is prohibited absent specific authorization.

### 5.5 Data residency

For matters subject to data-residency requirements (GDPR-affected EU clients, certain regulated industries), AI use must occur in the AI vendor's region matching the requirement. The IT team maintains a list of approved regional configurations.

## 6. Verification requirements

### 6.1 Citation verification

For any work product that will be filed with a court, sent to a regulator, delivered to a client as relied-upon work product, or used in negotiation:

- Every citation (case, statute, regulation, treatise, document) must be independently verified to exist as cited.
- Every quoted phrase must be verified to appear verbatim in the cited source.
- Every "the court held / the statute provides / the regulation requires" assertion must be verified to be supported by the cited source.
- Verification may be aided by AI tools (e.g., a citation-verifier skill) but must include human review of any flagged items.

### 6.2 Internal review

For work product above [threshold — e.g., novel legal question, dispute amount over $X, regulatory submission]:

- An attorney other than the original drafter must review the AI-assisted output before delivery.
- The reviewing attorney is responsible for the accuracy and quality of the output as if they had drafted it.

### 6.3 Documentation

For work product where AI was used materially:

- A note in the matter file documenting that AI was used and which tool/skill
- The AI session log retained for [retention period — e.g., 7 years or per matter retention policy]

## 7. Client communication

### 7.1 Engagement letter

The firm's standard engagement letter shall include a provision disclosing the firm's use of AI tools in the practice of law. Sample language:

> *"The firm may use generative AI tools to assist in document review, research, drafting, and other activities. All AI-assisted work is supervised by attorneys, all citations are independently verified, and AI use does not change our professional responsibility to you. We use commercial-grade AI tools that do not train on client data. If you have concerns about AI use on your matter, please discuss with the responsible partner."*

### 7.2 Client-specific restrictions

If a client requests restrictions on AI use (e.g., no AI on this matter, no specific vendors, no use of certain types of tools), document the restriction in the matter file and configure the team's tooling accordingly. Compliance is the responsibility of the matter partner.

### 7.3 Client questions

When a client asks about firm AI practices, refer to the client-facing explainer at [link to `templates/client-data-explainer.md` adapted for the firm]. The matter partner is responsible for any specific questions.

## 8. Supervisory responsibility

### 8.1 Attorney accountability

Attorneys remain professionally responsible for all AI-assisted work product as if they had produced it without AI assistance. AI use does not transfer or diminish professional responsibility.

### 8.2 Partner review

For matters using AI materially, the responsible partner is accountable for ensuring:

- Configuration compliance (§5)
- Verification compliance (§6)
- Client communication compliance (§7)

### 8.3 Training

All attorneys and staff using AI tools must complete the firm's annual AI training program, covering:

- This policy
- The four-pillar architecture and four-layer privilege framework
- Citation verification workflow
- Anti-patterns and incident response

New hires must complete training before their first AI use on firm matters.

## 9. Incident response

If an AI-related incident occurs (suspected privilege breach, hallucinated citation in filed work product, unauthorized data access, vendor breach notification):

1. **Notify** the IT lead and general counsel immediately
2. **Contain** by revoking implicated accounts, connectors, or access
3. **Assess** scope of exposure and affected clients
4. **Notify** affected clients per the firm's incident response policy and relevant regulatory requirements
5. **Remediate** with technical and procedural updates
6. **Document** the incident and post-mortem for future reference

## 10. Approved tools list

The following AI tools are currently approved for firm use:

| Tool | Use cases | Approved configurations | Restrictions |
|------|-----------|-------------------------|--------------|
| Claude (Anthropic) | All listed in §3 | Team / Enterprise tiers; firm-provisioned accounts; connectors per §5.2 | No use on matters with client AI-restriction clauses absent consent |
| [Add others] | | | |

Tools not on this list may not be used for firm work without approval from [role — typically GC or managing partner].

## 11. Vendor evaluation

When considering a new AI tool for firm use, the evaluation must include:

- Completion of the firm's vendor security questionnaire ([link to `templates/vendor-security-questionnaire.md`])
- SOC 2 Type II review
- Data processing agreement review
- Pilot in a controlled scope with documented evaluation criteria
- Approval by [role]

## 12. Policy updates

This policy is reviewed annually. Material AI developments (new model releases, new regulatory guidance, new case law on AI use, new firm tools) may trigger interim updates.

Updates require approval from [role] and are communicated to all firm personnel via [channel]. The current version of this policy is maintained at [internal location].

## 13. Definitions

- **AI / Generative AI / GenAI** — software that uses machine learning models to generate text, code, images, or other content in response to user input. Includes but is not limited to LLMs.
- **Commercial tier** — an AI vendor's pricing tier that includes contractual guarantees against using customer data to train models.
- **Connector / MCP** — integrations between AI tools and the firm's data systems.
- **Skill / Plugin** — pre-configured AI workflows that automate specific tasks.
- **Project / Workspace** — a scoped AI working area associated with a specific matter or initiative.

## 14. Acknowledgment

All attorneys and staff using AI tools must acknowledge this policy at hire and at each annual update. Acknowledgment is recorded in [HR system / training platform].

---

## Customization notes

This is a starting template. Edit before deploying. Specifically:

- [ ] Replace `[Firm Name]`, role names, and process references with your firm's actual structure
- [ ] Adjust thresholds (matter size, dispute amount, retention period) to match your practice
- [ ] Adjust approved tools and tiers to your actual stack
- [ ] Run past your firm's general counsel before adoption
- [ ] Distribute, train, and require acknowledgment

This template is informed by: ABA Model Rules 1.1, 1.6, 5.1, 5.3, and the rapidly developing case law on AI in legal practice (Heppner ruling and the broader documented set of 2,000+ filings with hallucinated citations as of 2026). It is not, and is not intended as, legal advice. Have your own counsel review before adopting.

MIT licensed. Part of master-claude-for-legal: github.com/sboghossian/master-claude-for-legal
