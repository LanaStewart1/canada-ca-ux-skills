---
name: "canada-ca-accessibility"
description: "Check a Canada.ca or GC page against WCAG 2.1 AA: automated scan, contrast and code checks in the browser, findings mapped to success criteria, plus a short hands-on test script."
---

# Canada.ca accessibility check

You check one Canada.ca or Government of Canada page (English, French or both) against
WCAG 2.1 Level AA. You measure what can be measured, mark what still needs a person, and
never call a page "accessible" or "compliant": automated and code checks find only part of
WCAG failures.

**Standard.** Target WCAG 2.1 AA. The GC Standard on Web Accessibility formally references
WCAG 2.0 AA; EN 301 549 and current GC guidance use 2.1 AA, so 2.1 AA covers both. Name the
success criterion (SC) for every finding, e.g. "1.4.3 Contrast (Minimum), AA".

**Modes.**
- *Standalone*: the user gives a URL. Run all steps and write the full output.
- *Called from canada-ca-page-audit*: use the audit's snapshot and task list, run the browser
  steps on the live English and French pages, and return findings in the shared severity
  scale plus the state for the report's Accessibility chip. Don't repeat findings the
  bilingual audit already made; reference them.

## Shared severity scale

| Severity | Meaning here |
|---|---|
| Critical | Blocks a top task for a group of users: the apply link or button can't be reached or named by keyboard or screen reader, a keyboard trap, a required form field with no label, task information only in an image with no text alternative |
| Major | An A or AA failure on a top-task path that slows or confuses but doesn't block: body-text contrast failure, broken heading structure in the main content, vague link text on a task link, a table with no headers |
| Minor | An A or AA failure off the task path, or a low-impact one (for example, duplicate ids) |

Confidence tags: **[Seen]** measured by the scan or script, or seen in the code;
**[Needs test]** only a hands-on check can confirm; **[Needs judgement]** depends on meaning
(is this alt text good enough? is this link clear in context?).

## Step 1: Load the page in the built-in browser

Use the built-in browser (no Terminal). Open the English page; if the French page is in
scope, use the language toggle link and run the same steps on it. Note the URL and date
modified for each.

## Step 2: Automated scan (axe-core)

Before the first scan in a conversation, ask once: "I'll load Deque's open-source axe-core
scanner from cdnjs into the page to run the scan. Okay?" If the user says no, or the page
blocks the script, skip to Step 3 and say the scan didn't run.

Run with the browser's JavaScript tool (wrap in an async function if top-level await isn't
accepted; pin a current 4.x version):

```js
await new Promise((res, rej) => { const s = document.createElement('script');
  s.src = 'https://cdnjs.cloudflare.com/ajax/libs/axe-core/4.10.2/axe.min.js';
  s.onload = res; s.onerror = rej; document.head.appendChild(s); });
const r = await axe.run(document, { runOnly: { type: 'tag',
  values: ['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'] } });
JSON.stringify({ violations: r.violations.map(v => ({ id: v.id, impact: v.impact, help: v.help,
  sc: v.tags.filter(t => /^wcag\d{3,4}$/.test(t)), nodes: v.nodes.length,
  targets: v.nodes.slice(0, 5).map(n => n.target.join(' ')) })),
  needsReview: r.incomplete.map(v => ({ id: v.id, nodes: v.nodes.length })) });
```

Violations are [Seen]. "needsReview" items are [Needs test] or [Needs judgement], never
failures by themselves.

## Step 3: Code and contrast checks (always run)

Run this script even when axe ran; it covers Canada.ca-specific checks and gives a
fallback when the scan can't load.

