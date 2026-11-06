# Counting feedback you can trust: building a codebook for Canada.ca comments

*Lana Stewart · November 6, 2026*

In my [first post](one-prompt-wasnt-enough.md) I wrote about building a suite of AI skills for
Canada.ca page audits. This one is about the skill I've spent the most time on since: the
feedback analyst, and the shared codebook it now uses to sort comments from GC Feedback, the
"Did you find what you were looking for?" tool at the bottom of Canada.ca pages.

## The problem with "about 40% of comments mention…"

Ask an AI tool to summarize a few hundred comments and you'll get confident percentages. The
trouble is that it doesn't count them. It estimates. Researcher
[Dr. Cody Dodd](https://github.com/codydodd/validation-first-thematic-measurement) warned me
about this. His point was simple: a theme count is only worth reporting once you've checked it
against people coding the same comments, and until then it should say so.

That changed how the skill works in two ways.

- **It labels, then counts with code.** Every comment gets an ID and a label, and the numbers
  come from counting those labels, not from the model's impression. Anyone can open the coding
  table and check a number.
- **Every category has a status.** A category starts as *Not yet validated* or *Emerging*. It
  becomes *Developing* or *Trusted* only after a blind comparison with an analyst shows enough
  examples (at least 30) and enough agreement (60% for Developing, 80% for Trusted), on that type
  of page. Trusted on one kind of page doesn't carry over to another.

## One codebook for any service

These are my own experiments so far, not something any team is using. I wanted to see whether
one set of themes could work for comments from any page, so results could be compared across
services. Building on an earlier taxonomy I'd drafted for My Service Canada Account, I started
on a core taxonomy with two questions for every comment:

- **Task:** what was the person trying to do? Check a status, apply, update their information,
  get a document or form, find a payment, contact someone, learn about a program or rule, or
  something the service doesn't offer.
- **Issue type:** what went wrong? A technical error, a sign-in problem, a service delay or rule,
  can't find it, confusing content, couldn't complete it online, wrong information, or not
  stated.

Program names and other specifics go in a separate detail field, so the core categories stay
the same everywhere. The core taxonomy is only the first layer. Some tasks need much finer
categories than "sign-in problem", and accounts and sign-in are the obvious first case, so I
expect to add targeted taxonomies for them, and possibly for other tasks or services, that sit
underneath the core one.

I tested it on comments from very different pages: an immigration program, a child benefit,
passport renewal, a public health page and Canada's food guide. Each one broke something, and
each fix went into the codebook:

- **"Looking for something this service doesn't offer."** People on an immigration page looking
  for jobs, or on a federal health page looking for a provincial vaccine record. These aren't
  noise. When enough people ask for the same thing, that's a sign the page needs a clear link or
  explanation.
- **Appointments are applying.** Booking an appointment to renew a passport is part of applying,
  not "contacting someone."
- **Single words stay Unknown.** "Chicken" on the food guide could be a recipe question or a
  food recall. Guessing would make the numbers look cleaner and be less true.
- **No usable content is reported, not hidden.** Blank, random or test comments are counted as
  a share, so the analysis is honest about what it left out.
- **Personal details stay out.** Comments with health details or sign-in information are
  excluded from quotes, and anything suggesting someone is at risk is flagged without being
  quoted.

## Testing it blind

A codebook that only I've applied isn't validated. So I did what Cody suggested: I coded random
samples myself, without seeing how the skill had coded them, and then compared.

I used passport renewal, a page with lots of comments and a clear main task. Two batches of 40
random comments, coded blind in a spreadsheet with dropdowns.

**The first batch** matched on 80% of tasks but only 12% of issue types. Almost all of that gap
was one difference in how we read short comments. The skill coded a comment that only states a
task, like "renewal form", as *issue not stated*. I coded it as *can't find*. My reasoning won:
everyone who leaves a comment has just answered "No" to "Did you find what you were looking
for?", so a bare task statement already tells you what they couldn't find. With that one rule
in place, issue agreement rose to about 88%. The first batch also produced a rule for questions
that could go either way: "Can I…" or "Do I need…" is learning about a rule, while "How do I…"
or "I want to…" renew is applying.

**The second batch**, coded against the updated codebook, matched on 88% of tasks and 90% of
issue types. Across both batches, *Apply* and *Can't find* passed the bar for Trusted on this
type of page. Most other categories haven't reached 30 examples yet, so they stay Emerging.

The disagreements were useful too. Most were about the line between "technical error" and
"couldn't complete it online," which now has a clearer rule: if the comment says what the site
did, it's a technical error.

## Channel churn

One more idea came out of this work. Some comments show people giving up on the web and turning
to the phone, email, mail or an office for help. That's channel churn, and it matters to anyone
who runs a service. It isn't a task of its own. It shows up alongside other tasks. So the
codebook now treats it as a separate yes-or-no flag, counted across every task. Choosing to
apply in person doesn't count. Turning to another channel because the online path failed does.

That also lines up the codebook with how we already think about the Canada.ca journey: find,
learn, sign in, apply, check status, and churn.

## What I'd tell anyone trying this

1. **Count, don't estimate.** If you can't trace a number back to labelled comments, don't
   report it.
2. **Code a blind sample yourself.** Forty comments took less time than I expected, and it
   found the biggest problem in the codebook within one batch.
3. **Write down every disagreement as a rule.** The codebook is mostly a record of small
   arguments, settled.
4. **Say how sure you are.** A count marked "emerging" is more useful than a confident number
   nobody has checked.

## What's next

The results so far rest on one analyst and one page. Next I'd like a colleague to code a batch,
and to test a second type of page, like a benefit status page, before calling anything Trusted
beyond passports. After that comes the first targeted taxonomy, for accounts and sign-in,
validated the same way.

The skill and its codebook are in this repo:
[canada-ca-feedback-analyst](../skills/canada-ca-feedback-analyst/SKILL.md).
