---
name: publishing-to-github
description: Publishing a repository and keeping it updated — the checks that must happen before a first public push because publication is irreversible, and the rule that every subsequent push is explained: what changed, why, what it means for anyone using it, and what they need to re-read. Load when asked to push something to GitHub, create a repo, publish a directory, or update one that is already published.
---

# Publishing, and explaining every update

## 1. Before the first push, because it cannot be undone

Public means indexed and cached. Deleting the repo later does not retract what was
already fetched. Treat the first push as one-way.

1. **Ask about visibility explicitly, and say that public is irreversible.** Never
   infer it from "push this to GitHub". Repo name too if it will be public.
2. **Sweep everything that will be published** — every file, the README, *and the
   commit messages* — for identity, credentials, private paths, client or employer
   names, and unpublished results. Commit messages are published and are routinely
   forgotten.
3. **Sweep the history, not just the working tree.** A file deleted in a later commit
   is still in the earlier one. If history contains anything sensitive, squash or start
   a fresh repo rather than pushing and rewriting after.
4. **Write down that permission was given** — what was asked, what was answered, when.
   An authorisation that exists only in the conversation cannot be verified by anyone
   later, including an auditing session that must not be briefed to find out.
5. Decide on a licence before, not after, other people start using it.

## 2. Every push is explained

**A push that lands without an explanation is a silent change to something other people
load.** Three audiences, three forms, all of them required:

**To the user, in plain language, at push time.** Not a diff. What changed, why it
changed, and *what they should do differently* — a new rule to follow, a claim to stop
repeating, a file to re-read. If the answer is "nothing changes for you", say that.

**In the repo, as a dated entry** in `CHANGELOG.md`. This is the only form that reaches
someone who uses the repo without talking to you. Newest first.

**In the commit message, standing alone.** Someone reading `git log` in six months has
no conversation to consult. State what was wrong, not only what is now right.

## 3. Classify the change, because the kinds are not equivalent

| kind | what the explanation must include |
|---|---|
| **Added** | what it now covers that it did not, and what prompted it |
| **Corrected** | **what the old version said, quoted**, why it was wrong, and who acted on it in between |
| **Retracted** | the claim being withdrawn, everywhere it may still be repeated, and a direct notice to anyone who relied on it |
| **Removed** | why it stopped earning its place, and where its content went if it moved |

A correction that only describes the new state is not a correction — the reader cannot
tell whether the thing they remember is still true. **Quote the old wording.**

## 4. When the repo *is* the live directory

Publishing a directory that something loads at runtime — a skills folder, a config
directory, a dotfile tree — is the right design, because a copy drifts from the original
and no one can tell which is authoritative. It has one consequence worth naming:

**Every edit is one `git push` from being public.** Any guard against publishing the
wrong thing becomes a pre-push gate rather than a nice-to-have, and it must run against
the live directory, not a snapshot.

And when verifying such a guard, do not create the hazard in the live directory to test
it. Plant the trigger in a scratch copy instead — the revert usually works, and "usually"
is not the standard for something irreversible.

## 5. Mechanics that waste time otherwise

- `gh auth login` authenticates `gh` but does **not** configure git's credential helper.
  The first `git push` then fails with *"could not read Username"*. Run `gh auth setup-git`.
- `gh repo create <name> --public --source=. --remote=origin --push` does creation,
  remote and first push in one step. Rename the branch to `main` first.
- **Verify what actually landed**, not what you intended to send: list the remote tree
  and confirm local `HEAD` equals `origin/main`. A commit that exists locally is not
  published.

## 6. Do not push

Anything whose licence you have not checked — derived data from third-party datasets is
a legal question, not a technical one, and different sources in the same project often
carry different terms. Unpublished results that would scoop the author. Anything the
user has not been asked about.

## Maintaining this skill

Patch it in the session where a publication step went wrong. Anything that needs a
specific project's nouns belongs in that project's own layer, not here. Full discipline:
`skill-maintenance`.
