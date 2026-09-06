---
name: experiment-engineering
description: Building and running the experiments in a research project whose results someone else will explain, audit, or publish — treating your own pipeline as the first suspect behind any result, decomposing a null against a published configuration before believing it, running long jobs without losing them, and reporting findings to a user who will act on them. Load when you are the session that writes the code and runs the experiments, as opposed to the one explaining or auditing them.
---

# Running the experiments

You are the session that writes the pipeline and produces the numbers. Others explain
them, audit them, and eventually publish them.

Two neighbouring skills carry the rest: **`experiment-provenance`** for how a run is
recorded and when it may be cited, **`multi-session-protocol`** for how findings and
retractions move between sessions. This one is about the work itself.

## 1. Your own pipeline is the first suspect, not the last

The single most valuable thing you do is find the places where **your implementation
choices produced the finding**, before anyone builds on them. In one project three
separate results turned out to be artefacts of the experimenter's own choices:

- **A normalisation domain.** Applying the source's feature standardiser to a target
  corpus pushed 3–6% of dimensions past |z|>3 (in-domain: 0.5%); the model then
  predicted one class with 0.93 confidence and three of four targets sat exactly at the
  chance floor. Every condition collapsed equally, so the ablation measured nothing.
- **A preprocessing step that was already redundant.** Contours were median-matched to
  remove a speaker-register confound — but the pairing was already within-speaker, so
  the confound was gone, and what median matching actually removed was pitch *level*,
  which is where much of the emotional signal lives. It manufactured a reversal.
- **A fusion architecture.** Concatenating five raw dimensions onto 25×1024 through one
  linear layer made every added feature stream *lower* accuracy. Read as evidence about
  the features, it supported a confident wrong conclusion.

Each looked like a finding. Each survived until someone decomposed it. **Before
reporting a result as being about your subject matter, ask what in your own code could
produce exactly this pattern** — and prefer the answer you can test today over the one
that is more interesting.

## 2. Decompose a null against a published configuration

A null result is only as strong as the configuration that produced it. When you get
one, find a paper reporting a positive effect on the same question and **reproduce its
choices, one axis at a time**, before claiming anything.

Two axes usually differ at once — the feature set *and* how it is fused, the model *and*
the training regime. Vary them separately: the decomposition itself is a result, and it
is rarely uniform. In one case the loss split 4.6 points to the front-end and 1.7 to the
fusion on one corpus, and the opposite way on another — so "it was the fusion" and "it
was mainly the front-end" were both over-generalisations from a single column.

Until that is done, state the scope honestly: **"this front-end and this fusion add
nothing"** is what you have; *"explicit features add nothing"* is what a reviewer will
read, and will refute with the paper you did not reproduce.

## 3. Long jobs: assume every one of them will be interrupted

- **Checkpoint in shards.** A pass that accumulates results in memory and writes once at
  the end loses everything to a sleeping laptop. Write each shard as it completes and
  skip finished shards on rerun.
- **Never poll with `ps | grep <script name>`.** The shell running the loop matches its
  own command line, so the loop never exits — and the same broken check reports the job
  as running. Wait on a file appearing, or let the harness notify you.
- **Verify a job started, not just that you launched it.** "I launched it" and "the
  output file exists" are different claims; only the second is checkable. Reporting the
  first as the second is how three jobs were reported as running for an hour while none
  had started.
- **A crash must leave a record.** A job that dies silently is indistinguishable from one
  that never began.
- Before starting anything heavy, check whether a competing job is already running.

## 4. Cache reuse is where silent wrongness lives

If a cache is keyed by an identifier two datasets can share, existence and coverage
checks both pass while the wrong data is read, and **the output looks entirely normal**.
Key on a content fingerprint and namespace by dataset.

Treat suspiciously clean agreement as a symptom: two runs matching to four decimal
places is not reassurance, it is usually the same cache read twice.

## 4b. Prove a guard has teeth without arming the hazard

A check that passes because it found nothing is worthless, so always verify a new guard
by making it fail. **But do not create the real hazard in the real artefact to do it.**

Testing a leak detector by planting a real secret in the live file, then reverting, is a
window in which an interruption, a background sync or an autocommit publishes it. The
revert usually works. "Usually" is not the standard for something whose failure is
irreversible.

Instead: plant the trigger in a scratch copy, or reimplement the match against the real
configuration in a throwaway directory. You get the same evidence — the guard fires on
input it should fire on — with no interval during which the thing you are guarding
against is actually true.

This is the general rule: **the test for a guard against an irreversible outcome must
not pass through that outcome.** It applies to deletion guards, spend limits, and
publication filters alike.

## 5. Reporting to someone who will act on it

- **Direction before precision.** If a result points against the hypothesis, say so in
  the first line, then qualify. A caveat that arrives after three paragraphs of numbers
  arrives too late.
- **Say what is not measured.** "No corpus shows a positive effect" and "the effect is
  smaller in group A" are different claims, and only one of them may be supported.
- **Never present a prediction as a result.** If a value has not been computed, say it
  has not been computed, however strongly the design implies it.
- **A smoke test is not a result.** One seed on one corpus flips sign routinely; report
  it as a mechanism check, not a finding.
- When a correction changes what the user should do, lead with the correction.

## 6. Receiving a finding from another session

Verify it against the files yourself — a peer's number is a lead, not a source, and
peers have arrived with correct analyses run on superseded inputs. Then fix it, report
what you changed, and **let whoever raised it close it**; you cannot see your own gap,
which is the whole reason the finding reached you from outside.

When a fix reveals an invariant worth re-checking forever, put it in the project's
checker rather than in your memory — you own that tooling, and it is the only part of
this that survives the session.

## 7. What you owe the other sessions

Changes that invalidate an explanation, logged before you message anyone. Runs you
archived or superseded. **Bugs that invalidate a number you already reported** — those
travel furthest and are the ones most likely to have been quoted onward.

## Maintaining this skill

This file is meant to be edited. **When a project hits a pitfall it did not prevent,
patch it in the same session** — a lesson that stays in one conversation is lost. Before
adding, check the rule would help a project with entirely different subject matter; if
it needs this project's nouns, it belongs in that project's own layer instead. Keep one
concrete failure per rule as its evidence, and its reason, or the next reader deletes it.

Full discipline, including how to port this skill set to a new project and the ways
editing a skill breaks it: **`skill-maintenance`**.
