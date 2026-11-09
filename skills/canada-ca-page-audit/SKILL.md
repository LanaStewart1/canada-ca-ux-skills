---
name: "canada-ca-page-audit"
description: "Run a full UX audit of a Canada.ca or GC page, or a section of related pages, by chaining the UX, content, SEO, accessibility, feedback and job-story skills into one merged report. Use for \"audit this page\", \"audit this section\" or \"full UX audit\"."
---

# Canada.ca page audit

This skill runs the other Canada.ca skills in a fixed order, on one shared copy of the page,
and merges their results into a single report. It does not replace those skills: load each one
with the Skill tool when its step comes, and follow its rules, except where this skill sets a
shared rule (snapshot, severity scale, merge).

Inputs:
- **Required:** a page URL, or pasted HTML for the English and French pages (HTML-only mode)
- **Optional:** a GC Feedback export, an earlier feedback analysis, an Adobe Analytics export,
  site search terms, call-centre data, AI citation monitor questions and results

**Section mode.** Use it when the user asks for a section, a report spread over several pages
(a budget, an annual report, a guide in chapters), or when the feedback lands on several pages
of one set. If it's unclear, ask once: one page in depth, or the section as a whole.
- **Scope:** list every page in the set from its table of contents or landing page, in English
  and French. If the address given returns Not Found, find the set's real entry page, say so,
  and report the bad address as a finding.
- **Snapshot:** capture the entry page in full (Step 1). For every other page, fetch the exact
  HTML in the browser and record: status, title, description, H1, H2 and H3 counts, word count,
  tables, images without an alt attribute, PDF links, date modified, language toggle, canonical,
  robots and snippet tags, and structured data. Check every anchor link in both tables of
  contents against the destination's ids.
- **Feedback:** code every comment, and add a topic column for the set's subject (for example
  retirement savings, seniors, tax measures, report format). For the biggest topics, search the
  pages' text to find where each one sits, so the report can say whether people can't find
  something that is there, or are asking for something that isn't.
- **Reviews:** judge the set as a whole: navigation between pages, whether the table of contents
  reaches the level where people's topics sit, the PDF and search, and the same metadata and
  accessibility checks across all pages. Run the axe scan on the entry page in both languages
  and on 2 content pages (the most-commented and the one with the most images).
- **Charts:** in place of the one-page sentence chart, chart the share of sentences over 20
  words and the reading time for each page. Add a feedback topics chart, and a journey map from
  the entry page to where the top topic sits.
- **Report:** title it "<set name>: section audit". The scope line gives the number of pages in
  each language. Every finding names the pages it applies to.

---

## Step 1: Page snapshot

All later steps work from this snapshot. Do not let each skill re-fetch the page.

1. **Get the exact page, not a summary.** Use a browser tool if one is available (prefer the
   built-in browser; load its skill first). Open the page and run this in the page to capture
   facts a summary misses:

   ```js
   (() => {
     const q = s => [...document.querySelectorAll(s)];
     const main = document.querySelector('main');
     return JSON.stringify({
       title: document.title,
       docLang: document.documentElement.lang,
       canonical: (document.querySelector('link[rel="canonical"]') || {}).href || null,
       robotsMeta: (document.querySelector('meta[name="robots"]') || {}).content || null,
       meta: Object.fromEntries(q('meta[name]').map(m => [m.name, m.content])),
       jsonld: q('script[type="application/ld+json"]').map(s => s.textContent.slice(0, 2000)),
       h1Count: q('main h1').length,
       headings: q('main h1, main h2, main h3, main h4').map(h => h.tagName + ' ' + h.textContent.trim()),
       ids: q('main [id]').map(e => e.id),
       links: q('main a[href]').map(a => ({ text: a.textContent.trim(), href: a.href, blank: a.target === '_blank' })),
       images: q('main img').map(i => ({ src: i.src.split('/').pop(), alt: i.getAttribute('alt') })),
       feedbackWidget: !!document.querySelector('.gc-pft, #gc-pft, [data-gc-pft], section.pagedetails form'),
       langToggle: (document.querySelector('#wb-lng a, .lang-toggle a') || {}).href || null,
       dateModified: (document.querySelector('time[property="dateModified"]') || {}).textContent || (document.querySelector('meta[name="dcterms.modified"]') || {}).content || null,
       text: main ? main.innerText : '',
       mainHtml: main ? main.outerHTML : ''
     });
   })()
   ```

   The feedback widget loads late: if it isn't found, wait about 2 seconds and check again before
   reporting it missing. If no browser tool is available, ask for pasted HTML (HTML-only mode; give
   the F12 copy instructions from canada-ca-bilingual-audit), or fall back to the web fetch tool
   and ask it for verbatim text. Anything you could not see in exact source is tagged
   [Needs test] in every later step.
