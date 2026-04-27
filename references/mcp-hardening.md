# MCP Hardening

The five-minute exercise every legal team should run before doing real matter work in Claude. Then the longer governance posture for firms that take this seriously.

---

## The five-minute hardening

For every connector you've wired (Outlook, Gmail, Drive, Slack, iManage, etc.), open its permission grid and apply this rule:

> **Read actions can be `always allow`. Every action that mutates state — send, modify, delete, move, share — should be `needs approval`.**

That is the whole hardening. If you do nothing else from this reference, do this. It buys you most of the operational safety with almost no friction loss.

The practical workflow becomes: Claude can read and analyze freely; whenever it wants to act on the world (send an email, modify a document, move a file), it asks first. You click approve or deny. The lag is two seconds per action. The privilege exposure prevented is enormous.

## Why this matters

MCP connectors are *persistent ambient access*. They are not one-shot pulls. Once you wire your inbox to Claude, Claude has standing capability to read that inbox in any future conversation that has the connector enabled, until the moment you revoke it. This is closer in shape to an OAuth grant than a file upload.

The implication: if a connector with `always allow` on send is wired to your firm Outlook, any conversation that goes off the rails (a misinterpreted instruction, an injected prompt from a malicious document, a model error) can result in an email being sent without your approval. That is the failure mode you're hardening against.

Read-only access is still ambient, but the failure mode is bounded — at worst, the model reads something it shouldn't have. With write access, the model can actively cause harm.

## Per-action defaults — the grid

| Action category | Default for read-only matters | Default for matter work |
|-----------------|-------------------------------|-------------------------|
| Read messages / files | always allow | always allow |
| Search / list | always allow | always allow |
| Draft (without sending) | always allow | always allow |
| Send email | needs approval | **needs approval** |
| Reply to email | needs approval | **needs approval** |
| Modify document | always allow | **needs approval** |
| Delete | needs approval | **blocked** |
| Move / archive | always allow | **needs approval** |
| Share externally | needs approval | **blocked** |
| Create calendar event | always allow | **needs approval** |
| Update calendar event | needs approval | **needs approval** |
| Post to channel | needs approval | **needs approval** |

The bold rows are the ones we'd treat as non-negotiable for any legal team using Claude on real matters.

## Per-connector specifics

### Outlook / Gmail

The high-risk actions: send, reply, modify draft, delete. Lock all of these to `needs approval`. Read can be free.

A subtle one: **forwarding**. Treat forwarding as a send action, not a read. Some connectors split these incorrectly.

### Calendar

Read calendar = always allow. Create / modify / cancel events = needs approval. Pay attention to attendees. A meeting Claude creates or modifies on your behalf may end up sending invites to people who shouldn't see it.

### Drive / OneDrive / SharePoint

Read = always allow. Modify = needs approval (a Claude-modified document looks legitimate to your team and may bypass review). Share externally = blocked. Sharing externally is how a privileged document ends up where it shouldn't.

### Slack / Teams

Read = always allow. Post to channel = needs approval. Direct message a user = needs approval. Be careful about scope: if you authenticate Claude to your own Slack account, Claude can see every channel you can see. Audit which channels are in scope.

### iManage / NetDocuments / DMS

Read = always allow. Modify a matter document = needs approval. Move between matters = needs approval (cross-matter moves are a classic privilege break vector). Delete = blocked. Apply security holds = needs approval.

### Linear / Jira / case management

Read = always allow. Create or modify tickets = needs approval. Move between projects = needs approval.

## Connector inventory

Maintain a list. For each connector you've wired, capture:

1. Connector name
2. Account it's authenticated to (firm vs personal — should always be firm)
3. Date wired
4. Date last reviewed
5. Permission grid state (snapshot)
6. Who has access to use it
7. Reason for the connector (which workflow needs it)

Review this inventory quarterly. Revoke connectors no one is using. The least-privilege principle applies to AI access exactly the way it applies to any other access.

## The personal-account problem

Do not wire personal accounts to firm-licensed Claude tools. Do not wire firm accounts to personal Claude tools.

The reason is not paranoia. It is audit trail. When the firm's compliance lead needs to answer "what did Claude read about Matter X," they need to find that information in firm-controlled audit logs, not in a partner's personal Google account.

The webinar showed Maggie's personal Gmail wired to her personal Cowork. That is fine for a demo. It is a fireable offense at most firms with a real information governance program. Be the firm with the program.

## Rotation and revocation

Treat connector grants like access cards.

- **Quarterly rotation.** Re-authenticate every connector every 90 days. This forces you to confirm the connector still belongs in the inventory.
- **Departure revocation.** When a lawyer or staff member leaves the firm, revoke their connector grants in addition to revoking their other access. A forgotten OAuth grant outlives an employee badge.
- **Matter-end revocation.** When a matter closes, revoke connectors that were authenticated specifically for that matter (less common but happens with client-system grants).

## Skill-level controls

Connectors are one layer; skills are another.

A skill that says "send this email to the team" inherits the connector's permission settings. If the connector has send on `needs approval`, the skill will pause and ask. If the connector has send on `always allow`, the skill will send without asking, even if the skill is poorly written or being invoked under a misunderstanding.

The implication: **the connector permission grid is the last line of defense.** Set it conservatively and skills can be written without paranoia. Set it permissively and every skill becomes a potential vector.

## RBAC (Enterprise tier)

If you're on Claude Enterprise:

- **Org-wide connector policies.** An admin can pre-configure connectors with locked permission grids that users cannot override. Use this to enforce the bold rows in the per-action defaults table above firm-wide.
- **Practice-group scoping.** Connectors can be made available to specific user groups only. Real Estate sees Real Estate connectors; Litigation does not.
- **Connector approval workflow.** Some firms route new connector authentications through IT for review before they activate.

## Audit log review

Anthropic's Compliance API exports audit logs of who used what connector when. For firms operating at scale or under regulatory scrutiny, schedule monthly review of these logs. Look for:

- Unusual access patterns (a lawyer reading from a matter they're not assigned to)
- Connector usage from non-firm IPs
- Bulk-read operations against sensitive sources
- Failed authentications (could indicate compromised credentials)

## When something goes wrong

Have an incident playbook. The minimum:

1. **Detect** — usually via the audit log or a user noticing.
2. **Contain** — revoke the implicated connector immediately.
3. **Assess** — what did the connector access while compromised? What did Claude see, draft, or send?
4. **Notify** — internal stakeholders (managing partner, GC, IT). Affected clients if the breach reaches them.
5. **Remediate** — re-authenticate with tightened permissions; update the inventory; brief the team.

Most firms will never trigger this playbook. The ones that do are glad they wrote it.

## How to use this reference

When the user is wiring a new connector or asking about MCP security:

1. Walk through the five-minute hardening first. Don't go deep on governance until the basics are set.
2. Pull the per-connector specifics that apply to the user's actual integrations.
3. If the user is at a firm with multiple lawyers using Claude, surface the inventory and rotation practices.
4. Mention RBAC if the user is on Enterprise; mention the upgrade if they need it but aren't.

## Source

Anthropic's connector documentation, Maggie Russo's permission-grid demo from the *Claude for Legal Teams* webinar (April 2026), and HAQQ's incident-response observations from deployments.
