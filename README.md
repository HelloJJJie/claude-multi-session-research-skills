# Claude Code skills

A personal skill set for running work across several Claude Code sessions — one that
builds, one that explains, one that audits — and for keeping experimental results
trustworthy enough to cite months later.

These are **not** a framework. Every rule here exists because a specific mistake got
through, and each one carries the failure that produced it. They are written to be
edited: see `skill-maintenance`.

## Two layers

Skills here are **generic** — they should help a project with entirely different
subject matter. Anything that needs a particular project's nouns belongs in that
project's own skill (`<repo>/.claude/skills/<name>/`), not here.

| skill | what it owns |
|---|---|
| **`multi-session-protocol`** | Roles; who owns a claim and its scope; who may close a finding; how a retraction propagates instead of staying alive in documents nobody re-read. Load first when sessions share a project. |
| **`experiment-engineering`** | The build role. Your own pipeline is the first suspect behind a result; decomposing a null against a published configuration; long jobs that survive interruption. |
| **`experiment-provenance`** | Append-only run directories; what a run's metadata must answer; when a result may not be cited; grading design decisions A/B/C; pre-registering before the run. |
| **`research-companion`** | The learn role. Explaining a codebase at the user's pace, graded literature work, re-deriving the builder's numbers before believing them. |
| **`project-audit`** | The audit role. Checking claims against the filesystem rather than against conversation. Never briefed by the sessions it audits. |
| **`study-notes`** | Turning a study session into atomic Obsidian notes with a linking index. |
| **`publishing-to-github`** | Publishing a repo and keeping it updated: the checks before an irreversible first push, and explaining every subsequent one — what changed, why, and what a user should do differently. |
| **`skill-maintenance`** | Keeping all of the above alive: when to patch, which layer a lesson belongs in, how to port the set to a new project, and the ways editing a skill breaks it. |

## Suggested load order

```
build   multi-session-protocol → experiment-engineering → experiment-provenance → <project skill>
learn   multi-session-protocol → research-companion → <project skill>
audit   multi-session-protocol → project-audit → <project skill>
```

State it in the project's `CLAUDE.md` so a fresh session gets it without being told.

## Using them in a new project

Copy nothing — these are global and a copy drifts. Write one project-layer skill holding
only what is true of that project: who owns which paths, the formats, the quirks worth
not re-deriving, the traps that repo has already sprung. Do not seed it from a previous
project's version; you will import facts that are false there and read as authoritative.

Carry the failure modes, not the findings. *"Two runs agreeing to four decimal places is
a symptom"* travels between projects. A specific measured value does not.

Changes to this repo are explained in `CHANGELOG.md`, newest first, with corrections
quoting the wording they replace.

## Where they came from

A speech-research project run across three concurrent sessions. Over one stretch of
work the sessions caught, in each other: numbers reported that existed in no archived
run; a feature cache scored against the wrong audio twice; three summarising claims each
falsified by one row of the table they summarised; a wait loop whose `ps | grep` matched
the shell running it, so jobs were reported as running that had never started; and a
rule whose premise was silently invalidated by moving it to another file.

Each of those produced output that looked entirely normal. That is the whole argument
for the setup, and the reason these files exist.