2. **Follow the primary calls to action one level.** Capture the 3–6 main buttons, cards or
   task links the same way. For each, record: destination H1, whether any #anchor in the link
   exists in the destination's ids, whether the user leaves the site or department, and the date
   modified. (Skip in HTML-only mode and say so in the summary.)
3. **Capture the French page** from the language toggle, the same way, including `mainHtml`.
4. **Open the site's robots.txt** and keep its text for the AI tools check.
5. **Look for a previous audit** of the same URL: search past chats and the user's artifacts for
   the URL or page name. If one exists, note its date, grade and findings for Step 5.

Keep the snapshot in the working directory (for example `snapshot-en.json`, `snapshot-fr.json`)
so steps can reuse it.

---

## Step 2: Evidence (only if provided)

- **Feedback file attached:** run canada-ca-feedback-analyst on it, in measured mode (each
  comment labelled by ID, counted with code, coding table kept).
- **No file, but an earlier analysis exists** (in the conversation or past chats): use its themes
  and say its date. Use its exact counts only if it was measured (a coding table exists, or the
  analysis says it labelled each comment by ID and counted with code). Otherwise show its numbers
  as approximate shares ("about 1 in 6 comments") and label them "approximate, from a single-pass
  analysis". Do not present old counts as new.
- **Analytics, search terms, call data, citation monitor results attached:** read them and keep
  the numbers that bear on the page's tasks.
- **Nothing provided:** say so in the report's scope line and continue.

Everywhere a comment count appears in the report, say whether it is measured or approximate and
whether it is validated against a human-coded sample.

---

## Step 3: User tasks

Run job-stories-writer in **audit mode** (3–6 tasks, each with a test scenario and a verified
answer from the snapshot). If the evidence step produced themes, ground the tasks in them and
show each task's comment count or approximate share, labelled as in Step 2.

These tasks are the yardstick for every later step: each finding must say which task it
affects, or that it affects none.

---

## Step 4: Reviews

Run each against the snapshot and the task list:

1. **ux-reviewer**: the full review, including the journey map, using the snapshot's
   link-follow data instead of re-fetching. Leave WCAG checks to step 6 (canada-ca-accessibility).
2. **canada-ca-writer**: assessment only (no rewrite unless asked), English text. Report its
   measurable checks (sentence, paragraph and list lengths); a reading grade is context only.
   Count the words in every sentence of the main page's body text with a script (headings
   excluded), in page order, and give each sentence a short label ("Intro", "Back payments",
   "Apply link") for the sentence-length chart.
3. **canada-ca-seo**: review title, description, keywords and structured data. Check that the
   description names the top 1–2 tasks. If site search terms were provided, draw keywords from them.
4. **canada-ca-bilingual-audit**: run on the English and French `mainHtml` from the snapshot
   ("called from canada-ca-page-audit" mode). This covers bilingual parity, link language and
   anchors, outdated content, and French clarity and style. WCAG findings come from step 6;
   don't report them twice. Quote the exact French text and say where it sits on the page
   (for example, "the description under the 'Après avoir présenté votre demande' link"), so a
   reader can find it.
5. **canada-ca-ai-answer-check**: run on the snapshot, robots.txt and the task list. Use the
   user's citation monitor questions if provided; otherwise draft 2–3 and label them proposed.
   Don't repeat findings the SEO or bilingual steps already made; reference them instead.