```js
(() => {
  const main = document.querySelector('main') || document.body;
  const out = { lang: document.documentElement.lang, title: document.title, issues: [] };
  const add = (sc, rule, el, note) => out.issues.push({ sc, rule,
    where: el ? el.outerHTML.slice(0, 160) : '', note: note || '' });
  const h1s = document.querySelectorAll('h1').length;
  if (h1s === 0 || h1s > 2) add('1.3.1', 'h1-count', null, h1s + ' h1 elements');
  if (h1s === 2) out.twoH1 = [...document.querySelectorAll('h1')].map(h => h.textContent.trim());
  let prev = 0;
  main.querySelectorAll('h1,h2,h3,h4,h5,h6').forEach(h => { const l = +h.tagName[1];
    if (prev && l > prev + 1) add('1.3.1', 'heading-skip', h, 'h' + prev + ' to h' + l);
    if (!h.textContent.trim()) add('2.4.6', 'empty-heading', h); prev = l; });
  main.querySelectorAll('img').forEach(i => { if (!i.hasAttribute('alt')) add('1.1.1', 'img-no-alt', i); });
  const vague = /^(click here|here|read more|more|learn more|cliquez ici|ici|en savoir plus|plus)$/i;
  main.querySelectorAll('a[href]').forEach(a => {
    const t = (a.innerText || a.getAttribute('aria-label') || '').trim();
    if (!t && !a.querySelector('img[alt]:not([alt=""])')) add('2.4.4 / 4.1.2', 'link-no-name', a);
    else if (vague.test(t)) add('2.4.4', 'vague-link', a, t);
    if (a.target === '_blank' && !/new (window|tab)|nouvel/i.test(t)) add('3.2.5 (AAA, advisory)', 'new-window', a); });
  document.querySelectorAll('input:not([type=hidden]),select,textarea').forEach(f => {
    const named = (f.id && document.querySelector('label[for="' + CSS.escape(f.id) + '"]')) ||
      f.closest('label') || f.getAttribute('aria-label') || f.getAttribute('aria-labelledby') || f.title;
    if (!named) add('1.3.1 / 4.1.2', 'field-no-label', f); });
  main.querySelectorAll('table').forEach(t => { if (!t.querySelector('th')) add('1.3.1', 'table-no-headers', t); });
  const ids = {}; document.querySelectorAll('[id]').forEach(e => ids[e.id] = (ids[e.id] || 0) + 1);
  Object.entries(ids).filter(([, n]) => n > 1).forEach(([k, n]) => add('4.1.1 (Minor)', 'duplicate-id', null, '#' + k + ' x' + n));
  const rgb = s => { const m = s.match(/[\d.]+/g); return m ? m.map(Number) : null; };
  const lum = c => { const f = v => { v /= 255; return v <= 0.03928 ? v / 12.92 : ((v + 0.055) / 1.055) ** 2.4; };
    return 0.2126 * f(c[0]) + 0.7152 * f(c[1]) + 0.0722 * f(c[2]); };
  const bg = el => { for (let e = el; e; e = e.parentElement) { const cs = getComputedStyle(e);
    if (cs.backgroundImage !== 'none') return null; const c = rgb(cs.backgroundColor);
    if (c && (c.length < 4 || c[3] > 0.9)) return c; } return [255, 255, 255]; };
  const seen = new Set(); let unknown = 0;
  main.querySelectorAll('p,li,a,span,td,th,h1,h2,h3,h4,h5,h6,label,button,dt,dd,figcaption,summary').forEach(e => {
    if (![...e.childNodes].some(n => n.nodeType === 3 && n.textContent.trim()) || !e.offsetParent) return;
    const cs = getComputedStyle(e), fg = rgb(cs.color), b = bg(e); if (!b) { unknown++; return; }
    const L1 = lum(fg), L2 = lum(b), ratio = (Math.max(L1, L2) + 0.05) / (Math.min(L1, L2) + 0.05);
    const px = parseFloat(cs.fontSize), large = px >= 24 || (+cs.fontWeight >= 700 && px >= 18.66);
    const need = large ? 3 : 4.5, key = cs.color + '|' + b.join(',');
    if (ratio < need && !seen.has(key)) { seen.add(key); add('1.4.3', 'contrast', e, ratio.toFixed(2) + ':1, needs ' + need + ':1'); } });
  out.contrastOverImages = unknown;
  return JSON.stringify(out);
})()
```

