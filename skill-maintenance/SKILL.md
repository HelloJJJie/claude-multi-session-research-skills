---
name: skill-maintenance
description: How to keep a personal skill alive across projects — when to patch it after a project hits a pitfall it failed to prevent, how to tell whether a lesson belongs in the reusable skill or in the project's own layer, how to port a skill set to a new project without dragging the old project's facts along, and the ways editing a skill breaks it. Load when about to edit or create a skill, when setting up a skill set for a new project, or right after a mistake a skill should have prevented.
---

# Keeping skills alive across projects

A personal skill is the accumulated cost of past mistakes. It is worth nothing if it
only describes the project it was born in, and worth nothing if it stops being edited.

## 1. Patch in the same session as the mistake

**When a project hits a pitfall the current text did not prevent, fix the file before
the session ends** — not "later", not "if there's time". A lesson that stays in one
conversation is lost, and the next project pays for it again.

Patch when any of these happened *and* the text did not prevent it:

- you asserted something unsupported and had to retract it;
- your work, or a peer's report, was wrong in a way a rule would have caught;
- the user pushed back and turned out to be right;
- a boundary was crossed or nearly crossed;
- a procedure here was followed and still produced the wrong outcome, or turned out to
  be ambiguous in a real case.

**Do not patch** for one-off project trivia, or for something already covered — re-read
the relevant section before adding anything.

## 2. Which layer does the lesson belong in

| | goes in the **generic** skill | goes in the **project** layer |
|---|---|---|
| test | would it have helped a project with entirely different subject matter? | is it only true of these files, these formats, these datasets? |
| example | "a cache keyed by an id two datasets can share is read wrongly while the output looks normal" | "this corpus ships no transcripts" |

If you cannot state the rule without the project's nouns, it is a project fact. If you
can, strip the nouns — **but keep one concrete failure as its evidence**. Abstract
lessons do not stick: *"a wait-loop whose `ps | grep` matched the shell running it"* is
remembered, *"avoid self-referential checks"* is not. Keep the mechanism, drop the
identifiers.

**Record the reason, not only the rule.** A rule without its reason is deleted by the
next person who finds it inconvenient, and they will be right to, because they cannot
see what it was buying.

## 3. Porting a skill set to a new project

1. **Copy nothing.** The generic skills are already personal and global; a new project
   just loads them. Copying creates a second copy that drifts.
2. **Write the project layer fresh.** One file, holding only what is true of this
   project: who owns which paths, the formats, the quirks worth not re-deriving, the
   traps this repo has already sprung. Do not seed it from the previous project's
   version — you will import facts that are false here, and they read as authoritative.
3. **State the load order** in the project's `CLAUDE.md`, so a fresh session gets it
   without being told.
4. **Carry the failure modes, not the findings.** "Two runs agreeing to four decimal
   places is a symptom" travels. "Delta was −0.0055" does not.
5. Expect the new project to expose gaps immediately. That is the point: a second
   project is the only real test of whether a rule was general or just familiar.

## 4. How editing a skill breaks it

- **A rule's premise can be silently invalidated by moving it.** The text is not edited
  and still reads correctly; what changed is the fact that made it true. "A repo skill
  beats a personal skill" was right while the coordination rules lived in the repo and
  became an inversion the moment they moved out. **Re-reading the new file cannot catch
  this — the new file is self-consistent.** When relocating a rule, ask of each sentence
  *"is this true because of where it currently sits?"*, and diff against the prior text
  rather than reviewing only the result.
- **What goes missing in a restructure is rarely the headline rule.** It is a qualifying
  caveat — the sentence that said the record is not proof, the clause that said read the
  log *first*. Diff section by section, not by impression.
- **The same clause in several files drifts.** Prefer one home and pointers. If a clause
  must be duplicated, say in each copy where the others are.
- **A cross-reference outlives what it points at.** Renaming or splitting a skill leaves
  references that read perfectly. Grep for the old name across every file that could
  mention it, including project docs and other skills.
- **Growth is a failure mode.** A skill nobody reads to the end protects nothing. When
  adding, check whether the new rule subsumes an existing one and merge rather than
  append; when a section stops earning its length, cut it.

## 5. Style that survives

Imperative, specific, project-agnostic. One concrete failure per rule, one line, not a
story. Tables where a rule has cases. No advice that would be followed anyway.

Every rule should be falsifiable by a future session: it should be possible to say
"I followed this and it produced the wrong outcome" — which is exactly the trigger in §1.
