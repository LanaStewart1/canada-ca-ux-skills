# Changelog

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
