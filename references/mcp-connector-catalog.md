# MCP Connector Catalog

In May 2026, Anthropic shipped a "connected legal stack" — twenty-plus MCP connectors purpose-built for legal work. This is the catalog: what each connector is, who builds it, and what category it sits in.

For the security model (permission grids, allow-lists, audit trails), see `references/mcp-hardening.md`. This file is about what's available, not how to configure it safely. Read both.

Mark Pike's framing: *"If plugins are the brain in these playbooks, connectors are the hands."* The plugin tells Claude how to do legal work. The connector is how Claude reaches into where your documents actually live.

---

## Why this catalog exists

Two reasons.

**Coverage check.** Most legal teams will look at this list and find half their stack already on it. The other half is either MCP-ready and not yet on the official list, or MCP-pending and worth pushing your vendor on.

**MCP is an open standard.** Mark in the May webinar: *"If your tool isn't on the slide, you can build the connector."* You are not stuck with what Anthropic shipped. Any system with a documented API can be wrapped in an MCP server. Several of the connectors below are partner-built, not Anthropic-built.

---

## Catalog by category

### Contract Lifecycle Management

The CLM/contracts category — where vendor agreements, NDAs, and order forms live and get version-controlled.

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Ironclad** | Ironclad | Query contracts in plain language across the Ironclad repository |
| **DocuSign** | DocuSign | Surface key terms, manage signature workflows |
| **Definely** | Definely | Live, deterministic access to contract structure (clauses, defined terms, refs) |

### Document Management Systems

The DMS layer where matters and firm documents live.

| Connector | Vendor | What it unlocks |
|---|---|---|
| **iManage** | iManage | Permission-bound, auditable access to governed matter content |
| **NetDocuments** | NetDocuments | Search and retrieve while respecting workspace governance |
| **Box** | Box | Files, deal rooms, securely shared workspaces |

### Deal Rooms / Transactions

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Datasite** | Datasite | Virtual data room access for M&A transactions |
| **Box** | Box | Virtual deal rooms (used in the May webinar's Pluto/Acme demo) |

### eDiscovery and Litigation Support

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Relativity** | Relativity | Matter management and RelativityOne analysis |
| **Everlaw** | Everlaw | Search and organize litigation documents |
| **Consilio** | Consilio | Live matter access with Aurora Legal AI |

### Legal Research

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Thomson Reuters / CoCounsel** | Thomson Reuters | Fiduciary-grade system for end-to-end drafting and research |
| **Midpage** | Midpage | Case law database with source verification |
| **Trellis** | Trellis | State trial-court data and judge analytics |
| **Legal Data Hunter** | Legal Data Hunter | 31M+ documents across 160+ jurisdictions |

### Legal AI Networks and Skill Libraries

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Harvey** | Harvey | Legal intelligence and Vault project analysis |
| **Solve Intelligence** | Solve Intelligence | Patent literature, claim mapping (IP/patent prosecution) |
| **Lawve AI** | Lawve AI | Curated library of legal-AI skills you can pull into Claude |
| **Lloyd** | The L Suite | Connect with The L Suite's Braintrust member platform |
| **TopCounsel** | The L Suite | Match in-house counsel with outside counsel |

> The L Suite ships **two** MCPs, not one — Lloyd and TopCounsel cover different parts of their offering.

### Access to Justice / Public Service

The connectors that exist so that solo practitioners, legal aid clinics, public defenders, and self-represented litigants get the same leverage that BigLaw gets. **Claude for Nonprofits** discounts the underlying Claude license for qualifying organizations.

| Connector | Vendor | What it unlocks |
|---|---|---|
| **Free Law Project** | Free Law Project | CourtListener opinions and PACER docket data |
| **Descrybe** | Descrybe | Case research and citation tools |
| **Courtroom5** | Courtroom5 | Self-representation support across all 50 states |
| **BoardWise** | BoardWise | State board guidance for licensed professionals |

Sonia Ebron's framing on Courtroom5, quoted by Mark Pike: *"Claude helps people meet them where they are, in the moment they're scared and searching for answers."*

### Productivity and Communication (general, not legal-specific)

Not legal-specific, but central to how legal work actually moves through an organization. The May webinar demo used Slack to brief a deal lead and an Excel add-in to surface tabular results.

- **Microsoft 365 family** — Word, Excel, PowerPoint, Outlook. See `references/microsoft-365.md` for the cross-surface context preservation pattern.
- **Slack** — channel writes, channel reads. Default to needs-approval on writes.
- **Google Workspace** — Gmail, Calendar, Drive, Docs, Sheets, Slides.

---

## Picking a connector when several exist for the same job

You will encounter cases where two connectors cover similar ground (e.g. a CLM connector and a DMS connector both pointing at contract folders). The decision rule:

**Prefer the system that owns the canonical record.** If contracts are authored and version-controlled in Ironclad, point Claude at Ironclad, not the iManage copies. If matters live in iManage and the CLM is a downstream consumer, point at iManage.

**Avoid wiring both unless you have a reason.** Two connectors into overlapping data sources will produce inconsistent answers when the systems diverge. Pick one as the source of truth.

**The legal-research stack is genuinely multi-source.** Unlike the CLM/DMS layer, research often *should* hit multiple connectors — Thomson Reuters for authoritative case law, Midpage for fast verification, Trellis for state trial-court data, Free Law Project for open-access fallback. Each covers different ground.

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

Catalog drawn from Anthropic's *Claude for the Legal Industry* announcement (claude.com/blog, May 2026), the LawNext write-up (lawnext.com, May 2026), and the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). Vendor lists update — verify against the live Cowork marketplace before assuming any specific MCP is or isn't shipped.