6. **canada-ca-accessibility**: "called from canada-ca-page-audit" mode, on the live English
   and French pages in the built-in browser (axe scan plus the code and contrast script), using
   the task list to decide what sits on a task path. Keep its template issues (GCWeb header,
   footer, breadcrumb, feedback widget) separate from page issues; they never set the grade
   cap. If no browser is available, it can't run: the Accessibility check is grey ("not
   checked") and the summary says so.

---

## Shared severity and confidence scale

Every finding from every skill is restated on this one scale before merging, whatever scale the
skill used on its own:

| Level | Meaning |
| --- | --- |
| Critical | A top task likely fails, some users are excluded, English and French differ on eligibility, a deadline, an amount, a contact or a legal condition, or the page is blocked from AI or search tools |
| Major | A top task has significant friction, confusion or errors; a non-blocking WCAG failure; any other EN/FR factual difference |
| Minor | Suboptimal, unlikely to cause failure |

High / Medium / Low from other tools map to Critical / Major / Minor.

And one confidence tag:
- **[Seen]**: confirmed in the snapshot's exact source
- **[Needs test]**: not confirmable from the snapshot; goes in the "To verify in browser" list
- **[Needs judgement]**: a person must decide (for example, a possible meaning change in
  translation); report it as a finding, but it can't set the grade cap on its own

[Needs test] items never go in the top issues or the grade. Never call a link broken: unconfirmed
links go under "Links to verify".

---

## Step 5: Merge and grade

1. **Merge duplicates.** The same underlying problem found by several skills is one issue.
   List every skill that found it. Keep the highest severity.
2. **Rank** by severity, then by how many users the affected task covers (use measured comment
   counts or analytics when available; approximate shares can break ties but are labelled).
3. **Grade** with the ux-reviewer caps: any Critical → C+ or lower; two or more → D or lower;
   three or more Major with no Critical → B- or lower; A requires no Critical or Major.
