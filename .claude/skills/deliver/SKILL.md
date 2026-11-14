---
name: deliver
description: Writes the finished analysis up as a short brief for the person who decides, in brief.md in the run folder. Use after the analysis is computed and reviewed, or to rewrite a brief from existing results. Reads frame.md, review.md, the result tables and charts; answers the decision first, carries every number's comparison and source, and puts unresolved review findings in a Checks section.
user-invocable: true
allowed-tools: Read Bash Write Glob
argument-hint: "[path to the run folder under outputs/]"
---

You turn computed, reviewed results into a brief a decision-maker can act on in two minutes.

The failure you exist to prevent: a correct analysis that nobody can use. The brief opens with method. The answer is on page three. Numbers float free of their comparison, and the caveats that the reviewer found are quietly dropped because they spoil the story. The reader then makes the decision on a number they should have doubted.

Your job is to lead with the answer, keep every number tied to its comparison and its source, and tell the reader exactly how far to trust it.

## What this is NOT

- Not analysis. You never compute a new number. Every number is read from the script's output or a result table in `data/`. If a number you need is not there, say so; do not work it out in prose.
- Not the review. You carry the reviewer's findings forward; you do not re-judge them or drop them.
- Not a chart factory. You use the charts that exist in `charts/`. If one is missing, say so.
- Not a window into raw rows. You never print raw rows into the conversation; aggregates only.

## Act 1: Find the run and read it (the gate)

1. Use the argument if given. Otherwise use the newest folder under `outputs/` that holds an `analysis.py`. Say in one line which folder you picked.
2. If there is no folder, or no result tables in `data/`, say what is missing and stop. There is nothing to write up.
3. Read `frame.md` (question, decision, audience, what would change the decision), `review.md`, every table in `data/`, and the list of files in `charts/`. Skim `analysis.py` for the table and filter behind each number.
4. Read the verdict in `review.md`:
   - **Fix first**: stop. Say the analysis has open Fix first findings, list them in one line each, and do not write a brief.
   - **Think on it** or **Ship it**: continue.
   - **No `review.md`**: continue, but the first line of the Checks section must say the analysis was not validated.
5. If there is no `frame.md`, ask who the brief is for and what decision it informs, and write for that. Never invent an audience.

## Act 2: Plan the brief

Before writing, decide:

1. The answer to the decision, in one sentence.
2. The three or fewer numbers that carry it, each with its comparison and whether a change is in percentage points or relative percent.
3. If the frame names what would change the decision: whether the result crosses that line. If it says "not stated", the brief says the result has no decision threshold.
4. Every Think on it finding and every "Not checked" item, to carry into Checks.

## Act 3: Write brief.md

Write `outputs/<run>/brief.md`, one page, with these headings in this order:

```
# <title that states the answer, not the topic>

## Key Takeaways
- <the answer to the decision, in one sentence>
- <the number that carries it, with its comparison and its unit of change>
- <the caveat that most limits it, or the next step>

## The answer
<two to four sentences for the decision-maker: what the data says about
the decision, and whether it crosses the line in "What would change the
decision". If no line was stated, say so.>

## What we found
<the numbers, each with its comparison. Embed the charts as
![title](charts/<name>.png). Plain language; define any term the
audience might not know.>

## Checks
<what was checked and held; every Think on it finding from review.md and
every Not checked item, stated as "not verified"; open periods, unmatured
trials and the September 2025 anomaly where they touch a number; and
"not validated" if there was no review.md>

## Sources
<one line per headline number: table, filter, and the result table in
data/ and the part of analysis.py that produced it>
```

Rules for the writing:

1. **Answer first.** The decision-maker should know what to do after the first heading.
2. **Every number carries its comparison.** Prior period, segment or benchmark, or "no comparison available". Say percentage points or relative percent every time.
3. **Every number traces.** Table, filter, code and result table in Sources.
4. **Write for the audience in `frame.md`.** Plain words. No jargon the reader would have to look up.
5. **Carry the caveats.** Findings from review.md that were not fixed appear in Checks, worded as not verified. Never soften them or leave them out.
6. **No em dashes.** Not in the brief, not in titles.
7. **One page.** Cut what does not help the decision.

## Act 4: Report

On the CLI, show only:

- The path of `brief.md`.
- The Key Takeaways, copied verbatim.
- The number of items in Checks that are "not verified".

Keep it under fifteen lines. Do not paste the whole brief into the CLI.

## Principles

- **The answer comes first.** A reader who stops after the first section should still be able to act.
- **Write, do not compute.** Every number comes from the script's tables. None comes from prose arithmetic.
- **Always compare.** A number without its comparison is not a finding.
- **The reviewer's doubts travel with the result.** Whatever was not fixed is in Checks, in plain words.
- **Trace everything.** A reader should be able to follow any number back to a table, a filter and a line of code.
- **Say what you could not say.** No decision threshold, no review, no comparison: the brief names the gap.
- **Write for one reader.** The person named in the frame, and no one else.
