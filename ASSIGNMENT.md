# AI Product Builder - Take-home assignment

Improvado, Product Team. Deadline: 3 calendar days from receiving this kit. Deliverables in English.

**Designed for 90 minutes to two hours.** We grade what is in the repository at that point. If you run
out of time, ship the narrow thing that works rather than the broad thing that does not. How you cut
under a limit is part of what we are reading.

## The situation

We sell a marketing data platform. One module in it is **data governance**: an agency writes rules over
its own advertising data, and the platform checks them on a schedule. Typical rules:

- every campaign name must contain an allowed region code
- no campaign may be limited by budget
- every campaign must carry a tracking template
- click-through rate must not fall below a set floor

Each run of a rule evaluates many entities, such as every campaign in an ad account. For each run we
store how many entities were checked and how many broke the rule. Broken rules show up on a dashboard,
and a daily email digest goes to the people who own the rule.

A few dozen agencies use the module. Between them they have about a thousand rules. Some agencies pay us
a lot and some very little. Some built their rules once, two years ago, and have not opened the module
since.

Our internal definition of a useful rule is strict: **a finding plus an action, delivered as a
notification.** A rule that produces something nobody receives, or something with no action attached,
does not count.

## Your job

Work out what this product is actually doing for these agencies, build the thing that catches it, and
give us one number.

Nobody will tell you what is wrong in the data or how much of it there is. Finding that is the
assignment.

## The data

Synthetic, but shaped exactly like our production tables. Money is given as a band rather than a figure.
Everything you need is in these four files, and nothing outside them is available to you.

| File | Columns |
|---|---|
| `rules.csv` | `rule_id, agency_id, rule_name, rule_type, status, frequency, created_at, owner_emails, last_successful_check_date, processing_error_message, view_update_time, author_email` |
| `check_events.csv` | `rule_id, agency_id, check_date, checks, broken` - one row per rule per run: entities evaluated, violations found |
| `agencies.csv` | `agency_id, name, arr_band, plan, csm, status, is_sandbox, is_test` |
| `schedules.csv` | `rule_id, schedule_created_at, owner_email` |

Notes on the columns:

- `status` on a rule is what the agency set in the interface. `frequency` is how often it is meant to run.
- `processing_error_message` is empty when the last run of the rule completed.
- `owner_emails` can hold several addresses separated by semicolons. Some are agency staff, some are ours.
- `author_email` is whoever created the rule. That is not always a person.
- `arr_band` is one of `xs, s, m, l, xl`.
- Not every rule has a row in `schedules.csv`.

## Deliverables

Three things. A link to a repository.

**A. Code that runs and prints one number.**

Runs from a clean clone, on these files, with one command. We may run it on a different copy of the
data, so it must not depend on figures you read off this one and typed in. Any language, any stack.

You pick the number. It should answer: **how much of this product is currently capable of causing
somebody to do something.** Its definition lives in the code, not in prose.

**B. The notification.**

The literal text or payload that a specific named person receives, with the finding and the action in
it. Plus one line: how many of these go out per day, and why that is the right number.

**C. Half a page in the README.** Three short answers:

1. What your code could have alerted on and you deliberately decided to suppress, and why.
2. What you would need from an engineer, written precisely enough that they could act on it.
3. What evidence would make you shut this feature down.

Plus one short paragraph: what you handed to an AI model and what you checked yourself.

## Rules

- **AI tools are allowed and expected.** We use Claude Code daily. We grade judgment, not typing speed.
- **Do not spend any of your two hours on:** a user interface, a deployment, a container, a dbt project,
  tests beyond what you need to trust yourself, or a tidy repository structure. None of it earns a point.
  A single script that prints the answer is a perfectly good submission.
- **Say what you cannot know.** If something we asked for cannot be computed from the files you were
  given, write that down and tell us what you would need instead. That answer scores higher than a
  confident number we cannot trace.
- If something is ambiguous, make an assumption, write it down in one line, and move on. That mirrors
  the job.
- If something in the data looks wrong to you, that is worth telling us either way.

## How we read it

Five things, weighted roughly equally. We would rather tell you than have you guess.

| What we look at | What a strong answer looks like |
|---|---|
| It runs | clean clone, one command, works on data you have not seen |
| Architecture | your logic holds for a rule you have never looked at, and for an agency that signs up tomorrow, without anyone editing your code |
| You understand the marketing | you can tell a rule that genuinely constrains something from a rule that cannot fail, and you rank by what is at stake rather than by row count |
| It is actionable | a finding, an action, a named recipient, a deliberate volume, and a defined case where your product stays quiet |
| You are straight with us | you name your own false positives and the limits of your own answer before we find them |

There is no single correct answer and we are not holding a solution key you have to match. Two candidates
can score well with different headline numbers. What we cannot assess is a submission that tells us
everything is fine.

## What happens next

A 30-minute review call: a walkthrough of your solution, a few questions about your decisions, and one
small live exercise on the same data. Part of the call will be in English.

Questions at any point are welcome and asking one does not count against you.
