# Canada.ca UX skills

A suite of Claude skills for auditing Canada.ca and Government of Canada web pages. One
command runs a full page audit and produces a shareable report: an at-a-glance panel, the top
5 fixes with severity and effort, and a section for each review.

## What's in the suite

| Skill | What it does |
| --- | --- |
| `canada-ca-page-audit` | Runs the skills below in order on one capture of the page and merges them into one report. Start here. |
| `job-stories-writer` | Writes the user tasks the audit measures everything against (audit mode). |
| `ux-reviewer` | UX review with a journey map and grading rules. |
| `canada-ca-writer` | Content review against the Canada.ca Content Style Guide, with measured sentence lengths. |
| `canada-ca-seo` | Title, description, keywords and structured data. |
| `canada-ca-bilingual-audit` | English and French parity, link language, outdated content, French clarity. |
| `canada-ca-ai-answer-check` | Whether people asking AI tools about the topic are likely to get a correct answer from the page. |
| `canada-ca-accessibility` | WCAG 2.1 AA: automated scan, contrast and code checks, and a hands-on test script. |
| `canada-ca-feedback-analyst` | Measured theme analysis of GC Feedback comments, when an export is provided. |

All skills share one severity scale (Critical, Major, Minor) and confidence tags
([Seen], [Needs test], [Needs judgement]).

## Background

How and why this suite came together: [One prompt wasn't enough: what I learned building a
page audit suite with AI](docs/one-prompt-wasnt-enough.md).

## Install

1. Download the latest zip from **Releases** (right side of this page).
2. Unzip it. Inside is one zip per skill.
3. In Claude, open **Settings**, find the **Skills** section, and upload each skill zip.
4. Start a chat and type: `audit this page: <Canada.ca URL>`

The audit works best in the Claude desktop app with the built-in browser, which captures the
exact English and French pages. Without it, the skills ask you to paste the page HTML.

## Acknowledgements

Thanks to [page-builder](https://github.com/quidampepin/page-builder) by
[quidampepin](https://github.com/quidampepin). Ideas from that project shaped this suite. Chelsey Donohue contributed the bilingual audit skill.

The feedback analyst's measured counting and validation status follow the
[validation-first thematic measurement](https://github.com/codydodd/validation-first-thematic-measurement)
framework by [Dr. Cody Dodd](https://github.com/codydodd): measurement follows validation.

## Suggest a change

Open an **Issue** describing what the audit got wrong or what you'd like it to do, with the
page URL if you can. Changes are listed in [CHANGELOG.md](CHANGELOG.md).
