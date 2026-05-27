---
name: referral-outreach
description: Use when drafting a referral request message for a specific job — reads the user's knowledge base (wenjun_wu_refreshed.md), parses the job description, picks the 2-3 strongest matching points, and generates a short personalized message (3-5 sentences) the user can send to a referrer or hiring contact
---

# Referral Outreach Skill

## Overview

Generates concise, personalized referral request messages for specific job applications. Reads the user's master knowledge base, extracts the 2-3 most relevant experience points for the target role, and drafts a message the user can send to a referrer (colleague, mutual connection, or hiring manager).

**Core principle:** Short beats long. A referral message is not a cover letter. It should take the reader 20 seconds to read and leave them with one clear ask.

## When to Use

- User has a job they want to apply to and knows someone (or has a warm intro to someone) at the company
- User wants to ask a former colleague or contact to refer them
- User wants a short intro message to send alongside their resume

**DO NOT use for:**
- Full cover letters (different skill)
- Cold LinkedIn outreach to strangers with no connection (use cover letter skill instead)
- Resume tailoring (use resume-tailoring skill)

## Quick Start

**Required from user:**
1. Job — any of:
   - A job posting URL (any ATS: Lever, Greenhouse, Workday, iCIMS, LinkedIn, company careers page, etc.)
   - Pasted job description text
   - Company name + role title (minimum — skill will web-search for context)
2. Referrer's name and relationship (e.g., "former colleague", "met at NeurIPS", "mutual friend introduced us")

**Optional:**
- Any specific angle to emphasize (e.g., "stress my medical imaging background")
- Preferred tone (warm/casual vs. professional/formal)
- Channel (LinkedIn message / email / Slack) — skill will ask if not provided

## Workflow

### Phase 0: Resolve the Job Input

**If the user provides a URL:**
1. Use WebFetch to retrieve the page content
2. Strip HTML tags and boilerplate navigation; extract the meaningful job description body
3. If the page requires JavaScript to render and WebFetch returns minimal content, fall back to WebSearch with `"[company] [role title] job description"` to find a cached or mirror version
4. Confirm the role title and company extracted: *"Got it — [Role] at [Company]. Proceeding."*

**If the user pastes text:** use as-is.

**If only company + role name:** proceed to Phase 2 and infer requirements from the title; optionally run a quick WebSearch for the live posting.

**ATS URL patterns and fetch notes:**

| ATS | URL pattern | Notes |
|---|---|---|
| Lever | `jobs.lever.co/{company}/{id}` | Full JD in HTML body |
| Greenhouse | `boards.greenhouse.io/{company}/jobs/{id}` | Full JD in HTML body |
| Workday | `{company}.wd5.myworkdayjobs.com/...` | JS-rendered; WebFetch may be partial — try WebSearch fallback |
| iCIMS | `careers-{company}.icims.com/...` | Full JD usually in HTML |
| LinkedIn | `linkedin.com/jobs/view/{id}` | Often truncated; try WebSearch for company careers page version |
| Company careers page | varies | Fetch directly; most render in HTML |

### Phase 1: Load Knowledge Base

**Discovery order** (stop at first match):
1. `resumes/knowledge_base.md` — canonical name; recommended default
2. Any single `*_refreshed.md` file in `resumes/` — legacy naming convention
3. Any single `.md` file in `resumes/` that is clearly a master CV (contains multiple roles + education)
4. If none found or multiple candidates exist, ask the user: *"Where is your knowledge base? (path to markdown file)"*

Once located, parse into structured profile:
   - Current role + company
   - Core expertise areas
   - Key projects with metrics
   - Domain strengths (medical imaging, generative AI, ranking, etc.)
   - Publications (if relevant to role)

### Phase 2: Parse the Job

Extract from the job description (fetched or provided):
- Company name + team (if mentioned)
- Role title
- Top 3 requirements (what they care most about)
- Domain focus (ML research, applied ML, industry research lab, etc.)
- Any specific tech stack or methods mentioned

If only a company + role name is given (no JD text retrieved), infer requirements from the role title and optionally do a brief web search.

### Phase 3: Match and Select Talking Points

Score the user's experience against the JD requirements. Select the **2-3 strongest matching points** — prioritize:
1. Direct matches (same domain, same method, same scale)
2. Quantified achievements (numbers > adjectives)
3. Recent over old

