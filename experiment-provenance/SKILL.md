---
name: experiment-provenance
description: Recording experimental runs so their numbers can still be cited months later — immutable run directories, what a run's metadata must be able to answer, when a result may not be cited, grading design decisions against the literature, and pre-registering an analysis before the run that feeds it. Load when running or reporting experiments whose numbers will end up in a paper, report, or decision; when setting up a results directory; or when unsure whether a number on disk is trustworthy.
---

# Recording experiments so the numbers survive

For work whose numbers will be cited later — a paper, a report, a decision someone
else acts on. Not for exploratory scripts nobody will quote.

## 1. Never report a number you did not read from a file

Before stating any number, read it from the results, the cache or the manifest — not
from memory, not from a conversation summary, not from a document that quoted it.
Label predictions as predictions. Check a run's status before citing it.

The failure this prevents: stating a result for a condition that had never been run,
because the hypothesis predicted it and the prediction had been discussed so often it
felt measured.

## 2. Scan the table before writing a universal quantifier

Before writing *all / none / every / exactly / 全部 / 无一*, read every row of the
table the word ranges over, and say what the exceptions are. "Nine of ten are
indistinguishable from zero, one is nominally negative" costs a clause and cannot be
falsified by someone opening the CSV.

A count also depends on its scope. State which: the same runs gave "1 of 10" and "2 of
20" for one metric and for all metrics, and both were true.

## 3. Results are append-only

Never overwrite or edit a result after it is written. Each run gets its own directory,
named by UTC timestamp plus a short tag, holding its outputs and its metadata. An index
per experiment lists them newest-first.

To correct a run, write a **new** one and set the old one's status to `SUPERSEDED` with
a reason. Never delete it, and never silently fix it in place — the superseded run is
usually the evidence for why the new one exists.

**Editing a record's status does not rebuild the generated index that says what is
citable.** Rebuild it, and have a checker compare the two.

## 4. What a run's metadata must be able to answer

In six months, any number in the write-up should be reconstructible without reading
code or git history:

| question | field |
|---|---|
| when, and did it finish | start / end timestamps, status |
| **why does this run exist** | free-text notes — **required**; nobody can reconstruct this later |
| what was typed | full command and parsed arguments |
| what code | commit sha, branch, **and a dirty flag** — plus a hash per source file, which still identifies the code when the tree is dirty and the sha does not |
| **what structure** | model architecture: widths, depths, dropout |
| what hyperparameters | epochs, patience, learning rate, batch size |
| which seeds | the seeds used, and the module default |
| how inputs were computed | feature-extraction settings |
| which input files were consumed | path, size and mtime of each — so drift is detectable |
| what came out | a one-line headline, plus the tables |
| where it ran | CPU/GPU model, package versions |
| what was *not* pinned | determinism actually in force; record what is loose rather than implying determinism that does not hold |

**Provenance written after the fact is not proof.** Backfilling records for artefacts
that predate the system is worth doing, but each backfilled entry must say so — it
describes the code as it stands *now*, not as it stood then. Believing it is the same
as having no record, except that it also looks reliable. In one repo 25 of 40
outstanding warnings are exactly this, and every one of them is a result whose inputs
cannot actually be vouched for.

Read architecture and hyperparameters **out of the source** (parse it) rather than
maintaining a hand-written copy, which drifts.

**A dirty flag must distinguish code from outputs.** Archiving a run leaves untracked
files under results, which would otherwise mark the *next* run dirty when no source
file changed — and a flag that fires constantly gets ignored.

## 5. When a result may not be cited

- its status is not clean, or it is superseded;
- the code was dirty when it ran — the recorded sha does not describe what executed;
- **its metadata predates the field that would settle the question.** A record that
  only says "dirty: true" cannot be decomposed afterwards. Rerun it rather than
  arguing about it;
- an input it consumed has changed since — the run's inputs no longer exist as
  recorded, while nothing about the run looks wrong.

**Commit before a run whose numbers you intend to cite.** A crashed run must leave a
record with its traceback; a run that died silently and left nothing is
indistinguishable from one that never started.

## 6. Cache reuse must check content, not keys

If a cache is keyed by an identifier that two different datasets can share, existence
and coverage checks both pass while the wrong data is read, and the output looks
normal. Key on a content fingerprint — sizes and paths of the underlying files — and
namespace caches by dataset.

Verify this the hard way: two runs agreeing to four decimal places is not
reassuring, it is a symptom.

## 7. Grade every design decision against the literature

Any choice that can move a result gets recorded with a grade:

| grade | meaning |
|---|---|
| **A** | published backing, and the implementation matches it |
| **B** | backed, but the cost is real and must appear in Limitations |
| **C** | an engineering call with no citation yet |

**A C-grade choice must never enter a Method section as an unmarked default.** Either
find the citation and promote it, or state it explicitly as this study's own choice.

Before citing a paper, check its *data and protocol*, not just its abstract. One
project nearly adopted a model as a clean control until its pretraining set turned out
to contain the entire test corpus. And when a choice is a workaround — a hand-written
implementation because a library would not install — say so; it is not a research
decision wearing a costume.

## 8. Pre-register before the run, not after

If an analysis is not pre-registered, write the plan and commit it **first**. Deciding
the model form, the weighting, the threshold, or the exclusion rules after seeing
results is p-hacking, and it is the one failure that provenance cannot repair
afterwards.

Fix in advance: the test and its criterion (including whether the boundary is `≤` or
`<`, which can decide whether the criterion is satisfiable at all), the equivalence
bound if a null result will be claimed, exclusion rules and their justification, the
seeds, and a staging rule if later stages depend on earlier ones — that rule is what
prevents "run a few more until something turns up".

**Note which experiments still have unspent confirmatory value.** Once results have
been read, that analysis is permanently exploratory, and the loss is irreversible. An
experiment nobody has seen output from is the only place a confirmatory claim can
still live — spend it deliberately.

## Maintaining this skill

This file is meant to be edited. **When a project hits a pitfall it did not prevent,
patch it in the same session** — a lesson that stays in one conversation is lost. Before
adding, check the rule would help a project with entirely different subject matter; if
it needs this project's nouns, it belongs in that project's own layer instead. Keep one
concrete failure per rule as its evidence, and its reason, or the next reader deletes it.

Full discipline, including how to port this skill set to a new project and the ways
editing a skill breaks it: **`skill-maintenance`**.
