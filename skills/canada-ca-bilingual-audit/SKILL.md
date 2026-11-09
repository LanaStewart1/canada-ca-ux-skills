---
name: "canada-ca-bilingual-audit"
description: "Audit English and French Canada.ca page pairs for parity, link language, accessibility basics, outdated content and French clarity. Use for bilingual or EN/FR checks, alone or from canada-ca-page-audit."
---

# Canada.ca bilingual audit

Audit one or more English/French Canada.ca page pairs and produce a neutral, actionable report
for Government of Canada web teams. Adapted from Chelsey Donohue's Copilot skill.

Write in Canadian English unless the user writes in French. Start directly with the report.

## Modes

- **Standalone:** the user gives URLs or HTML. Run all five checks and write the full report.
- **Called from canada-ca-page-audit:** use the audit's snapshot (`mainHtml` for English and
  French, date modified, link-follow data). Don't re-fetch. Skip the page-code check (the
  accessibility skill covers WCAG) and return findings on the audit's shared severity scale.
- **Section mode** (a set of pages, alone or from a page audit section audit): first compare
  the structure of every English and French pair: status, H2 and H3 counts, tables, images,
  links, PDF links, the language toggle in both directions, and whether every anchor in each
  table of contents exists on its destination. Then compare the titles of every pair, and
  look into any pair whose counts differ to find what's different. Run the five checks in
  full only on the entry page and the 2 pages with the most feedback (or, without feedback,
  the 2 most visited). Report the structure comparison as one line per difference, not one
  row per page, and say which pages got the full check.

## Step 1: Get the exact HTML

Base every finding on the exact page markup, never on a summary or on memory.

1. **Browser available (preferred):** open each page in the built-in browser (load its skill
   first) and capture `document.querySelector('main').outerHTML`, the H1, `<html lang>` and the
   date modified. Find the French page from the language toggle. No copying needed.
2. **No browser:** ask the user to paste the HTML, with this template:
   `=== PAIR 1: page name ===` / `--- EN ---` / `[paste English HTML]` / `--- FR ---` /
   `[paste French HTML]`. Copy instructions: open the page, press F12, go to Elements, press
   Ctrl+F and search `<main`, right-click that line, then choose Copy > Copy element.
3. **Pairing:** if pages aren't labelled, pair them by H1, URL and language-toggle link, and say
   how. A pair with only one language isn't audited; name the missing language and continue.
4. **Partial HTML** (cut off, no closing element, ends mid-tag): label the pair `Partial`, say
   what's missing, and audit only what's present.
5. **Batches:** at most five pairs per response. If more remain, end with:
   `Pairs X–Y remain. Reply 'continue'.`

Don't open the links on the page or search the web to judge content. Links that need a look go
under Links to verify.

## Step 2: Run the five checks on each complete pair

1. **Same facts (parity):** compare sections in order. Every heading, paragraph, list item,
   table row, note and call to action has an equivalent. Facts match: amounts, percentages,
   dates, deadlines, eligibility, contacts, addresses, form numbers and **program names** (the
   same program must have the same name everywhere on each page). Flag meaning changes that add,
   omit, soften or alter a condition. Compare the date modified values. Check link
   descriptions and card text too, not just body text.