Avoid:
- Generic claims ("strong ML background")
- Listing everything — pick the sharpest 2-3
- Anything the referrer can't verify from a resume

### Phase 4: Determine Tone and Format

**Tone variants:**

| Relationship | Tone |
|---|---|
| Close colleague / friend | Warm, first-name, casual opener |
| Warm intro (mutual connection) | Professional but personal, reference the mutual |
| Former manager / mentor | Respectful, brief on background (they already know) |
| Someone met at conference/event | Reference the meeting, then pivot to ask |

**Format variants:**

| Channel | Format |
|---|---|
| LinkedIn message | No subject line, ~100 words max |
| Email | Subject line + body, up to 150 words |
| Slack / text | Even shorter, 2-3 sentences |

### Phase 5: Draft the Message

**Structure (3-5 sentences):**

1. **Opener** — establish context / relationship (1 sentence)
2. **Who you are + what you're applying for** (1 sentence)
3. **Why you're a strong fit** — 1-2 specific points from knowledge base (1-2 sentences)
4. **The ask** — clear and easy to say yes to (1 sentence)

**Templates by tone:**

*Warm colleague:*
```
Hi [Name], hope you're doing well!

I'm applying for the [Role] position on [Team] at [Company] and noticed
you're there — would you be open to referring me?

I've been a Research Scientist at Meta for the past two years working on
[specific point 1] and [specific point 2], which maps closely to what
the team is doing. Happy to share my resume — let me know if this works!
```

*Warm intro (mutual connection):*
```
Hi [Name], [Mutual] suggested I reach out — I'm applying for the [Role]
at [Company] and they thought there might be a good fit.

My background is in [specific match 1]; most recently at Meta I [specific
achievement]. I'd love to be referred if you think it's a good match —
no pressure either way. Happy to send my resume or hop on a quick call.
```

*Met at conference:*
```
Hi [Name], great meeting you at [Event] last [month/year]!

I'm reaching out because I'm applying for [Role] at [Company] — given
our conversation about [topic], I thought there might be a good fit.

[1-2 sentence pitch from knowledge base.] Would you be open to referring
me, or pointing me to the right person on the team?
```

### Phase 6: Present and Refine

Present the draft with:
- The message itself (copy-paste ready)
- A one-line explanation of each talking point chosen and why
- Subject line (if email)
- Word count

Ask:
```
Does this capture the right angle? I can:
- Adjust tone (more/less formal)
- Swap in a different talking point
- Shorten or lengthen
- Add/remove subject line
```

Make any requested adjustments and re-present.

## Output Format

```
REFERRAL MESSAGE
────────────────
Subject: [Subject line — omit if LinkedIn/Slack]

[Message body]

────────────────
Word count: [N]
Talking points used:
  • [Point 1] — matches [JD requirement]
  • [Point 2] — matches [JD requirement]
```

## Key Constraints

- **Never fabricate.** Only use experience that exists in the knowledge base.
- **Never oversell.** A referrer who forwards your resume is sticking their neck out — don't make them look bad.
- **Keep the ask easy.** "Would you be open to referring me?" is easier to say yes to than "Can you put in a strong word with the hiring manager?"
- **One ask per message.** Don't ask for a referral AND an informational interview AND resume feedback in the same message.

## Example

**Input:**
- Job: Research Scientist, Medical AI at Stanford HAI
- Referrer: "Former PhD classmate, now a postdoc there"
- No specific angle requested

**Knowledge base:** `resumes/knowledge_base.md` (auto-discovered)

**Top matches:**
- PhD in Biomedical and Health Informatics (UW) — direct domain match
- 5 publications in medical image analysis (MICCAI, Medical Image Analysis, IEEE)
- Current work: deploying ML models at scale at Meta

**Draft:**
```
Subject: Referral request — Research Scientist, Medical AI at Stanford HAI

Hi [Name], hope the postdoc is going well!

I'm applying for the Research Scientist role on the Medical AI team at
Stanford HAI and thought of you. My PhD was in Biomedical and Health
Informatics at UW with a focus on medical image analysis (5 papers in
MICCAI / Medical Image Analysis), and I've spent the past two years at
Meta deploying ML models at scale — feels like a natural next step.

Would you be open to referring me? Happy to send my resume!
```
