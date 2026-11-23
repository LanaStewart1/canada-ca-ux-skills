# Changelog

## 3.1 (November 23, 2026)

- **All skills:** the confidence tag [Seen] is now [Confirmed], so it reads plainly in reports.
  The other tags ([Needs test], [Needs judgement]) are unchanged. Updated in page audit, UX
  reviewer, writer, accessibility, bilingual audit, AI answer check and service experience.
- **canada-ca-page-audit:** the merge step is now where recommendations come together. It
  merges duplicate fixes found by several skills, resolves conflicts between them (for example
  SEO, plain language and accessibility on the same title), ranks by severity, reach and effort,
  and tags each fix as a page fix or a service fix. The Top 5 fixes table gets an Owner column.
  The feedback analyst stays evidence only.
- **canada-ca-service-experience:** treats the sign-in page as an evidence wall. It names the
  account the service uses (My Service Canada Account, CRA My Account and others), marks stages
  that happen inside it as "Inside the account: not seen" instead of guessing, flags "Account
  mentioned" comments, and uses sign-out feedback, call reasons, analytics or testing when
  available. A new Inside the account section lists which sources exist and which are missing;
  no evidence at all becomes a service gap.

## 3.0 (November 23, 2026)

- **canada-ca-service-experience:** new skill. Follows each task from the Canada.ca entry page
  to where it actually starts, and scores six stages named after the codebook tasks: Learn,
  Sign in, Apply, Update, Check status and Contact. The report opens with a service journey
  diagram showing where the friction is, then each stage with "Can people do it?" and "Action
  needed", what GC Feedback tells us (with a note that comments come only from people who
  answered No), clicks and hand-offs to start each task, and service gaps to fix, each tagged
  as a page fix or a service fix. Tested on Passport and the Canada Disability Benefit.
- **canada-ca-page-audit:** runs the service experience check when the page is a service
  journey, with a Service card in the audit panel and a Service experience section. Reports
  and budgets get a grey "not applicable".

## 2.0 (November 9, 2026)

- **canada-ca-page-audit:** new section mode, to audit a report or set of related pages as a
  whole (snapshot of every page in both languages, feedback topics by page, a reading-time and
  sentence-length chart per page, and a journey map to where the top topic sits). The
  at-a-glance panel has no title line, a plain bold finding with a number, and no AI tools cards.
- **ux-reviewer:** asks the stage first (sketch, prototype or live page) and adjusts the review;
  adds a two-second first glance (does the eye land on the top task?); no grade for early
  designs.
- **canada-ca-ai-answer-check:** new "Snippets allowed" check; Search Console's Generative AI
  performance report added to the test plan; llms.txt, AI-only markup and splitting pages for AI
  are on the "do not recommend" list, in line with Google's AI optimization guide.
- **canada-ca-seo:** checks indexing and snippet controls; new section mode with one table of
  proposed descriptions and keywords for every page.
- **canada-ca-bilingual-audit:** new section mode that compares the structure of every English
  and French pair, then checks the entry page and the most-commented pages in full.
- **canada-ca-feedback-analyst:** v1.12 (codebook v1.8): on report pages a one-word topic is
  coded Learn; a topic column and topic-by-page table for sections.
- **README:** the suite audits a single page or a section.

## 1.2 (November 6, 2026)

- **canada-ca-feedback-analyst:** codebook v1.7, after blind validation on passport renewal
  (Apply and Can't find are now Trusted on that page type). New rules: "how do I" vs "can I"
  for Apply and Learn; a comment that only states a task or asks a question is coded "Can't
  find"; "Get a document or form"; information about an office is Learn; single words stay
  Unknown; Technical error vs Unable to complete online. Adds a channel churn flag, and reports
  "No usable content" exclusions as a share.
- **New post:** [Counting feedback you can trust](docs/counting-feedback-you-can-trust.md).

## 1.1 (November 5, 2026)

- **canada-ca-feedback-analyst:** new Codebook mode that codes every comment against a general
  GC Feedback taxonomy (Task × Issue type) that works on any service, tested on Express Entry,
  the Canada child benefit, passport renewal and PHAC vaccine pages. It includes "Looking for
  something this service doesn't offer" with a jurisdiction test, theme maturity statuses
  following validation-first thematic measurement, redirect flags, and coding in any language.
  Personal health details are now excluded, and comments suggesting risk to someone are flagged
  separately, without quoting them.

## 1.0 (November 5, 2026)

First shared release of the suite.

- **Page audit report layout:** at-a-glance panel (grade box coloured by grade, AI tools cards,
  checks with a legend), a contents line, divider lines between sections, and a Top 5 fixes
  table with severity and effort chips. Review sections are named, not lettered, and each opens
  with a one-line verdict.
- **New skill: canada-ca-accessibility.** Automated axe-core scan, measured contrast and code
  checks, findings mapped to WCAG 2.1 AA success criteria, template issues kept separate, and a
  hands-on test script.
- **canada-ca-bilingual-audit:** adapted from Chelsey Donohue's Copilot skill. Captures pages in the
  built-in browser, uses a plain-language results table, says where each finding sits on the
  page, and checks program names and structured data.
- **canada-ca-ai-answer-check:** plain-language version with three questions answered Yes,
  Partly or No.
- **canada-ca-feedback-analyst:** measured counting (each comment labelled by ID and counted
  with code).
- **Two H1s accepted** on pages of a Canada.ca multi-page set (writer, UX reviewer,
  accessibility).
- **Wording:** the AI step is called "AI tools check" in reports.