2. **Right-language links:** destinations match the page language (`/en/`, `-eng`, `lang=eng`
   versus `/fr/`, `-fra`, `lang=fra`); equivalent links match across languages; flag vague link
   text ("click here", "here", "read more", "learn more", "cliquez ici", "ici", "en savoir
   plus", "lire la suite"); flag identical link text pointing to different URLs; file links
   state format and size; each `#anchor` has a matching `id`; flag `target="_blank"` with no
   "opens in a new window" warning. Never call a link broken: external links and links that look
   old (`/eng/`, `.asp`, archived paths) go under Links to verify.
3. **Page code basics** (standalone mode only): other-language passages have their own `lang`;
   `<html lang>` matches the page; ordered heading levels and no empty headings; one H1, or two
   on a page of a Canada.ca multi-page set (the program name above the section title), which is
   accepted; images have purpose-appropriate alt text (`alt=""` only for decorative images;
   flag file-name alt text); data tables have headers and captions; form fields have labels and
   iframes have titles; no instruction relies only on colour, shape or position. For a full
   WCAG check, use canada-ca-accessibility.
4. **Out-of-date content:** flag past dates or years next to deadline, apply by, until, new,
   upcoming, coming soon, date limite, jusqu'au, nouveau or à venir; flag a date modified older
   than 12 months on a time-sensitive page; flag programs, forms, rates or contacts that appear
   superseded, without asserting it unless the HTML proves it. Include structured data (JSON-LD)
   when it's in the source, since search and AI tools read it.
5. **Clear French (and English):** steps with no clear action, missing what-happens-next
   information, undefined jargon or acronyms, buried conditions, contradictions; a main task or
   call to action that isn't near the top; prose that would work better as a list, table or
   steps; plain language, Canada.ca style, spelling and grammar in both languages.

## Step 3: Severity and confidence

Use the suite's shared scale:

| Severity | Meaning here |
|---|---|
| Critical | English and French differ on eligibility, a deadline, an amount, a contact or a legal condition; or a problem that could cause a wrong application or block a task |
| Major | Any other factual difference between English and French (including a different program name); likely to confuse or slow the task; a WCAG failure that doesn't block |
| Minor | Polish, style, minor wording or consistency |

Confidence: **[Seen]** clearly in the markup; **[Needs test]** only a browser or hands-on
check can confirm; **[Needs judgement]** a person must decide (for example, whether a
translation changes the meaning).

## Step 4: Report

**Two or more pairs:** start with a **Batch summary** table (Pair · Page (EN H1) ·
Critical · Major · Minor · Most serious issue), then **Patterns** (issues found in two or
more pairs).

**For each pair** (`Pair N: [EN H1]`, or no pair heading for a single pair):

1. **In one line:** the verdict for the pair, in plain words.
2. **Source check:** EN H1 "…" · FR H1 "…" · Date modified EN … / FR …, quoted from the
   HTML. If missing, say the HTML is incomplete.
3. **What this check looked at:** a plain-language table, never letters or codes:

   | Check | What it means | Result |
   | --- | --- | --- |
   | Same facts | English and French say the same thing (names, dates, amounts, conditions) | Issues found / No issues / Not checked |
   | Right-language links | Links on the French page go to French pages, and the reverse | … |
   | Page code basics | Headings, image text and language settings are coded correctly | … |
   | Out-of-date content | Dates or statements that are no longer true | … |
   | Clear French | The French reads naturally and follows French style rules | … |

   Use plain words for results (no emoji). "Not checked" says why (for example, "covered by
   the accessibility check" in page-audit mode, or "French HTML not provided"). Never mark a
   check as passed when it couldn't run. Under the table, one line: colour contrast, keyboard
   use and screen readers can't be judged from the page code.
4. **Findings** table: # · Finding and impact · Severity · Language (EN, FR or EN+FR) ·
   Check · Where and evidence · Fix · Confidence. Sort Critical to Minor. Don't list the same
   issue twice; group repeats (for example, several missing alt attributes). Cap at 25 rows a
   pair.
   - **Where and evidence:** say where it sits on the page in words a reader can follow ("the
     description under the 'Après avoir présenté votre demande' link") and quote the exact
     text, up to 25 words. For parity issues, quote both languages.
   - **Fix:** exact replacement text or markup where possible.
5. **Links to verify:** up to 15 URLs to check in a browser, or None.
6. **What's working:** one line, optional.

## Step 5: Quality check before returning

- Every complete pair got all its checks; no pair was silently skipped.
- Every finding is supported by the captured or supplied HTML, with no invented text, URL or
  issue.
- Every finding says where it is on the page and quotes the text, so a reader can find it.
- Uncertain external status is phrased as something to verify, never as a broken link or a
  confirmed supersession.
- The report is neutral, specific, actionable, correctly sorted and within the limits.