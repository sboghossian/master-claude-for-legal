# Vendor Security Questionnaire — Legal AI Tools

A starter questionnaire for evaluating any AI vendor your firm considers adopting for legal work. Send it to the vendor; require written responses; review with IT, GC, and the responsible partner before approval.

This questionnaire covers the four privilege layers (`references/privilege-layers.md`). Skip the layers at your own risk.

---

## Vendor information

1. Vendor legal entity name
2. Headquarters location and entity type
3. Year founded
4. Funding stage / public markets / ownership
5. Regulatory registrations (if any) relevant to the firm's jurisdictions

## 1. Contract layer — training and data use

| # | Question | Required answer |
|---|----------|-----------------|
| 1.1 | Does your standard agreement include a contractual prohibition on using customer inputs and outputs to train, retrain, or improve your models? | YES required |
| 1.2 | If yes, where in the agreement is this provision located? | Specific section reference |
| 1.3 | Are there any tiers or modes (free trial, beta, etc.) where this provision does not apply? | NO required, or full disclosure |
| 1.4 | Do you ever share customer data with third-party model providers, sub-processors, or partners? | Full list of sub-processors |
| 1.5 | What is your data retention default for prompts and responses? | Specific period; ZDR option preferred |
| 1.6 | Can you offer zero-data-retention (ZDR) configurations? | YES required for sensitive matters |
| 1.7 | What are your data deletion guarantees and timelines on contract termination? | Specific commitment |

## 2. Infrastructure layer — security and compliance

| # | Question | Required answer |
|---|----------|-----------------|
| 2.1 | Do you have a current SOC 2 Type II report? | YES required; provide under NDA |
| 2.2 | Do you have ISO 27001, HITRUST, or other relevant certifications? | List all current certifications |
| 2.3 | Where are customer data and AI processing geographically located? | Specific regions; multi-region options noted |
| 2.4 | Can customer data and AI processing be restricted to a specific region (US-only, EU-only)? | YES required for regulated industries |
| 2.5 | Encryption in transit standards | TLS 1.2+ required |
| 2.6 | Encryption at rest standards | AES-256 required |
| 2.7 | Authentication mechanisms supported (SSO, SAML, MFA) | SSO + MFA required for enterprise |
| 2.8 | Do you support BAA execution for HIPAA-regulated matters? | YES if firm handles PHI |
| 2.9 | What is your incident response process and customer notification timeline? | Specific commitment within regulatory windows |
| 2.10 | Have you experienced any data breaches or security incidents in the past 24 months? | Full disclosure required |
| 2.11 | What is your business continuity / disaster recovery posture? | Specific RTO/RPO commitments |

## 3. Deployment layer — admin controls

| # | Question | Required answer |
|---|----------|-----------------|
| 3.1 | What admin controls are available for managing user access? | Granular role-based access control (RBAC) preferred |
| 3.2 | Can the firm scope features and connectors to specific user groups? | YES required for multi-practice-group firms |
| 3.3 | What audit logging is available (who did what, when)? | Comprehensive logs with retention period |
| 3.4 | Can audit logs be exported programmatically (API, SIEM integration)? | YES preferred |
| 3.5 | What controls are available for connector / integration permissions? | Granular per-action permission grid required |
| 3.6 | Can the firm enforce default permission settings (e.g., "send actions always require approval")? | YES required |
| 3.7 | What data loss prevention (DLP) capabilities are available? | Specific features listed |
| 3.8 | Does the platform support eDiscovery export? | YES required for litigation-relevant firms |
| 3.9 | How are connector authentications managed and revoked? | OAuth-style with admin override |
| 3.10 | What is the process for offboarding users (immediate revocation)? | Real-time revocation supported |

## 4. Matter layer — privilege scoping

| # | Question | Required answer |
|---|----------|-----------------|
| 4.1 | Does the platform support per-matter or per-project workspaces with isolated data? | YES required |
| 4.2 | Can the platform enforce information barriers (Chinese walls) between practice groups or matters? | Specific feature description |
| 4.3 | Does each AI session log include matter-level attribution that can be queried? | YES required |
| 4.4 | Can data from a closed matter be archived or deleted on demand? | Per-matter deletion required |
| 4.5 | Can the platform integrate with the firm's matter management system to enforce conflicts and access? | YES preferred; specific integrations listed |
| 4.6 | If a conflict arises mid-engagement, can the firm cleanly remove conflicted data from active sessions? | Specific process |
| 4.7 | Does the platform support privilege-aware access (i.e., honoring privilege scope rather than just role)? | Honest answer; this is rare in general-purpose tools |

## 5. AI accuracy and verification

| # | Question | Required answer |
|---|----------|-----------------|
| 5.1 | Does the platform support grounded retrieval against legal-research databases (Westlaw, Lexis, etc.)? | YES preferred |
| 5.2 | When the model cites a source, does it surface a verifiable reference (link, location)? | YES required |
| 5.3 | Are there built-in features for citation verification or quote round-tripping? | Specific features described |
| 5.4 | What happens when the model cannot find a source for a claim — does it acknowledge or fabricate? | Behavior described; should be honest acknowledgment |
| 5.5 | What model(s) underpin the service, and how often are they updated? | Specific models and update cadence |
| 5.6 | How does the vendor measure and improve accuracy on legal tasks specifically? | Specific evaluation methodology |
| 5.7 | What are the known failure modes of the service on legal work, and how are they mitigated? | Honest disclosure |

