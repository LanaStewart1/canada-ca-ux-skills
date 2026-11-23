---
name: "canada-ca-service-experience"
description: "Check how well a Canada.ca service journey lets people learn if it applies, apply, update, check status and get help, scored by stage with page and service fixes. Use alone or from canada-ca-page-audit."
---

# Canada.ca service experience

A page can be clear and still leave people unable to get the service done. This skill follows
each task from the Canada.ca entry page to where it actually starts, and judges the journey
stage by stage. The stages use the same names as the feedback analyst's codebook tasks, so coded
comments roll straight into them.

| Stage | The person's question | Codebook tasks that feed it |
| --- | --- | --- |
| **Learn** | Does this apply to me, how much, and which way should I do it? | Learn about a program or rule; Looking for something this service doesn't offer |
| **Sign in** | Can I get into my account? (only when the service uses one) | Sign in or register |
| **Apply** | Can I start now, from here, on my phone? | Apply or submit documents; Get a document or form (a blank form, before applying) |
| **Update** | Can I change my details, send documents or get a letter after applying? | Update my information; Get a document or form (letters and statements, after applying) |
| **Check status** | How long will it take, what happens next, where is it now, when am I paid? | Check status; Find my payment or benefit details |
| **Contact** | It didn't work, I disagree, or I'm not eligible: who do I reach and what's my next step? | Contact or get help; reconsideration and appeals; the churn flag |

**Find** isn't a stage: it's the "Can't find" issue type, measured across every stage. The
feedback analyst counts it. What a program is in general stays with the content, SEO and AI
tools skills; Learn here is about the person's own situation.

## Modes

- **Standalone:** the user gives an entry URL (and optionally a feedback export or analysis,
  and analytics). Capture the pages yourself, in the built-in browser where available. If a
  feedback export is attached, code it with canada-ca-feedback-analyst in Codebook mode first.
- **Called from canada-ca-page-audit:** use the audit's snapshot, link-follow data, task list
  and feedback analysis. Fetch only the pages the journey needs that the snapshot doesn't have.
  Return findings on the audit's shared severity scale.

## Step 0: Is this a service journey?

A service journey ends in something the person does with the government: apply, renew,
register, book, update, pay, claim, check a status. If the page set has no such task (a report,
a budget, a policy statement, general health information), say "Not a service journey: this
check doesn't apply" in one line, give the Service check a grey "not applicable" in the audit
panel, and stop. Never invent service gaps for an information page.

## Step 1: Map the stages and routes

1. **Which stages apply.** Leave out Sign in when the service doesn't use an account, and
   Update when there's nothing to change after applying. Mark them Not applicable, with the
   reason.
2. **Where GC Feedback shows friction.** Using the coded feedback, count comments by stage
   (map each codebook task as in the table above), and give each stage its share of usable
   comments. GC Feedback comes only from people who answered "No" to "Did you find what you
   were looking for?", so these shares show where people get stuck, not what everyone comes
   to do. Never describe them as what visitors came for. If analytics are provided (page
   views, clicks to the application or the account, exits), use them for where people go,
   and say which source each number comes from.