4. **Compare with the previous audit** if one was found: mark each top issue as New, Still open
   or Fixed. A Critical issue still open since the last audit lowers the grade one step below its
   cap. Report the comparison only in Follow-up checks ("Since the October audit: 4 still open,
   1 new, 0 fixed"), never in the summary or the fixes table.

---

## Step 6: Report

Make the report a shareable doc (use the Docs artifact type when available) unless the user
asked for a different format. Structure:

**Wording:** call the AI step "AI tools check" in the report, never "AI Answer". Say "AI tools" in labels, headings and table cells. Don't call the
review sections appendices or letter them; refer to a section by its name ("See UX review").
Avoid grading jargon such as "capped by"; give the counts instead.

The report is one page, in this order, with a divider line (`---`) above each main section
from Summary to Sources:

1. **At-a-glance panel** (right under the title and byline), drawn as one diagram widget where
   the doc supports it, otherwise a short table. Use colour tokens only, and look at a screenshot
   before moving on. The panel has no title line of its own: the finding box says it once.
   Don't repeat the panel's contents in the prose below it.
   - **Grade box:** "Overall grade", the grade large, and the issue counts under it ("5 Major
     issues, 0 Critical"). The box is tinted and outlined by grade band: green for A, amber for
     B, red (critical) for C, D or F.
   - **The finding:** the single thing a manager should leave with, in a plain box. A short
     bold first line that a non-specialist gets in one read (who, what, how bad, with a number:
     "The 3 things people want most are 2 or 3 clicks away:"), then one plain line naming
     them. No jargon, no heuristics or check names, under about 20 words in all.
   - **Checks box:** one quiet box holding a row of coloured dots with names (not pills or
     chips, which read as clickable): Tasks · UX · Content · SEO · French · Links ·
     Accessibility · Outdated · AI tools. Under a thin rule inside the same box, the legend:
     amber = issues found, green = no issues (and grey = not checked, when any check is grey).
     Never green a check that couldn't run; Accessibility is green only when the scan and
     script both ran and found nothing. A check's colour must match what its section says.
2. **Contents line**, under the panel: "**Contents:** Summary · User tasks · UX review ·
   Content review · Metadata and findability · Bilingual audit · AI tools check ·
   Accessibility · Follow-up checks · Sources". Plain text: a one-page doc can't link to its
   own headings.
3. **Summary** (no wording about previous audits here):
   - grade line: the grade, the issue counts, the page's last-modified date
   - scope in one line ("Captured Nov 4, 2026 · English and French · no feedback or
     analytics data"); how and when each page was captured goes in Sources
   - **Top 5 fixes** as a table: # · Fix · Why · Severity · Effort (estimate) · Found by.
     Severity is a colour-coded dropdown chip (Critical, Major, Minor) where the doc supports
     it. Effort is Small, Medium or Large, also a chip, with a note under the table that effort
     is an estimate for the page owner to confirm, and why any Large one is Large (for example,
     it depends on the service, not the page). Write Fix as an action ("Split the
     back-payment sentence in two"). Make Why specific enough to find the problem on the page:
     quote the text and say where it sits.
   - what's working
   - one priority next step
4. **User tasks**: job stories, scenarios with answers, user need statements.
5. **UX review**, with the journey map drawn as a diagram where the doc supports it.
6. **Content review** (English), with the sentence-length chart (see Charts) under the
   measurable checks.
7. **Metadata and findability**.
8. **Bilingual audit**: source check, findings table, links to verify.
9. **AI tools check**: the three questions, what to fix, citation monitor test plan,
   robots.txt in plain words.
10. **Accessibility**: findings table with WCAG success criteria, template issues for the
    theme team, what couldn't be tested, the hands-on test script.
11. **Follow-up checks**: one short intro line, then:
    - since the previous audit (only if one exists): open, new and fixed counts, and a link to it
    - evidence to check (Adobe, GC Feedback, site search, call data, citation monitor: what to
      look at and what result confirms the issue)
    - "To verify in browser" (all [Needs test] items, including the accessibility hands-on steps)
    - "Links to verify"
12. **Sources**: every page opened, with how and when it was captured. If a feedback analysis
    was run, attach its coding table.

**Every review section (4 to 10) has the same shape:**
- the heading is the section name only ("UX review"); the skill that produced it goes on the
  next line in small grey text ("From ux-reviewer")
- the first line is a bold **In one line:** verdict, so a reader can skim headings and first
  lines
- findings go in a table: # · Finding · Severity · Confidence · Task · Fix, with the same
  severity chips as the Top 5 fixes (no emoji markers). Each finding still names the task it
  affects, or "none".

Keep the panel and summary to what a manager reads in two minutes. The review sections hold
each skill's full output; follow-up lists go at the end so they don't interrupt the findings.

### Charts

Use the doc's own chart tools (not Canada.ca chart code). Follow these rules, which match the
canada-ca data viz skill's accessibility gate:

- **Chart only five or more values.** Fewer stay a sentence or a table.
- **The title states the finding with its number**, not just the topic ("Three of twelve
  sentences run over 20 words, all in the opening text").
- **Grey first:** data in one quiet grey, with one accent colour on what the title is about.
  Colour is never the only signal: highlighted items are also bold or labelled.
- **Show the standard on the chart** as a labelled reference line (for example the 20-word
  sentence limit).
- **Values on the bars**, no extra axis or gridlines. Bars start at zero.
- **Numbers once:** don't repeat the plotted values in a table or list below the chart.
- **A caption says where the numbers come from** and when they were measured.
- **Look at the rendered chart** before moving on, and fix any clipped or overlapping labels.

The standard charts:

1. **Sentence length (always, Content review):** one horizontal bar per sentence of the main page's
   body text, in page order, labelled with a short name for each sentence. A dashed reference line
   at 20 words (Style Guide 2.6). Sentences over 20 words in the accent colour and bold; the rest
   grey. One sentence under the chart saying where the long sentences sit and why it matters.
2. **Feedback themes (only with measured counts, Step 2):** themes ranked by comment count. Never
   chart approximate or unvalidated counts as if they were exact; give approximate shares in text
   instead.
3. **Before and after (only when there is a previous audit or citation monitor results):** issues
   open and fixed, or how often each AI tool cites the page, before and after a fix.

---

## Rules

- Never invent page content, counts or answers. Every test-scenario answer must appear in the
  snapshot.
- Never present a comment count as exact unless it was measured.
- If a step can't run (no browser, page down, skill missing), grey its check in the panel, say
  why in the summary, and continue with the rest.
- Plain language throughout; the reader may not be a UX specialist.