## 6. Operational and commercial

| # | Question | Required answer |
|---|----------|-----------------|
| 6.1 | What is the standard deployment timeline from contract signature to production? | Specific timeline |
| 6.2 | What support tiers are available, and what are the SLAs? | Specific tiers and SLAs |
| 6.3 | What is the pricing model (per-user, per-seat, usage-based, enterprise license)? | Full pricing transparency |
| 6.4 | What training and onboarding does the vendor provide? | Specific offerings |
| 6.5 | Are there reference customers in the legal industry we can speak with? | Contact info or case studies |
| 6.6 | What is the vendor's roadmap for legal-specific features over the next 12 months? | Forward-looking commitments |

## 7. Integration

| # | Question | Required answer |
|---|----------|-----------------|
| 7.1 | What integrations does the platform support today (DMS, CLM, case management, email, calendar)? | List all current integrations |
| 7.2 | Does the platform support Model Context Protocol (MCP) or equivalent open integration standards? | YES preferred |
| 7.3 | What APIs are available for custom integration? | API documentation provided |
| 7.4 | Can the platform integrate with the firm's existing identity provider (Okta, Azure AD, etc.)? | YES required |
| 7.5 | What is the process for requesting new integrations? | Specific process and typical timeline |

## 8. Customization and skill authoring

| # | Question | Required answer |
|---|----------|-----------------|
| 8.1 | Can the firm author and deploy custom workflows (skills, prompts, agents) without vendor assistance? | YES preferred |
| 8.2 | Are custom workflows portable (i.e., can they be exported in standard formats)? | YES preferred — markdown / open formats |
| 8.3 | Does the platform support a private organizational marketplace for firm skills? | YES preferred for larger firms |
| 8.4 | What is the workflow for sharing skills across the firm with appropriate scoping? | Specific feature description |

## 9. Termination and data portability

| # | Question | Required answer |
|---|----------|-----------------|
| 9.1 | On contract termination, what is the timeline for data deletion? | Specific commitment |
| 9.2 | Can the firm export all its data (configurations, custom skills, audit logs) on termination? | YES required |
| 9.3 | Are there any vendor-lock-in features that would make migration difficult? | Honest disclosure |
| 9.4 | Are skills, prompts, and configurations stored in open, portable formats? | YES preferred |

## 10. Custom firm questions

[Add firm-specific questions here, e.g.:]

- [ ] Can the platform integrate with our [specific DMS]?
- [ ] Does the platform handle [specific regulatory framework] requirements?
- [ ] Can the platform serve [specific client industry] with their data residency needs?

---

## Evaluation matrix

After receiving the vendor's responses, score each section:

| Section | Weight | Score (1-5) | Weighted |
|---------|--------|-------------|----------|
| 1. Contract | 20% | | |
| 2. Infrastructure | 20% | | |
| 3. Deployment | 15% | | |
| 4. Matter | 15% | | |
| 5. AI accuracy | 15% | | |
| 6. Operational | 5% | | |
| 7. Integration | 5% | | |
| 8. Customization | 3% | | |
| 9. Termination | 2% | | |
| **Total** | **100%** | | |

Adjust weights based on firm priorities. A litigation-heavy firm weights matter scoping and audit higher. A transactional firm weights integration and customization higher.

A vendor scoring under 3.0 weighted overall is not enterprise-ready for legal use, regardless of market hype. A vendor scoring 4.0+ across all sections is rare.

## Red flags

Stop the evaluation if any of these surface:

- Vendor refuses to share SOC 2 report under NDA
- Vendor cannot provide a contractual no-training-on-inputs guarantee
- Vendor's data retention defaults can't be turned off, even for sensitive matters
- Vendor cannot demonstrate per-matter or per-project workspace isolation
- Vendor has had an unresolved security incident in the past 24 months
- Vendor's roadmap shows no investment in legal-specific features
- Vendor's customer base in legal is fewer than 5 firms

---

## Process

1. Send the questionnaire to the vendor
2. Require written responses (not phone)
3. Review responses with IT, GC, and responsible partner
4. Score using the evaluation matrix
5. Pilot in a controlled scope with documented evaluation criteria
6. Decide based on pilot data, not just vendor responses

Total elapsed time: typically 4-8 weeks for an enterprise legal-AI vendor evaluation. Do not skip steps to compress.

---

## Customization notes

- Replace bracketed placeholders with firm-specific details
- Adjust weights in the evaluation matrix to firm priorities
- Add firm-specific questions in §10
- Run past your IT and GC before sending

This questionnaire is informed by industry-standard vendor evaluation practices and the specific risks of AI in legal work. It is not exhaustive; vendors with novel architectures may require additional probing. It is also not legal advice. Have your own counsel review before adopting as a firm standard.

MIT licensed. Part of master-claude-for-legal: github.com/sboghossian/master-claude-for-legal
