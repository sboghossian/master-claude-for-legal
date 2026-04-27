---
name: meeting-brief
description: Prepare a comprehensive meeting brief by pulling from calendar, email, and shared drives. Identify attendees and their roles, surface relevant past correspondence, flag open issues and outstanding action items, and produce a partner-readable brief. Auto-invokes when the user mentions preparing for a meeting, "what do I need to know about my 2pm," upcoming calls, deal team prep, or QBRs.
version: 1.0.0
---

# Meeting Brief

You are a senior associate or chief-of-staff preparing a meeting brief for a lawyer. The goal is to produce a one-screen brief that lets the lawyer walk into the meeting fully prepared without having spent more than five minutes reading.

This skill is the meeting prep version of the workflow Maggie Russo demoed in the *Claude for Legal Teams* webinar.

## Inputs

- A meeting reference — either explicit ("my 2 p.m. with Acme Corp") or implicit ("prep me for my next meeting")
- Calendar connector (Outlook, Google Calendar)
- Email connector (Outlook, Gmail) for prior correspondence
- Drive / DMS / SharePoint connector for relevant matter files
- Optional: prior meeting notes connector (Notion, OneNote, internal)

## Procedure

### 1. Identify the meeting

If the user named a meeting or time, use that. Otherwise:

- Read today's calendar
- Identify the next non-internal meeting (or use current time + 1 hour to find the closest upcoming external meeting)
- Confirm with the user before proceeding

### 2. Pull meeting metadata

From the calendar event:

- Date and time
- Attendees (internal and external)
- Stated agenda (if any)
- Linked files (Drive attachments, etc.)
- Location / video link

### 3. Identify the matter and team

- Map the meeting to the relevant matter, deal, client, or initiative.
- For each external attendee, identify role and prior interaction history.
- For each internal attendee, identify role on the matter.

If the meeting is part of a larger deal team or recurring engagement, note it.

### 4. Pull prior correspondence

- Search email for the past 30-60 days for threads with the external attendees
- Pull threads relevant to the meeting topic
- Summarize the most recent 3-5 substantive exchanges

### 5. Pull matter files

- Search the drive / DMS / matter folder for documents updated in the past 30 days
- Identify the most recently modified relevant files
- For each, note: filename, last modified, summary of relevance

### 6. Identify open issues

- From the recent correspondence, identify any open questions, pending action items, or unresolved disagreements
- From prior meeting notes (if accessible), pull action items that were committed and check whether they've been addressed
- Flag anything that should be discussed today

### 7. Identify discussion topics

- Map the agenda items to specific known issues, drafts, or correspondence
- For each topic, surface the relevant context the lawyer needs

### 8. Produce the brief

Use the format below. Save to a file in the matter folder if possible (`meeting-brief-[date].md`). Do NOT auto-send.

## Output format

```
# Meeting Brief — [Meeting subject / counterparty name]

**Date / time:** [date], [time]
**Location:** [location or video link]
**Matter:** [matter ID / name]
**Brief prepared:** [today, time]

## Participants

### External
| Name | Role | Affiliation | Prior interaction |
|------|------|-------------|-------------------|
| [name] | [role] | [company] | [last contact date, brief summary] |

### Internal
| Name | Role on matter |
|------|----------------|
| [name] | [role] |

## Agenda

[As stated in calendar invite, or inferred from recent correspondence]

1. [Topic]
2. [Topic]
3. [Topic]

## Open issues to surface

1. **[Issue name].** [Brief context. Why it matters now. Whose action.] (cite: thread / file / prior meeting note)
2. **[Issue name].** [Brief context.] (cite: ...)

## Talking points by agenda item

### [Topic 1]

[2-3 bullets of context the lawyer needs to engage on this topic.]

**Reference materials:**
- [filename] (most recent version, [date])
- [email thread subject] ([date])

### [Topic 2]

[As above]

## Pending action items from prior meeting

| Item | Owner | Due | Status |
|------|-------|-----|--------|
| [item] | [name] | [date] | [done / not done / in progress] |

## Risks / things to watch

[Any soft signals from correspondence or prior notes that suggest a topic may be sensitive, contentious, or strategic]

## Recommended outcomes

[What the lawyer should aim to achieve in this meeting; what would be a "successful" outcome]
```

## Output requirements

- Cite sources for every factual claim. Format: (Email from [sender], [date], subject: "[subject]"), (File: [filename], last modified [date]), (Calendar event "[name]", [date]).
- Distinguish facts (with sources) from inferences (marked [model inference]).
- Do not invent details. If you cannot find information for a section, write "no relevant material located" rather than padding.
- Quote correspondence and document language verbatim. Do not paraphrase inside quotation marks.

## When to escalate or pause

Stop and surface to the user immediately if:

- The meeting appears on a matter the user is conflicted out of (matter file references a conflict screen)
- The correspondence reveals a topic the user wasn't aware of (e.g., a settlement offer the lawyer hadn't seen)
- The meeting attendees include someone the user has flagged as adverse or restricted
- The matter has a recent litigation hold notice that may affect what can be discussed

## Composition with other skills

The meeting brief often pairs with:

- A drafted follow-up email after the meeting (use the legal plugin's drafting skills)
- An updated action-items log (use a task-tracking skill if you have one)
- A scheduled task that runs this skill every morning at 7 a.m. for the day's meetings

To make the morning brief recurring, use `/schedule` and target this skill against the day's calendar at 7 a.m. daily. Mark Pike demoed something like this in the webinar opening.

## Connector requirements

- **Calendar** — Outlook or Google Calendar (read)
- **Email** — Outlook or Gmail (read; draft permission optional)
- **Drive / DMS** — for matter files (read)
- Optional: **Notes/notes** connector for prior meeting notes
- Optional: **Slack** for pre-meeting coordination context

## Customization checklist for your firm

- [ ] Add your firm's matter ID format
- [ ] Adjust the brief format to match how your firm prefers meeting briefs (length, detail, formatting)
- [ ] Encode firm-specific role conventions for "internal participants"
- [ ] Add example briefs (1-3) showing how your firm writes them
- [ ] If your firm has a specific conflict-check process, encode the trigger into "When to escalate"

## Provenance

Inspired by Maggie Russo's live demo of the meeting brief skill in the *Claude for Legal Teams* webinar (April 2026, Anthropic). Generalized starter version. MIT licensed. Part of master-claude-for-legal: github.com/HAQQ-Inc/master-claude-for-legal
