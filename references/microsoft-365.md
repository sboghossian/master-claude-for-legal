# Microsoft 365: Word, Excel, PowerPoint, Outlook

Most legal work happens in Microsoft 365. Claude is now inside it — not as a separate window you copy-paste to, but as a sidebar that carries matter context across the four apps.

Mark Pike's framing in the May 2026 webinar: *"We're not asking you all to learn a new tool. We're putting Claude inside of these tools, and it carries matter context."* The thing that matters about this is not "Claude can edit a Word doc." The thing that matters is the cross-surface continuity.

---

## The four surfaces

### Word

The legal-relevant one. Three workflows:

- **Drafting** — generate clauses, sections, full first drafts grounded in firm precedent.
- **Redlining** — track-changes edits against a playbook. The user sees standard Word redlines; Claude maintains the rationale.
- **Clause-by-clause comparison** — pull a contract against a playbook clause-by-clause and surface deviations.

**Pre-requisite.** A paid Microsoft 365 license and a recent Word version. Office 2019 and earlier won't run the add-in. If your firm is still on 2019, this is on the IT roadmap, not a tomorrow problem.

### Excel

Used in the May webinar demo. Pattern is **tabular review** — many documents, same schema, output as a spreadsheet with one row per document and one column per field. Each cell carries either a source citation (clickable deep link to the source document) or a "could not verify" flag.

The Pluto/Acme demo: 30 contracts in a Box deal room → tabular review skill → Excel spreadsheet with severity ratings (blocking / high / medium / low) and a findings-by-category tab. The lawyer then asked Claude to pull verbatim clause language into a new column, citing back to the source.

This pattern is now a starter skill in this pack — see `skills/tabular-review.md`.

### PowerPoint

The "I just did the analysis, now I have to present it to the board" surface. Claude generates client updates, matter summaries, and stakeholder briefings *from the matter record* rather than from scratch.

Less of a daily-driver than Word/Excel, but high leverage for in-house counsel who present to executives and for partners who present to clients.

### Outlook

Drafts in your voice. The May webinar demo example: brief a deal lead on contract findings, draft as a Slack message — but Mark explicitly noted the same pattern works in Outlook for email. Claude knows the firm's escalation style, the executive's preferred tone, the standard CC list.

Default rule (and this stays default forever): **never auto-send.** Drafts only. The human hits send.

---

## The thing that's genuinely new: cross-surface context

This is the part of the May launch that's hard to convey in a slide. The same Claude session, with the same plugin, with the same customization profile, follows you across all four apps.

The demo flow:

1. **Cowork** — start the matter, run the cold-start interview, point at the Box deal room, invoke the tabular-review skill.
2. **Excel** — open the spreadsheet Claude just produced. The sidebar shows the *same* Claude, with full memory of what happened in Cowork. Ask it to pull verbatim language into a new column. It does so, and it knows which Box documents it just analyzed.
3. **Slack / Outlook** — go back to Cowork or use the Slack/Outlook surface directly. Ask Claude to brief the deal lead on the findings. It writes in your voice, cites the right contracts, knows the deal team, and asks for approval before posting.

Mark's exact framing of why this matters: *"It didn't wipe its memory of what it is and what we were doing. It's just effortlessly continuing the workflow alongside of you."*

The contrast is the old pattern: paste the findings from a chat into Excel manually, then paste the Excel findings into an email manually, losing context twice. With cross-surface context, the lawyer is the editor, not the courier.

---

## What this looks like in practice

For an in-house corporate counsel doing M&A diligence:

| Surface | Work |
|---|---|
| Cowork | Spin up the matter, run tabular-review skill on deal-room contracts |
| Excel | Refine the review output, add columns, surface verbatim language |
| Word | Draft the disclosure schedule and reps deviations memo |
| PowerPoint | Generate the board-briefing deck |
| Outlook | Brief the deal lead, queue closing-team distribution |

For a litigator on a discovery review:

| Surface | Work |
|---|---|
| Cowork | Run thematic analysis across deposition transcripts |
| Excel | Tabular review of produced documents, hot-doc tagging |
| Word | Draft motion-to-compel sections referencing specific transcript passages |
| PowerPoint | Trial-prep summary deck |
| Outlook | Update the case team with the day's review status |

The plugin is the same. The customization profile is the same. The skills are the same. The surface changes based on where the work happens to live.

---

## What this is not

It is not a replacement for the desktop apps' editing surfaces. You still redline in Word, you still pivot in Excel, you still arrange slides in PowerPoint, you still hit send in Outlook. The sidebar is augmentation, not takeover.

It is not a generic LLM-in-Word add-in. The thing that distinguishes it is the carried matter context. Without that, this is a slightly-nicer version of Copilot. With it, this is a coworker who's already up to speed on the deal.

It is not a substitute for actual document management. Claude in Word does not replace your DMS. Save to iManage / NetDocuments / SharePoint the way you always did.

---

## Installation checklist

1. Microsoft 365 commercial license (consumer Office accounts won't enable add-ins from third parties in many enterprise tenants).
2. Modern Office build (Word 2021+ ideally, 365 monthly channel best).
3. Claude account on a tier with the M365 add-ins enabled (Team or Enterprise recommended; available on Pro/Max consumer with privacy preferences tuned — see `references/privilege-layers.md`).
4. Install the add-in via Word → Add-ins → Claude. Repeat for Excel, PowerPoint, Outlook. The sign-in is shared.
5. Confirm cross-surface context works: in Cowork, do a small piece of work; switch to Word; open the Claude sidebar; ask *"what were we just working on?"*. The answer should be the right matter.

If step 5 fails, the most common cause is that the user is signed into different Anthropic accounts across surfaces. Sign out everywhere; sign back in once on the same account.

---

## Privilege and confidentiality posture

The M365 surfaces inherit the underlying Claude tier's privacy settings. On Team/Enterprise, **no training by default** is the standard posture. On consumer Pro/Max, the user must toggle training off in privacy preferences.

For client-data work, confirm the privacy posture before turning the add-ins loose on a privileged matter. See `references/privilege-layers.md` for the four-layer framework.

---

## Source

Demo, framing, and cross-surface context behavior drawn from the *How Legal Teams Put Claude to Work* webinar (Anthropic, May 2026; Mark Pike and Harry from Applied AI). The Pluto/Acme tabular-review walkthrough is reproduced from the live demo.