Rules for the results:
- `twoH1`: two H1s are accepted on a page of a Canada.ca multi-page set (the program name
  above the section title). Don't report them. Only if the page is a standalone page, report
  it as Minor.
- Contrast is measured from computed colours. Text over a background image is counted in
  `contrastOverImages` and goes to the hands-on list as [Needs test].
- Check `lang` is `en` on the English page and `fr` on the French page (3.1.1), and that
  any passage in the other language has its own `lang` (3.1.2).
- Check the page title is unique and describes the page (2.4.2) [Needs judgement].
- For every image with alt text, judge whether the text does the image's job [Needs
  judgement]. Decorative images with `alt=""` are correct; don't report them.
- Check expand/collapse (`details`/`summary`), tabs and other GCWeb widgets are native or
  WET components; custom ones go to the hands-on list.
- Note PDFs, videos and other attachments: captions (1.2.2) and accessible PDFs need a
  hands-on check.

## Step 4: Separate page issues from template issues

The GCWeb header, menu, breadcrumb, footer, date modified and the "Report a problem" /
feedback widget come from the Canada.ca theme. Report findings there in a separate
"Template issues" list, owned by the theme team, so the page owner isn't asked to fix what
they don't control. Everything inside `main` belongs to the page.

## Step 5: Rate and merge

Merge axe and script findings that describe the same problem. Rate each with the shared
scale, using the task list (or the page's obvious top tasks in standalone mode) to decide
whether it sits on a task path. Count with code, not by eye.

## Step 6: Hands-on test script

Give a short checklist the user can do in about 15 minutes, limited to what this page needs:

1. **Keyboard**: press Tab from the top. "Skip to main content" appears first; every link
   and button is reached in a sensible order with a visible focus outline; nothing traps
   focus; expand/collapse sections open with Enter or Space. (2.1.1, 2.1.2, 2.4.3, 2.4.7)
2. **Zoom and reflow**: zoom to 200%, then to 400% (or a 320 px-wide window). Nothing is
   cut off and there's no sideways scrolling for text. (1.4.4, 1.4.10)
3. **Screen reader quick pass** (NVDA with Chrome, or VoiceOver): open the headings list and
   the links list. Headings outline the page; each link makes sense on its own; the French
   page is read with a French voice. (1.3.1, 2.4.4, 2.4.6, 3.1.1)
4. **Text spacing**: apply a text-spacing bookmarklet; nothing overlaps or disappears.
   (1.4.12)
5. Any item marked [Needs test] above.

## Output

**Summary line**: counts by severity, whether the scan ran, and the date checked. Example:
"0 Critical, 2 Major, 3 Minor on the page; 1 template issue. axe scan ran. Checked
Nov 5, 2026."

**Findings table**: # · Issue · WCAG SC (level) · Severity · Confidence · Where · Fix.
Use plain language in Issue and Fix; put the selector or snippet in Where.

**Template issues**: same columns, owner "Canada.ca theme team".

**Couldn't test here**: the [Needs test] items, each linked to a step in the hands-on script.

**Hands-on test script**: from Step 6.

**For canada-ca-page-audit**: the Accessibility chip is amber ("issues found") when any page
finding is rated; green ("no issues found in automated and code checks") only when the
scan and script both ran and found nothing, with "hands-on checks pending" beside it. If
neither ran, the chip is grey ("not checked"). Never mark it as passed.

## Don'ts

- Don't claim compliance or give an accessibility score or percentage.
- Don't report `alt=""` on decorative images, or AAA criteria as failures (mark them advisory).
- Don't estimate contrast from colour names or screenshots; measure it or mark [Needs test].
- Don't blame the page owner for template issues.