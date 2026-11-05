---
name: "canada-ca-ai-answer-check"
description: "Check whether people who ask ChatGPT, Copilot, Gemini or Claude about a Canada.ca page's topic are likely to get a correct answer from it, with plain fixes and a before/after test."
---

# Canada.ca AI answer check

This check answers one question: **if someone asks an AI tool about this topic and it answers
from our page, will they get the right answer?**

It breaks that into three plain questions, each answered **Yes**, **Partly** or **No**:

1. **Can AI tools find the page?**
2. **Is there a clear answer to copy?**
3. **Will they repeat it correctly?**

Recommend only changes that also help people reading the page and follow the Canada.ca Content
Style Guide. Every recommendation comes with a test the user can run in their AI citation monitor.
Research on what AI tools cite is still early, so treat each recommendation as a likely
improvement until the monitor confirms it.

Write everything in plain language for web advisors and managers. Don't use terms like
"retrieval", "GEO" or "RAG". Say "hidden FAQ data" or "structured data (hidden code that
describes the page to search engines)" rather than "JSON-LD", at least the first time.

---

## Inputs

- **Required:** a page URL, or the page snapshot from canada-ca-page-audit
- **Helpful:** the citation monitor's questions for this page and its latest results; site search
  terms; GC Feedback themes; the job-stories-writer task list

If no monitor questions exist for the page, draft 2–3 from the page's top tasks, written the way
a member of the public would ask ("How much is the Canada Disability Benefit?"), and label them
as proposed.

---

## Step 1: Capture (skip if the page audit snapshot exists)

Use a browser tool to capture the exact page: main text, headings, metadata, structured data,
date modified, and whether the main text is in the page's HTML (not loaded later by script).
Capture the French page too. Open the site's `robots.txt` and note any rules for AI or search
crawlers. Record exactly what the file says; don't guess what each crawler does.

---

## Step 2: Answer the three questions

### 1. Can AI tools find the page?

Look for:
- **Not blocked:** robots.txt and the page's own tags don't stop AI or search tools from reading it.
- **Text in the page:** the key facts are written in the page itself, not only in a PDF, an image,
  or content that loads later.
- **People's words:** the title, heading and intro use the words people actually ask with (from
  monitor questions or site search), not only the program's official name.
- **One main page:** the page points to itself as the main version, and no other GC page competes
  to answer the same question.

### 2. Is there a clear answer to copy?

Look for:
- **A direct answer near the top** for each top task: who qualifies, how much, when, how to apply.
- **Facts written out:** amounts, dates, ages and deadlines in plain text, with the period they
  apply to ("$204.20 a month, July 2026 to June 2027").
- **Current:** figures are for the current period, the date modified is recent for time-sensitive
  content, and nothing describes a past event as if it's still coming.
- **Complete:** the page, or the pages it links to in the same section, answers the questions
  people ask about this task.

### 3. Will they repeat it correctly?

Look for:
- **No contradictions:** the page, its section pages, its description, its hidden FAQ data and its
  French version all say the same thing. A contradiction is the most likely cause of a wrong AI
  answer.
- **Sections that make sense alone:** each section names what it's about in its first sentence,
  because AI tools often quote one passage, not the whole page. (Later mentions in the same section
  can still say "the program", as the Style Guide recommends.)
- **Conditions kept with the facts:** limits and exceptions sit in the same sentence or list as the
  fact they limit, not in a footnote or another section.
- **Accurate hidden data:** any structured data matches the visible page and is current. FAQ
  structured data only for questions and answers shown on the page.

---

## Answering Yes, Partly or No

For each of the three questions:
- **No:** something stops it outright, or a fact an AI tool is likely to repeat is wrong, out of
  date or contradicted (eligibility, amounts, dates, deadlines, contacts). Rate these findings
  Critical.
- **Partly:** a top task has no clear answer, key facts are only in a PDF or image, or a passage
  loses its meaning when quoted alone. Rate these findings Major.
- **Yes:** only small issues, or none. Rate these findings Minor.

Give one short reason with each answer. Don't give an overall percentage or score: the evidence
doesn't support that precision.

---

## Do not recommend

These conflict with GC neutrality, the Style Guide, or the evidence:
- invented or decorative statistics, quotations or testimonials
- promotional, boastful or "authoritative" tone
- repeating keywords or rotating synonyms for their own sake
- hidden text, or content written for AI tools but not for people
- untested files or tags (for example llms.txt) presented as effective
- blocking or unblocking AI crawlers: report what robots.txt says; that decision belongs to the
  site owner and policy, not this review

---

## Confidence

Tag each finding:
- **Seen:** confirmed in the captured page
- **Needs test:** needs a check in a browser or search console
- **Needs judgement:** a person needs to decide

---

## Step 3: Test plan

For the top 3 findings, give a test the user can run in the AI citation monitor:

| What we're fixing | Question to ask the AI tools | Before the fix, record | After the fix, it worked if |
| --- | --- | --- | --- |

- Run each question several times on each platform before the fix. Record which platforms cite
  the page and whether the answer matches the page.
- Re-run after the fix is published and the page has had a few weeks to be re-read by search and
  AI tools (say when the page was updated).
- Results vary between runs and platforms, so compare patterns, not single answers.

---

## Output

1. **The answer in one line:** "If someone asks an AI tool about [topic], they're [likely /
   partly likely / unlikely] to get the right answer from this page," plus the single most
   important fix.
2. **The three questions:** a small table: Question · Answer (Yes / Partly / No) · Why, in one
   sentence.
3. **What to fix:** a table: What an AI tool might tell people · How serious · Evidence from the
   page · Fix · Confidence. Most serious first. Cite the Style Guide section where it applies.
4. **Test plan** (Step 3).
5. **robots.txt in plain words:** one or two sentences on what the file allows or blocks for this
   page.

When called from canada-ca-page-audit, return this as Appendix F ("AI answer check") and send
Critical and Major findings to the audit's merge step.

## Rules

- Base every finding on the captured page and files. Never claim how a specific AI platform
  finds or ranks content unless the user's monitor results show it.
- Never invent page content, test results or effect sizes.