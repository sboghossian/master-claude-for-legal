# MCP Connector Catalog

In May 2026, Anthropic shipped a "connected legal stack" — twenty-plus MCP connectors purpose-built for legal work. This is the catalog: what each connector is, what it lets Claude do, and when to reach for it.

For the security model (permission grids, allow-lists, audit trails), see `references/mcp-hardening.md`. This file is about what's available, not how to configure it safely. Read both.

Mark Pike's framing: *"If plugins are the brain in these playbooks, connectors are the hands."* The plugin tells Claude how to do legal work. The connector is how Claude reaches into where your documents actually live.

---

## Why this catalog exists

Two reasons.

**Reason one — coverage check.** Most legal teams will look at this list and find half their stack on it. The other half is either MCP-ready and not yet on the official list, or MCP-pending and worth pushing your vendor on. The list is a starting point for "what could we wire?", not a final answer.

**Reason two — MCP is an open standard.** Mark's emphasis in the May webinar: *"If your tool isn't on the slide, you can build the connector."* You are not stuck with what Anthropic shipped. Any system with a documented API can be wrapped in an MCP server. Several of the connectors below were built by partners, not by Anthropic.

---

## Catalog by category

### Contracts and CLM

Connectors into contract repositories, CLM platforms, and deal rooms. These power commercial-legal, M&A, and procurement workflows.

- **Box** — virtual deal rooms, document repositories. Used in the May webinar demo for the Pluto/Acme M&A tabular review.
- **Best-of-breed CLM platforms** — multiple, named on the May webinar slide. If your firm runs Ironclad, LinkSquares, Agiloft, or similar, check whether your vendor has shipped an MCP connector.
- **Document repositories / DMS** — iManage, NetDocuments, SharePoint. Several are now MCP-native; some still rely on the file-system bridge.

**When to use.** Any time the work involves more than three documents that already live somewhere structured. Don't drag-and-drop forty NDAs into a chat window when the CLM has them indexed.

### Case Law and Research

- **Thomson Reuters / CoCounsel** — fiduciary-grade case law and statute research. The May webinar called this out specifically as a partnership. Pattern: start a research thread in Claude, hand off to CoCounsel for deeper authority work, return with the citations grounded.
- **Free Law Project** — millions of opinions, citation data, dockets. Open access. This is the access-to-justice equivalent of WestLaw for solo practitioners and clinics.
- **Descrybe** — primary law research, jurisdiction-aware.

**When to use.** Any time a memo or motion needs a citation to a real case or statute, *and especially* when the citation chain could be fabricated by a chat-only model. See `references/verification.md` for the round-trip citation pattern.

### eDiscovery and Litigation Support

- **eDiscovery platform connectors** — multiple. Specific vendor names were not all called out by name in the May webinar; the slide showed coverage.
- **Deposition / transcript search** — Mark called out that litigators use Claude inside Anthropic to do transcript searches across long depositions and spot themes.

**When to use.** Document review at scale, deposition theme analysis, motion-to-compel briefing where you need to surface specific passages from a large discovery set. Cowork is the right surface here, not chat. See `references/long-documents.md`.

### Legal AI Networks

- **Harvey** — full legal-AI platform with its own MCP. Claude can call Harvey skills; Harvey can run on Claude's models. Two-way.
- **Lovable** — community of skill builders. Pre-built skills you can pull in.
- **Solve Intelligence** — patent and IP-specific. Patentability searches, prior art, claim drafting support.
- **LSuite** — two MCP connectors, not one. Full integration.

**When to use.** When your firm has already standardized on one of these platforms, the MCP makes them composable with Claude rather than choosing one or the other.

### Access to Justice

These are the connectors that exist so that solo practitioners, legal aid clinics, public defenders, and self-represented litigants get the same leverage that BigLaw gets. Anthropic discounts the underlying Claude license through **Claude for Nonprofits** for qualifying organizations.

- **Free Law Project** — see above.
- **Descrybe** — see above.
- **Courtroom5** — self-represented litigant tooling. Sonia Ebron's framing, quoted by Mark in the May webinar: *"Claude helps people meet them where they are, in the moment they're scared and searching for answers."*
- **BoardWise** — governance and board materials, often relevant in clinic and nonprofit contexts.

**When to use.** If you work in a clinic, public defender's office, legal aid org, or nonprofit, install these first. Combine with the **Legal Clinic plugin** (see `references/practice-area-plugins.md`) for built-in supervision flags.

### Productivity and Communication

Not legal-specific, but central to how legal work actually moves through an organization. The May webinar demo used Slack to brief a deal lead and an Excel add-in to surface tabular results.

- **Microsoft 365 family** — Word, Excel, PowerPoint, Outlook. See `references/microsoft-365.md` for the cross-surface context preservation pattern.
- **Slack** — channel writes, channel reads. Default to needs-approval on writes.
- **Google Workspace** — Gmail, Calendar, Drive, Docs, Sheets, Slides.

**When to use.** Always. Legal work doesn't end with the analysis — it ends when the right person reads the right summary. These connectors are how Claude carries the work from analysis to delivery.

---

## Picking a connector when several exist for the same job

You will encounter cases where two connectors cover similar ground (e.g. a CLM connector and a DMS connector both pointing at contract folders). The decision rule:

**Prefer the system that owns the canonical record.** If contracts are authored and version-controlled in a CLM, point Claude at the CLM, not the DMS copies. If matters live in iManage and the CLM is a downstream consumer, point at iManage.

**Avoid wiring both unless you have a reason.** Two connectors into overlapping data sources will produce inconsistent answers when the systems diverge. Pick one as the source of truth.

---

## What to do if your tool isn't here

Three options, in order of effort:

1. **Check whether the vendor has shipped an MCP server.** Most enterprise SaaS vendors are in the process. Search the vendor's docs for "MCP" or ask their support.
2. **Wrap the public API yourself or pay someone to.** MCP servers are not hard to write. A junior engineer can build one in a day for a simple REST API.
3. **File a request.** Mark explicitly invited reach-outs to the product partnerships team in the May webinar, especially for non-US jurisdictions and underserved practice areas.

---

## Configuration is not in this file

This catalog tells you what exists. Once you decide what to wire, go to `references/mcp-hardening.md` for the security configuration — permission grids, audit trail setup, OAuth scope decisions, and the rule that **writes are needs-approval, reads can be always-allow**.

---

## Source

Catalog derived from the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). Specific vendor mentions tracked back to the connector slide shown live and to direct verbal callouts. Vendor lists update — verify against the live Cowork marketplace before assuming any specific MCP is or isn't shipped.
