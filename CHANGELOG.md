# Changelog

Newest first. Every published change is explained here — what changed, why, and what a
reader who already uses these skills should do differently.

Corrections quote the old wording, because otherwise you cannot tell whether the thing
you remember is still true.

## 2026-09-16 (later still) — correction to the record

### Corrected — the previous commit carried an unrelated change

`dbb0531`, whose message describes environment splits and dangling references, also
contains a one-line addition to `study-notes/SKILL.md` — a rule about not merging
separate atomic units into one note — **written by a different session and swept up by
`git add -A`**. The content is sound and stays; the commit message does not describe it,
and the history is not being rewritten to hide that.

### Added — `publishing-to-github` §4: `git add -A` stages what others are doing

A live directory shared with concurrent sessions is being edited while you commit. A
sweeping add publishes their work-in-progress under your message, and a half-finished
edit goes public with it.

**Do differently:** stage by path when the directory is shared; run `git status` first,
and if files you did not touch appear, ask whose they are rather than assuming they are
stale.

## 2026-09-16 (later) — two more

### Added — `research-software-practices` §2: make the dependency files enforce the split

Documenting "two environments, deliberately" while shipping one `requirements.txt`
containing both sets guarantees the obvious command violates it. A project doing exactly
that ended up with torch beside numpy 2.x in the environment documented as having no
torch, where it **half works**: tensor operations run, `torch.from_numpy` raises.

**A half-working install is worse than a missing one** — it passes every "is it
available?" guard, so optional-dependency skips never fire and the suite reports more
passes than it actually ran. That is how a test count of 53 was published when one
environment ran 49 and the other 53.

### Added — `skill-maintenance` §4: a reference to something you meant to write next

Writing *"described in the changelog under today's date"* and then not writing the entry
produces a document that is confidently wrong and reads as authoritative, because the
pointer is specific. Found by an auditor following the pointer. **The reference and the
thing it points at are one unit of work.**

## 2026-09-16 (later) — fix

### Fixed — a duplicated section in this file

The `2026-09-16` entry below was published twice, byte for byte, with the older
section sandwiched between the copies. Live and public for nine days; found by an
auditing session, not by me.

Cause: a scripted edit anchored on `## 2026-09-06`, which is also a prefix of
`## 2026-09-06 — initial publication`, so an unanchored `str.replace` inserted the new
block at both matches.

**Do differently:** anchor scripted edits on a string unique in the file, or pass a
count — and **re-read the file afterwards**. `skill-maintenance` and
`publishing-to-github` now both say so, the latter because its "verify what actually
landed" rule checked the *transport*: `HEAD` matched `origin/main`, the push was
perfect, and the content was wrong.

## 2026-09-16

### Added — `research-software-practices`

The codebase, as distinct from the record of a run (`experiment-provenance`) and from
diagnosing a result (`experiment-engineering`): project layout, an explicit environment
with a lock file, tests, code written for humans, a README that states the scientific
contract, and one documented command from clone to figures.

Distilled from a graduate handout on software practices for scientific computing, kept
only where it was not already covered and sharpened with what this set had learned.

**The section that earned its place is §3, and one rule inside it:**

> **A verification you ran once by hand and never again is not a test.**

A project using these skills had verified, by hand and once each, that a padding mask
was correct to 0.003%, that a corpus yielded exactly its published utterance count, that
two significance tests agreed on synthetic data, and that a confidence interval
contained its own estimate. All four were recorded in its notes as *"verifications worth
keeping in mind rather than repeating"* — which is exactly backwards. Each costs seconds
to run forever and each was silently at risk from every later refactor. The project had
no `tests/` directory at all.

**Do differently:** when you verify something by hand and feel pleased about it, that is
the moment to make it a test, not to write a note saying it was checked. Scientific code
makes this cheap, because the science supplies the properties — a degenerate
self-comparison must return zero, an interval must contain its estimate, a padded batch
must match the same item alone.

### Changed — `experiment-engineering`

Its §1 (your own pipeline is the first suspect) now says that the fix for a pipeline
artefact is not only to correct the result but **to add the test that would have caught
it**. Every artefact listed there was findable by a property test.

## 2026-09-06

### Added — `publishing-to-github`

Publishing a repo and keeping it updated: the checks that must happen before a first
public push because publication is irreversible, and the requirement that **every**
subsequent push is explained to three audiences — the user in plain language, the repo
in a dated changelog entry, and `git log` in a commit message that stands alone.

Prompted by publishing this repository and finding the explanation step was nowhere
written down. Also carries the mechanics that cost time otherwise: `gh auth login` does
not configure git's credential helper, so the first push fails with *"could not read
Username"* until `gh auth setup-git` runs.

**Do differently:** if you push anything, say what changed. "It's pushed" is not a report.

### Added — `experiment-engineering` §4b, *prove a guard has teeth without arming the hazard*

A check that passes because it found nothing is worthless, so a new guard must be made
to fail once. **But not by creating the real hazard in the real artefact.** A leak
detector was verified by planting a real secret in a live file and reverting; the revert
worked, but that file sits in a public repo, so for the duration the thing being guarded
against was actually true. An auditing session asked to confirm the same check declined
to edit the live file and reimplemented the match in a scratch directory instead — same
evidence, no window.

**Do differently:** test guards against irreversible outcomes in a scratch copy. The
rule generalises to deletion guards and spend limits.

### Added — `multi-session-protocol` §4b, *an authorisation that lives only in a transcript cannot be audited*

Outward-facing, irreversible or billable actions need a durable record of what the user
was asked and answered. An auditing session could confirm a published repository
contained nothing sensitive but could not confirm anyone had been asked before
publishing it — and must not be briefed to find out, because being told makes it a
participant. Without a record its only options are to assume or to accuse.

**Do differently:** keep an authorisations file in the project, not only the answer in
the conversation.

## 2026-09-06 — initial publication

Seven skills, generic by construction: three role skills (build, learn, audit), a
coordination protocol, a provenance discipline, a note-taking workflow, and the
maintenance rules that keep the set current. Every rule carries the failure that
produced it.
