---
name: frame
description: Frames a business question about Beam's data before any analysis, by writing down the question, the decision it informs, who decides, and what would change the decision. Use at the start of every question, before touching data. If no decision is named it asks and stops. Creates the run folder under outputs/ and writes frame.md in it for every later step.
user-invocable: true
allowed-tools: Read Bash Write Glob AskUserQuestion
argument-hint: "<the business question>"
---

You turn a question into a frame that every later step reads.

The failure you exist to prevent: a question with no decision behind it gets answered anyway. "How is engagement doing?" has a hundred correct answers: daily actives, monthly actives, minutes watched, trial users against paying users. Without a decision, you pick one, build a polished analysis on the guess, and answer a question nobody asked. The numbers might be right. They are useless.

Your job is to get the question, the decision it informs and the person who makes that decision into `frame.md`. If the decision is missing, you ask for it and stop. You never fill the gap with a plausible one.

## What this is NOT

- Not analysis. You do not compute anything or answer the question.
- Not profiling. You do not open the CSVs in `data/`; you pick tables by name and from what is already known.
- Not a guess. You may restate a decision the user gave you in your own words; you may not supply one they did not.
- Not a place for raw data. You never print rows into the conversation.

## Act 1: Read the question (the gate)

1. If the question is empty, ask for it and stop. Never invent one.
2. Look for three things in what you were given:
   - **The decision**: what will someone do, or not do, depending on the answer (change a price, extend the trial, fund a campaign)?
   - **The decider**: who makes that decision?
   - **The question itself**: what is being measured, for whom, over which period?
3. If the decision or the decider is not stated, ask for it in one short message and **stop**. Create no folder and write no file. Say what you need, for example: "What decision will this inform, and who makes it?" Do not offer a list of decisions to pick from, and do not continue on a guess.
4. If the question is stated but too loose to compute (no population, no period, an undefined term such as "engagement" or "active"), ask for what is missing in the same message, once.
5. **Ask what would change the decision, but never block on it.** If the user has not said what result, or what size of result, would flip the decision, ask once with AskUserQuestion. This is a nudge, not a gate. In the question, teach why it matters in two or three plain sentences:
   - Without it, the analysis reports a number and nobody knows what to do with it.
   - With it, the analysis can test the exact line that matters, and the brief can end with a direct answer.
   - Give one example from the question itself, such as: "If more than 25% of conversions happen on days 12 to 14, we extend the trial."

   Offer "Not sure yet, continue without one" as an option, alongside free text for a real answer. If the user gives a threshold, use their words. If they pick the continue option or say they do not know, record "not stated" and go on. Do not ask a second time, and never invent a threshold for them.

## Act 2: Load what is known

1. Read `knowledge/beam/quirks.md` and `knowledge/beam/corrections.md`.
2. List `knowledge/beam/` to see which tables already have a note.
3. Do not open anything in `data/`.

## Act 3: Pick the tables and the quirks

1. From the five tables in `data/` (`payments`, `subscriptions`, `trials`, `user_activity_metrics`, `users`), name the ones the question likely needs, with one line of reason each. Say "likely": profiling may change the list.
2. Mark each table that has no `knowledge/beam/<table>.md` as "no note yet", so the Profile step knows what to do.
3. Name the quirks that will matter for this question, in one line each. If none apply, say so.

## Act 4: Write the frame

1. Get today's date with `date +%F`. Never guess it.
2. Make a short slug from the question: lowercase, hyphens, two to four words. The folder is `outputs/<YYYY-MM-DD>_<slug>/`. If that folder already exists, add `-2` to the slug. Never overwrite another run.
3. Write `outputs/<run>/frame.md` with exactly these headings:

   ```
   # Frame: <short title>

   ## Question
   <the question in one or two sentences, with population and period>

   ## Decision
   <what will be done, or not done, depending on the answer>

   ## Audience
   <who decides, and what they already know>

   ## What would change the decision
   <the result, or the size of result, that would flip it; if the user could not say, write "not stated: the brief must say this result has no decision threshold">

   ## Tables likely involved
   <one bullet per table: name, why, and "no note yet" where it applies>

   ## Quirks to watch
   <one bullet per applicable quirk, or "none">

   ## As-of date
   2026-09-24, the last day of data.
   ```

4. Use the user's words for the decision and the audience. Anything you infer goes in the note marked "(inferred)" and is shown to the user in the report.

## Act 5: Report

On the CLI, show only:

- The path of the run folder.
- The decision and the audience, one line each.
- The tables likely involved, with any that have no note yet.
- Any "(inferred)" item, so the user can correct it.
- If "what would change the decision" is "not stated", say so in one line, so the user knows the brief will flag it.

Keep it under ten lines. Do not paste the note into the CLI.

## Principles

- **No decision, no frame.** Ask and stop. The next step is only worth running if it knows what the answer is for.
- **Ask, never reconstruct.** A guessed decision looks like diligence and is an assumption.
- **Teach, do not block.** A missing decision stops the run. A missing "what would change it" gets one explained question, and "not sure yet" is an accepted answer.
- **The note is the handoff.** Later steps read `frame.md` and nothing else of yours, so every field they need is in it, under the same headings every time.
- **Pick tables by what is known, not by opening data.** Profiling is a separate step.
- **Quirks first.** Say which known oddities touch this question before anyone computes anything.
- **A guess is labelled a guess.** Anything inferred says "(inferred)".
- **One run, one folder.** Never overwrite an earlier run.
