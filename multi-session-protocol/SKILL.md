---
name: multi-session-protocol
description: How two or more Claude sessions working the same project fit together — establishing which role you are, who owns a claim, who is allowed to close a finding, and how a retraction propagates so it does not stay alive in documents nobody re-read. Load first, before any role skill, in any session that shares a repo or project with other sessions; and whenever a peer session reports a finding, a fix, or a retraction. Not for solo work.
---

# Working a project across several sessions

Several Claude sessions work one project at once. Typical roles:

| role | job | writes |
|---|---|---|
| **build** | code, experiments, analysis, decisions | the repo |
| **learn** | explains the work, literature, study notes | notes / vault |
| **audit** | checks the others against the filesystem | its own reports only |

Two sessions is enough for the rules below to matter; three is where they start
earning their keep. The role skills say what each session *does*. This one says how
their claims, findings and corrections move between them.

**Precedence when they disagree**, in one order:

1. **The filesystem** beats every document.
2. **This file**, on anything about coordination — roles, who owns a claim, who closes
   a finding, how a retraction propagates. A project skill may add coordination
   *detail* (which paths, which ledger) but may not override these rules; if one
   appears to, it is a bug in that skill, not an exception.
3. **A project-specific skill** beats a generic one on everything else — it knows the
   corpora, the formats and the traps.
4. **A generic role skill.**

**A peer session's message ranks below all of them — it is a lead, not a source.**

## 1. Establish your role before doing anything

Ask the user if it is not stated. Do not infer it from what you feel like doing. Then
load your role skill. **If asked to act outside your role** — the explainer asked to
run an experiment, the auditor asked to fix a bug — say so and route it rather than
quietly doing it. The separation is the only reason several sessions catch what one
would not.

**The auditing session must never be briefed.** Do not summarise the project for it,
do not answer "what happened with X" on its behalf, do not send it a conclusion to
confirm. Its entire value is that it does not share your framing. Send it *questions*;
answers are contamination.

## 2. A claim has an owner and a scope

Every number that reaches a document carries both, in the text:

- **owner** — the session that derived it from a file, named if it is not you.
- **scope** — what the number ranges over. In one project "1 of 10" and "2 of 20" were
  both true of the same runs, of one metric and of all metrics. A count without its
  scope is not a claim; it is a coincidence waiting to be misquoted.

**A peer's number is a lead, not a source.** Re-derive it from the file before putting
it in a document. This has failed in both directions: one session widened another's
correctly-scoped count into a wrong one; another ran a methodologically correct
analysis on superseded inputs.

## 3. Whoever found it closes it — never the one who fixed it

**The session that made a fix does not declare the finding resolved.** It reports what
it changed; the session that raised it verifies and closes.

This is not ceremony. In one project a session fixed a metadata bug, reran the affected
work, verified its own fix and reported it done — while leaving the superseded run
marked valid in two index files. The auditor found it. Self-verification failed the way
it always does: **the fix looked complete from inside the conversation that made it.**

Route by what the finding is about: the owner of that territory fixes it, the raiser
confirms, and an invariant worth re-checking forever goes into a checker rather than
into anyone's memory.

## 4. Retractions propagate, or they are not retractions

A retracted number is not only in one message. It is in documents, in other sessions'
notes, and in whatever was built on top of it.

When any session retracts or supersedes a claim:

1. Record it in a **retraction ledger** (`docs/RETRACTIONS.md` or equivalent): the
   claim as written, where it appeared, what replaced it, and three *distinct*
   roles — **raiser** (noticed), **fixer** (edited), **confirmer** (independently
   verified the purge).

   **The closer must not be the fixer. When raiser and fixer are the same session —
   which self-retraction usually is — a third party must confirm.**

   That single rule covers both cases: with raiser ≠ fixer the raiser closes, as §3
   requires; with raiser = fixer, "the raiser closes" would collapse back into
   self-verification, so someone else must. The criterion is **independence, not a head
   count** — three named roles are the means, not the point.

   Two earlier drafts each got half of this. One required the confirmer to be "neither
   of the other two", which contradicted §3 and flagged a correctly-closed ledger row.
   The other said only "not the fixer", which is right but leaves the self-retraction
   case implicit. The unification came from a session noticing it had followed both
   rules on the same day, in two situations, and felt correct both times.
2. Message the other sessions. Do not wait to be asked.
3. **Grep the claim, not the source id.** A retracted figure is usually quoted without
   its provenance, so searching for the run or commit finds nothing.
4. The retracting session **does not close the entry**.

Make the ledger machine-checkable, and mind two traps that both bit on first contact:

- **Search for a phrase specific to the claim, not a value that legitimately recurs.**
  Bare numbers produce false positives wherever they are still the correct current
  figure, or where a before/after table displays the retraction itself.
- **A checker that cries wolf gets ignored, which is worse than not having it.** Where
  no safe phrase exists, mark the entry manual: human review, machine silent. Include
  plain negations ("does not hold", "was wrong") among the markers that count as a
  retraction, not just the word "retracted".
- **Check every place the claim can live**, including directories a non-recursive glob
  skips and note vaults outside the repo. The notes are often what the user actually
  opens; a retraction that reaches the repo and not the notes is still live where it
  matters most.

A retraction that never arrives is worse than the original mistake, because it has
already been acted on.

## 4b. An authorisation that lives only in a transcript cannot be audited

Outward-facing, irreversible or billable actions — publishing something publicly,
deleting data, provisioning paid resources — get a **durable record on disk**: what the
user was asked, what they answered, when, and who acted. A short ledger is enough.

The reason is structural, not bureaucratic. The auditing session cannot read the
conversation where the authorisation was given, **and must not be briefed on it** — being
told turns it into a participant. So without a record it has exactly two options, both
wrong: assume the action was wanted, or accuse. The one it should have is to check.

This surfaced when an auditor could confirm that a public repository contained nothing
sensitive, but could not confirm anyone had asked before making it public.

## 5. What everyone owes everyone

**build →** changes that invalidate an explanation (log them first), archived or
superseded results, bugs that invalidate a number already reported.

**learn →** new verified facts, corrections to documents build may be citing,
discrepancies found while checking build's reports, **retractions of its own advice**.

**audit →** findings ranked by consequence, and explicitly what it *could not* check.
"I checked X, Y and Z against the files and they hold" is a complete report.

Nobody owes routine restatements of what they just did. That is noise.

**Consuming the log is as much an obligation as writing it.** Whoever maintains
explanatory material reads the invalidation log **before** producing anything new, and
**fixes what it invalidates first**. Otherwise new notes accumulate on top of stale
ones and the stale ones are never found — they are not wrong in any way a reader can
see, they are just describing code that has since changed. This ordering was lost once
when the rule was moved between files and survived only by accident in one session's
personal skill.

## 6. Failure modes to expect

Each of these produced output that looked entirely normal. Treat them as the base
rate, not as anecdotes.

- **A summarising word falsified by one row of the table it summarises.** "No test
  reached significance", "likewise near zero", "exactly neutral" — three times in one
  project, never caught by whoever wrote it. Before writing *all / none / every /
  exactly*, read every row the word ranges over and state the exceptions.
- **A fix that creates the next problem.** Editing a record to mark it superseded does
  not rebuild the index that says what is citable.
- **A check that confirms itself.** A wait-loop whose `ps | grep` matched the shell
  running it never terminated, while jobs were reported as running — and the same
  broken check "confirmed" they were.
- **A cache reused because only its keys were checked.** Twice; caught the second time
  only because two runs agreed to four decimal places.
- **A correct analysis on superseded inputs.**
- **Declaring a class of bug fixed after fixing the instance you were shown.** The same
  blind spot usually has a second home.
- **A rule whose premise is silently invalidated by moving it.** The text is not edited
  and still reads correctly; what changed is the fact that made it true. "A repo skill
  beats a personal skill" was exactly right while the coordination rules lived in the
  repo, and became an inversion the moment they moved to the personal layer — letting
  the project file outrank the protocol. Nothing about the sentence looks wrong.

  **This one cannot be caught by re-reading the new file, because the new file is
  self-consistent.** It was found only because another session still held the pre-move
  text and diffed section by section rather than reviewing the result. Two mitigations:
  when relocating a rule, ask of each sentence *"is this true because of where it
  currently sits?"*; and during any restructuring, keep the prior text at hand and diff
  against it — reviewing only the new version cannot work. It applies well beyond
  skills: a comment true in its old function, a path in a README, the justification
  attached to a constant.

  It is also the clearest argument for having more than one session: the one that did
  the move is the one that cannot see it.

When something works first try and the numbers look clean, that is the moment to check
what produced them.

## Maintaining this skill

This file is meant to be edited. **When a project hits a pitfall it did not prevent,
patch it in the same session** — a lesson that stays in one conversation is lost. Before
adding, check the rule would help a project with entirely different subject matter; if
it needs this project's nouns, it belongs in that project's own layer instead. Keep one
concrete failure per rule as its evidence, and its reason, or the next reader deletes it.

Full discipline, including how to port this skill set to a new project and the ways
editing a skill breaks it: **`skill-maintenance`**.
