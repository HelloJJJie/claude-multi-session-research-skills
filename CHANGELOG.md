# Changelog

Newest first. Every published change is explained here — what changed, why, and what a
reader who already uses these skills should do differently.

Corrections quote the old wording, because otherwise you cannot tell whether the thing
you remember is still true.

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