3. **Routes for each stage:** online, in person, by mail, by phone, through an agent or
   representative, and who each route is for (each route's own rules).
4. **Which account, and what happens inside it.** Name the account the service uses, if any
   (My Service Canada Account, CRA My Account, an IRCC account, GCKey or a Sign-In Partner,
   or another), and which stages the public pages say happen inside it. GC Feedback runs
   only on public pages, so there is usually no feedback from inside an account. While
   coding the feedback, flag each comment that mentions the account ("in my account", "my
   account says", "can't upload", sign-in or lockout problems) as **Account mentioned**: these
   come from people who left the account to look for help, and are the only GC Feedback
   signal about it.

## Step 2: Walk the journey

For each stage and route, follow the links from the entry page to the point where the task
actually starts: the sign-in or account page, the online form, the booking tool, the PDF
form, the mailing address, the phone number. Include pages off Canada.ca (portals, booking
sites). Record:
- **Clicks to start:** from the entry page to the start point, counting each page load
- **Hand-offs:** each time the person changes site, sign-in system, channel or document type
- **Phone:** whether the start point works on a phone. Check the pages at phone width (375
  pixels), and quote any instruction that rules phones out ("Don't use your phone or tablet")
- **Pages:** the URL of each step, and its date modified
- **Not described:** a stage the feedback shows people need but no page explains (for
  example, how to send documents after applying). Record it as a gap, not as zero clicks.

Stop at sign-in pages, payment pages and form submission. Never sign in, create an account,
book, pay or submit anything. What happens beyond that point is [Needs test].

The sign-in page is an **evidence wall**. For any stage that happens mostly inside the
account, score only what the public pages show, and mark the stage **Inside the account: not
seen**. Never score it Yes or No on what happens in the account without evidence from one
of these sources, each named with its date when used:
- **Sign-out or exit feedback** from the account (for example, the MSCA sign-out survey)
- **Account mentioned** comments from the public pages' GC Feedback
- **Call-centre reasons** for people who called after trying online
- **Analytics** from inside the account, if it's tagged (drop-off by step, trips back to
  public pages)
- **Sign-in errors and lockouts**, if the account team shares them
- **Usability testing** with a test account, run by the service team

If none of these is available for a stage that lives in the account, that is itself a
gap: "No evidence from inside <account> for this service." Its owner is a service fix, and
the fix is getting one of the signals above in place for this service.

## Step 3: Score each stage

Each stage answers one question: can people do what they need to here? Answer with:
- **Yes:** they can
- **Partly:** they can, but something gets in the way
- **No:** there's no workable way to do it online
- **Not applicable:** this service doesn't have that stage
- **[Needs test]:** it's behind a sign-in or submission, so you couldn't see it

Give the evidence: quoted text and the page it's on. Score each stage on its own; don't merge
stages into one verdict.

- **Learn:** can someone confirm on the site that the service applies to them, what they'd
  get, and which route fits? Look for checkers, estimators, clear eligibility lists, and
  routes compared in one place. Partly if the information exists but is split across pages, or
  if the person has to work it out from if/then text. Use "Looking for something this service
  doesn't offer" comments as evidence: a large share means the entry page needs to point people
  to the right program (and to any program that must come first).
- **Sign in:** is it clear which account to use, and is the sign-in linked where the task
  needs it?
- **Apply:** can someone start within 2 clicks of the entry page, on a phone? Yes only if
  the main route's start point is linked from the entry page or one level down, and works on a
  phone. Note any route that only works on a computer or with special software.
- **Update:** can people change their details, send documents and get letters or statements
  after applying, and is the way to do it described? No if the only routes are the phone or
  paper forms by mail and feedback shows people looking for an online way.
- **Check status:** does the journey say how long it takes, what happens next, where to
  check status, and when payments arrive? Look for processing times, service standards, status
  links for each route and payment dates. Partly if timing is clear but status is available
  only by phone.
- **Contact:** when something fails, the person disagrees or isn't eligible, is there a clear
  next step? Look for "if you can't" or "instead" text, other routes linked at the point of
  failure, contact options with hours and accessibility (TTY, VRS, callback), published wait
  times against the standard, help for technical problems with the portal or form, and
  reconsideration forms linked where people look for them. Use churn comments (people turning
  to phone, mail or an office because online didn't work) as evidence.

Use feedback where it exists: the stage counts from Step 1, "Unable to complete online" and
Technical error issues, and the churn flag. Label each count measured or approximate, and
validated or not, as the feedback analyst does.

## Step 4: Who can fix it

Tag every finding with an owner:
- **Page fix:** the web team can do it in the content or page design (a link on the entry
  page, one eligibility checker for all routes, status information on the right page)
- **Service fix:** it needs the program, IT or a policy change (a form that works on phones,
  online updates or document upload, online booking where there is none, a status tracker,
  wider online eligibility)

Give each an effort estimate (Small, Medium, Large) for the owner to confirm. Service fixes are
usually Large: say who would own them, not how to build them.

## Step 5: Severity

On the shared scale:
- **Critical:** a stage that a large group of people needs can't be done by them at all (for
  example, the only route doesn't work on a phone and there's no alternative), or the journey
  gives conflicting eligibility or deadlines
- **Major:** a stage holding a large share of comments has no online route or isn't described,
  the start point is 3 or more clicks from the entry page, a route works only on a computer,
  status or timing is missing, or there's no next step when the online path fails
- **Minor:** everything else

## Guardrails

- Judge against GC standards first: accessibility, official languages, privacy, plain
  language. Private-sector services are examples of what's possible, never the measure.
- Never recommend upselling, urgency nudges, collecting personal data to personalize, or
  anything that would weaken accessibility or English and French parity.
- A static page isn't a problem in itself. A clear page with a direct link to the next step
  beats an interactive tool that's slow or inaccessible. Recommend a checker or tool only
  where the answer truly depends on the person's situation.
- Never invent processing times, steps or features. If you couldn't see it, it's [Needs test].
- Count comments with health, financial, criminal or other personal details, but never quote
  them.

## Output

1. **Summary:** one or two sentences: which stage works best, and where the friction starts
   and is worst. Then the **service journey**, drawn as one diagram where the doc supports it:
   the stages left to right on a single line, in journey order (Learn, Sign in, Apply,
   Update, Check status, Contact). Under each stage:
   - the stage box, tinted by its result (green Yes, amber Partly, red No, grey Not
     applicable or Needs test), with the worst stage outlined more heavily
   - the result word
   - a small bar for its share of GC Feedback comments, with the percentage
   - a coloured flag and a 2-line friction note in plain words ("No online route: phone or
     mail"), or what works for a green stage
   - when the service uses an account: a dashed vertical line at Sign in labelled "Evidence
     ends here", and a dashed outline on each stage marked Inside the account: not seen

   A legend under the line: red = no route, amber = friction, green = works, and what the bar
   measures (comments from people who answered No). The title states where the friction is
   ("Applying works; the friction starts after people apply"). Owners don't go in the
   diagram. A short scope line follows: pages captured, date, feedback period, and whether
   counts are measured or validated.
2. **What happens at each stage:** open with one line saying what Yes, Partly, No and Not
   applicable mean (as in Step 3). Then, for each stage in journey order:
   - the stage name as a small heading
   - **Can people do it?** the result word, then 2 or 3 sentences on what the person finds and
     what gets in the way, in their terms, with the comment count where it matters
   - **Action needed:** the owner (Page fix, Service fix, or both, with the page fix that can
     happen now), the fix in a few words, and the gap numbers from Service gaps to fix. "None." when nothing is
     needed.
3. **What GC Feedback tells us:** start by saying the comments come only from people who
   answered No, so they show friction, not what everyone came to do, and that click data
   would show that. Then a bar chart of comments by stage in journey order, the stage with the
   most friction in the accent colour, and one line per stage on what people asked for.
4. **Inside the account** (only when the service uses one): one line naming the account and
   which stages happen in it, then a short table: Source · Available? · What it says, by
   stage. List every source from Step 2, including the missing ones, so the gaps are visible.
   End with the one source that would most improve the next audit.
5. **Clicks and hand-offs to start each task:** a table with Stage and route · Who it's for ·
   Clicks to start · Hand-offs · Works on a phone · Where it starts (link).
6. **Service gaps to fix:** # · Gap · Stage · Severity · Confidence · Owner · Effort · Fix.
   Start each Gap cell with a bold short name of 3 to 6 words ("Status only by phone"), then
   the evidence below it. Confidence uses the suite's shared tags: **[Confirmed]** (checked on
   the live page and quoted), **[Needs judgement]** (the evidence is clear, but the program
   needs to say whether it's an error) or **[Needs test]** (behind a sign-in, a submission or
   a letter). A gap can combine two ("[Confirmed]; letter wording [Needs test]"). Put a
   one-line key explaining the three under the table.
7. **What works:** patterns other services could reuse (for example, a status link for every
   route, published wait times), with their page.
8. **Measures to track:** the stage shares from the comments, the share of "Looking for
   something this service doesn't offer", the churn rate, "Unable to complete online"
   comments, Account mentioned comments, sign-out feedback by stage where it exists, phone
   wait times against the standard, and task completion and ease from GC TSS
   where it runs. Add analytics measures when available (clicks from the entry page to the
   application or account, exits from each stage).

Keep it short. A manager should be able to read the summary and the journey in a minute.