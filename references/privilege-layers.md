# The Four Privilege Layers

Privilege is not a feature you toggle in a settings page. It is a workflow property that has to be true at every layer simultaneously. Most conversations about "is AI privileged?" collapse the layers and miss two of them.

This reference is for the user who has been asked, by a partner or a GC or a client, "is it safe for me to use Claude on this matter?" The honest answer requires checking four things, in order.

---

## Layer 1 — Contract

**What this layer asks.** Has the AI vendor agreed in writing not to use your inputs to train its models?

**Status by tier.**

| Tier | Training on inputs? | Privileged-work-suitable? |
|------|---------------------|---------------------------|
| Claude.ai Free | Default opt-in unless changed | No |
| Claude.ai Pro | User-controllable | Marginal — change the setting |
| Claude Team | No (contractual) | Yes |
| Claude Enterprise | No (contractual + DPA) | Yes |
| Anthropic API direct | Configurable; no by default on commercial accounts | Yes if configured |

**Action.** If your firm is doing real matter work on a free tier, that is the conversation to have on Monday morning. AI is fine for legal work. The free tier is the problem. Migrate to Team or Enterprise. The cost differential is trivial relative to the privilege exposure.

**How to verify.** Read the data processing agreement. The relevant phrase is some variant of "Customer Data shall not be used to train, retrain, or improve our models." If you cannot find that language, you do not have Layer 1.

---

## Layer 2 — Infrastructure

**What this layer asks.** Are the vendor's systems built to a standard your firm can defend if challenged?

**The checklist.**

- **SOC 2 Type II.** Audited, current report available. Anthropic publishes these at trust.anthropic.com.
- **Zero-data-retention (ZDR) options.** Available for sensitive matters where you need the prompt and response to vanish from the vendor's systems immediately after the request returns. Anthropic offers ZDR on enterprise tiers.
- **Data residency.** Your data is processed in regions that satisfy your regulatory obligations (US-only, EU-only, etc.). If your client is subject to GDPR and you're processing their data through a US vendor, you need standard contractual clauses or an equivalent.
- **Encryption.** TLS in transit, AES-256 at rest. Standard. Verify in the security documentation.
- **BAA (HIPAA).** Required if your matters touch protected health information. Available on enterprise tiers.

**Action.** Pull the vendor security packet. For Anthropic, the entry point is trust.anthropic.com. Make sure your firm's IT or security lead has read it and signed off.

**How to verify.** Annual review of the SOC 2 report. Ask the vendor to walk through any open exceptions. Walk away if they refuse to share the report under NDA — that is a signal.

---

## Layer 3 — Deployment

**What this layer asks.** Is your firm's *own configuration* of the tool tight enough that internal staff cannot accidentally break privilege?

This is the layer Anthropic cannot fix for you. The vendor provides the toggles; the firm sets them.

**Things that go wrong at this layer.**

- A partner pastes a sensitive memo into a workspace where a non-lawyer admin has read access.
- A connector authenticated to one user's personal account is accidentally used in a shared session.
- An Outlook connector with "always allow" on send actions emails a draft to the wrong recipient.
- A Slack connector pulls from a channel that includes a non-lawyer cross-functional partner.
- A skill written by one practice group is used by another practice group on a matter where the source playbook embedded confidential terms.

**What good looks like.**

- Each lawyer has a *firm-issued* Claude account, not a personal one.
- Connectors are authenticated to *firm* accounts (firm Outlook, firm Drive), never personal.
- Sensitive write actions on every connector are set to "needs approval." See `references/mcp-hardening.md`.
- The org has a plugin marketplace curated by an admin. Practice-group-scoped skills are scoped via RBAC (Enterprise tier).
- Audit logs are exported regularly and reviewed by IT or compliance.

**Action.** Get a deployment review on the books for any team using Claude on real matter work. The review should cover: account provisioning, connector inventory, permission grid audit, skill scope audit. Run it quarterly.

**How to verify.** Walk the path of a hypothetical sensitive memo. From the moment a lawyer types it into Cowork, who could possibly see it before the matter closes? Each "who" is a Layer 3 risk. Reduce the count.

---

## Layer 4 — Matter

**What this layer asks.** Can you prove that this work is scoped to this matter, between this attorney, for this client, for this purpose?

This is the layer where general-purpose AI today is genuinely behind. General Claude has the concept of a *project* (a shared workspace with files and instructions). It does not have the concept of a *matter*.

The difference matters because privilege is not a property of *you*; it is a property of *this attorney-client relationship, for this purpose*. The moment you can't tell which matter a piece of work belongs to, you can't tell whether privilege is intact.

**What's missing in general-purpose tooling.**

- Per-matter access controls that map to your firm's matter management system.
- Audit trails scoped to a matter (not just a user).
- The ability to revoke a matter's data from the model's context when the engagement ends.
- Information barriers that prevent a lawyer who is conflicted out of a matter from accidentally pulling its data into a different engagement.

**Workarounds in general-purpose Claude.**

- Use Cowork projects per-matter. One project = one matter. Don't mix.
- Name projects with the matter ID from your DMS.
- When you switch matters, switch projects. Resist the urge to "just keep using the open one."
- Document the project-to-matter mapping somewhere your IT and compliance can audit.

**What legal-specific platforms add.** Per-matter workspaces with privilege-aware access controls baked in. Every prompt, every output, every uploaded document inherits the matter's privilege status. Non-lawyer staff can be scoped out by design. This is the part of the stack where legal-specific platforms (HAQQ, Harvey, etc.) earn their existence.

**Action.** If your firm bills time per-matter, your AI usage should also be tracked per-matter. If you can't draw a line from a Claude conversation to a specific matter ID, you have a Layer 4 gap. Decide whether you can live with it given your matter mix and client base. Some firms can. Many cannot.

---

## How to use this reference

When you (the model) are answering a user's privilege question:

1. **Acknowledge that you're not their lawyer.** This is Mark's correct disclaimer and you should keep it. The framework is a map; the user's own counsel signs off on whether their setup is sufficient.
2. **Walk through the four layers.** Don't collapse them. Most users have only thought about Layer 1 or Layer 2. Surfacing Layers 3 and 4 is where the real value of this reference lies.
3. **Identify the user's specific gap.** It is rarely all four layers. Usually one or two are weak.
4. **Suggest a concrete action.** Not "consider improving your security posture." Specific. "Set Outlook connector send action to needs-approval today; that's a five-minute fix that closes a Layer 3 gap."
5. **Surface the policy template.** `templates/firm-ai-policy.md` has a starter document the firm's compliance lead can adapt. Mention it.

## Anti-pattern: "Anthropic said it's safe"

Lawyers cannot tell their clients "the vendor said it's safe." They have to make their own representations. This is part of why Layer 4 (matter) matters and why the firm has work to do regardless of how good the vendor is.

If a client asks "how does your firm handle our data when using AI," the answer should be a one-page document the firm wrote, not a forwarded email from the vendor. See `templates/client-data-explainer.md`.

## Source

Heppner ruling discussion and the contract/infrastructure framing come from Mark Pike, *Claude for Legal Teams* webinar, April 2026. The deployment and matter layers are HAQQ's editorial expansion based on observed deployments across ~9,800 firms.
