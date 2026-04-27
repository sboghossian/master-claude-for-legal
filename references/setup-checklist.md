# Setup Checklist

Four hours from zero to your first production-quality skill. Counter to the "AI takes six months and $100K of consulting" prior. This is the realistic walkthrough.

---

## Before you start

You need:

- A laptop (Mac, Windows, or Linux all fine).
- Admin or near-admin permissions on it (to install Cowork).
- A Microsoft 365 or Google Workspace account that you actually use for work. (Use the firm account, not a personal one. See `references/privilege-layers.md`.)
- A real piece of work to point at. Pick a task you've done at least five times this quarter. NDA triage, contract review, meeting prep, weekly status — pick one. The skill you build will target this exact task.
- One uninterrupted block of about four hours. You can split it across days; the work is sequential.

You do *not* need:

- An engineer to set this up for you.
- Six months of pilot.
- A vendor consultant.
- IT to build a custom integration.
- Permission from anyone above you, unless your firm requires IT approval to install desktop software.

---

## Hour 1 — Account and install

### Step 1.1 — Sign up for Claude on a commercial tier

Go to claude.ai. Sign up or sign in. Confirm you are on **Team** or **Enterprise**, not the free consumer tier.

If your firm doesn't have a Team or Enterprise subscription yet, this is the conversation to have today. See `references/privilege-layers.md` for why this matters. The cost is trivial relative to the privilege exposure of doing real work on the free tier.

### Step 1.2 — Install Cowork

Download Cowork from claude.ai/cowork. Run the installer. Sign in with the same account.

If your firm requires IT approval for new desktop software, file the ticket now. The conversation is much shorter than installing a new SaaS — Cowork runs locally, doesn't require firewall changes, and stores data per your firm's existing privacy posture on Claude.

### Step 1.3 — Install the legal plugin

In Cowork:

1. Open Customize (gear icon, top right)
2. Click Plugins
3. Find "Anthropic and Partners" section
4. Find "Legal" plugin
5. Click Install

Total clicks: about twelve. Total time: maybe ten minutes plus the wait for IT approval if applicable.

You now have the starter skills available: meeting brief, NDA triage, contract review, privilege log drafting, etc. They are starting templates. Don't use them as-is on real work yet.

---

## Hour 2 — Wire one connector

You will pick the connector that maps to where most of your daily work lives.

### Step 2.1 — Pick the right connector

For most lawyers, the right first connector is one of:

- **Outlook** — if you live in Microsoft email
- **Gmail** — if you live in Google email
- **iManage** — if you have it and your firm has wired the MCP integration
- **Drive** — if your work mostly lives in Google Drive
- **OneDrive / SharePoint** — if your work mostly lives in Microsoft cloud storage

Pick ONE for now. You can wire more later.

### Step 2.2 — Authenticate

In Cowork → Customize → Connectors:

1. Find your chosen connector
2. Click Connect
3. OAuth flow opens in your browser
4. Sign in with your firm account (not personal)
5. Approve the access scopes Claude requests
6. Return to Cowork; verify the connector now shows as connected

### Step 2.3 — Harden the permission grid

This is the five-minute exercise that prevents most operational problems.

For the connector you just wired:

1. Click into its settings
2. Find the action permissions list (read, send, modify, delete, etc.)
3. For every action that **mutates state** (send, modify, delete, share, create, post), set to **Needs Approval**
4. Read-only actions can stay on Always Allow

See `references/mcp-hardening.md` for the per-connector specifics. The default rule: Claude can read freely; whenever it wants to act on the world, it asks first.

### Step 2.4 — Test with a read query

Open a new conversation in Cowork. Type something like:

> "Summarize the email threads from this week with [a specific person you correspond with]."

Claude should:

1. Use the email connector (you'll see this in the conversation log)
2. Surface a few recent threads
3. Produce a brief summary

If this works, your connector is live. If it doesn't, troubleshoot the connection before proceeding. You cannot build skills against a connector that isn't working.

Allow about five extra minutes for this verification.

---

## Hour 3 — First skill remix

Now the fun part. You're going to take one starter skill and customize it for your firm.

### Step 3.1 — Pick the skill that matches your real work

In Cowork → Customize → Plugins → Legal plugin → expand the skill list, pick one:

- If your real work this quarter has been NDAs, pick **NDA triage**
- If it's been contract review, pick **contract review** or **commercial redlining**
- If it's been meeting prep, pick **meeting brief**
- If it's been privilege logs, pick **privilege log drafting**

Pick one. Don't pick three.

### Step 3.2 — Read the skill

Open the skill's markdown file. It will be roughly 100-300 lines of plain English. Read it. The whole thing.

You will notice several things. First, it's not magic — it's a careful prompt that tells Claude how to do the workflow. Second, it has placeholders ("the firm's standard playbook," "the firm's risk classification," "the firm's voice") where your actual firm-specific content needs to go. Third, the format is approachable; you can edit it.

### Step 3.3 — Remix it

Edit the skill to encode your firm's actual practice. Concretely:

- Replace generic "the firm's standard playbook" placeholders with your actual playbook positions. If you negotiate hard on indemnification caps, write down what your standard cap is and what fallbacks you accept.
- Replace generic risk classifications with your firm's actual risk taxonomy. If you use red/yellow/green, use that. If you use 1-5, use that.
- Replace generic voice with your firm's voice. If your firm writes in plain English, say so. If your firm uses specific defined-term conventions, encode them.
- Add any firm-specific clauses that should always be flagged. The clauses your senior partner always reviews twice should be in the skill.

This step takes 30-60 minutes. You will keep finding small things to add. Don't try to be exhaustive on the first pass — get to a 70% version and stop.

Save the skill as a personal skill (or share to your team if you have edit permission on shared skills).

If you want help with the meta-prompting, the built-in **skill creator** skill (from the legal plugin) walks you through this. Maggie Russo demoed it. It's recursive: Claude helping you write the skill that Claude will use.

See `references/skill-authoring.md` for the deeper guide.

---

## Hour 4 — Run on real work

### Step 4.1 — Pick a real document or task

Take one current matter. Not a hypothetical. A real document or workflow you actually need to do this week.

### Step 4.2 — Run the skill

In Cowork, point at the relevant folder or document. Invoke the skill. You can do this three ways:

- Slash command (`/triage-nda`)
- Skill name in plain text ("use the NDA triage skill")
- Plain language describing what you want ("there are five NDAs in the inbox folder, what should we do") — Claude will infer the right skill

Watch what happens. The skill should run end-to-end and produce its output (a triage report, a redline, a brief, whatever the skill produces).

### Step 4.3 — Read the output critically

Read it as if a junior associate handed it to you. What did it get right? What did it get wrong? What did it miss?

The output will not be perfect. That's expected. The point of this hour is identifying the gaps so you can fix the skill.

Common gaps on first run:

- Wrong tone (skill defaulted to a formal voice; your firm uses informal)
- Missing specific clauses you care about
- Output format doesn't match what you'd hand to a partner
- Citations are too vague (no page or paragraph references)
- Skill made model-only assertions without flagging them as such

### Step 4.4 — Edit the skill, run again

Make the edits. Save the skill. Run it again on the same document. Compare.

If the second run is closer to what you wanted, you've internalized the iteration loop. This is how you turn a starter skill into your skill.

### Step 4.5 — Iterate to "production quality"

"Production quality" means you'd hand the skill's output to a senior partner without editing further. Most skills get there in 3-5 iterations of run-edit-run on real documents.

Do as many iterations as you have time for in this hour. If you can't get to production quality in the hour, save where you are and come back tomorrow. The next iteration will go faster because you've learned the pattern.

By the end of Hour 4 you will have at least one production-quality skill running on real work. That's the milestone.

---

## After the four hours: month-by-month

### Week 1

Use the one skill you just built on real work, daily. Iterate as you find more gaps. By end of week, you'll have a stable, fully-customized version.

Share the skill with one teammate. Ask them to use it on their work. Get their feedback. Iterate.

### Month 1

Build 3-5 working skills covering most of your recurring work. The marginal cost of each new skill is much less than the first because you've learned the format.

Wire a second connector. By month-end, Claude has access to most of your daily work surfaces.

### Month 3

Personal library of 10-20 skills covering the bulk of your recurring work. Schedule 2-3 of them to run on a recurring basis (daily morning brief, weekly status synthesis, etc.). Pamela's regulatory tracker pattern.

By now, your daily work has shifted: you're chaining and reviewing more than drafting from scratch.

### Month 6

Routine work mostly automated. Lawyer reviews scheduled outputs each morning, makes judgment calls, intervenes on edge cases. Mark Pike's "manager not doer" framing.

Firm-level: someone is curating an org plugin marketplace. New hires get access to the firm's skill library on day one. The firm's institutional knowledge is portable.

---

## When this checklist breaks

It mostly doesn't. The failure modes:

- **IT delays installation.** You can do steps 1.1, 3.1, 3.2, 3.3 (and partially 4.1) on the web version of Claude.ai while waiting. Install Cowork the moment IT clears it; the rest of the work moves faster.
- **No connector available for your DMS.** Use the second-best connector. If your case management system isn't MCP-ready, use Drive or email as the proxy until the vendor ships a connector. The four-hour exercise still works.
- **Your real work is too sensitive for an early skill.** Pick a less sensitive workflow for the first iteration. Build confidence and validate the workflow on lower-stakes work. Migrate to the sensitive work once the pattern is proven.
- **You can't get to production quality in four hours.** That's fine. Sometimes the skill needs a second session. The first four hours teach you the iteration loop; the next four refine.

---

## When to use a legal-specific platform instead

If you read this checklist and think "I don't have four hours, and I don't have someone to curate this firm-wide later," you're a candidate for a legal-specific platform that handles the curation pre-built.

The honest tradeoff: less customization, less work. Most firms below 100 lawyers should make that tradeoff. Most firms above 200 lawyers should do the work themselves because they have the scale to amortize it. The middle is judgment.

Either way, the four-pillar architecture (`references/four-pillars.md`) and the four-layer privilege model (`references/privilege-layers.md`) apply. They are vendor-agnostic.

## How to use this reference

When the user says "I'm new to this" or "where do I start" or "what does setup actually look like":

1. Walk them through this checklist.
2. Calibrate the time estimate. If they have less than four uninterrupted hours, suggest splitting it across days but doing each step in one sitting.
3. Pick the skill they should remix in Hour 3 based on what they've actually been doing recently. Don't recommend a generic starting point.
4. Stay with them through Hour 4. The first iteration loop is where most users get stuck. The skill they end Hour 4 with is the proof point.
5. Surface the month-by-month progression. The four-hour milestone is the start, not the end.

## Source

The four-hour structure is HAQQ's standard onboarding cadence for legal teams adopting Claude, validated against ~9,800 firms. The "manager not doer" framing comes from Mark Pike's webinar.
