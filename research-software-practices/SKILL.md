---
name: research-software-practices
description: Building a research codebase someone else can run — project layout, an explicit environment, tests that would catch a wrong scientific conclusion, code written for humans, a README that states the scientific contract, and one documented command from clone to figures. Load when starting a research project, when a codebase has grown past a pile of scripts, or when a result needs to be reproducible by someone who cannot ask you questions.
---

# A research codebase someone else can run

The skill being practised is not "write Python". It is **making a computational claim
others can verify.**

Two neighbouring skills carry adjacent ground: `experiment-provenance` for recording
individual runs so their numbers stay citable, `experiment-engineering` for diagnosing
whether a result is real. This one is about the codebase itself.

## 1. A project, not a pile of scripts

```
project/
  README.md          what, why, how to run
  requirements.txt   or environment.yml / pyproject.toml, with a lock file
  src/               reusable code, as packages — not one mega-file
  tests/             automated tests
  data/              raw data, or a pointer to where it lives
  scripts/           numbered entry points that call src/
  results/           append-only outputs
  docs/
  .gitignore
```

Keep notebooks thin: exploration only, with the reusable functions in `src/` and
imported. **Never overwrite raw data** — derived files are regenerable, raw files are
not, and one raw source with generated derivatives beats the same data copied into five
folders.

Version control from the first real function. Commit small units, one idea each, and
write messages that say **why**, not only what.

## 2. "It works on my laptop" is not a scientific result

Pin the environment and record the exact command that created it. For a paper, a range
(`numpy>=1.26`) reproduces nothing: today and in six months it resolves differently.
Freeze a lock file alongside the ranges, and say in the README which is authoritative.

When a platform forces an ugly split — two environments because a dependency has no
build for this OS, say — **write down why in the README**. The next person will
otherwise "simplify" it back into a broken state.

## 3. Test what you claim

You do not need full coverage. You need **tests that would catch a wrong scientific
conclusion**. Four layers, cheapest first:

| layer | what it catches |
|---|---|
| **smoke** | the entry point runs end to end on a tiny sample and exits 0 |
| **unit** | numerical kernels, parsers, transforms |
| **property / sanity** | conservation, symmetry, bounds, known limits, degenerate cases |
| **regression** | a small fixture whose output you inspected by hand once |

**The rule that matters most: a verification you ran once by hand and never again is
not a test.** Research projects are full of these — "I checked the padding mask is
correct", "the corpus yields exactly the published count", "the two significance tests
agree on synthetic data". Each is a regression test that costs seconds to run forever,
and each is silently at risk from every later refactor. **When you verify something by
hand and feel pleased about it, that is the moment to make it a test**, not a note
saying it was checked.

Property tests are especially cheap in scientific code, because the science supplies
the properties: a degenerate self-comparison must return zero difference; a confidence
interval must contain the estimate it describes; a padded batch must give the same
answer as the same item alone.

Run them automatically. A green suite is part of the result.

## 4. Write code for humans

- Names say what the thing is (`mean_squared_error`, not `mse2`).
- A function does one thing and returns a value, or raises a clear error.
- Name your constants; no magic numbers.
- Keep modules short. If a file needs a table of contents, split it.
- Use the standard formatter and linter so review discusses ideas, not whitespace.
- **Comment intent and invariants**, not what the next line obviously does. The comment
  that earns its place is the one explaining why a non-obvious choice is correct.

## 5. The README states the scientific contract

It answers: what question does this address; what are the inputs and outputs; how do I
install and run the main experiment; **what are the known limitations** — precision,
assumptions, hardware, what the design cannot show.

Docstrings on public functions state units, shapes and failure modes. That is part of
the science, not paperwork.

## 6. One documented command, from clone to figures

A third party should clone, create the environment, run **one** documented command, and
obtain the figures and tables or a close numerical equivalent. Prefer scripts plus
configuration files over "I clicked these cells in order"; if a notebook is in the path,
run it top to bottom in a clean kernel before sharing.

Being unable to say which script produced Figure 3 is the most common late failure in
scientific software, and the cheapest to prevent.

## 7. Failure modes and the habit that prevents each

| symptom | habit |
|---|---|
| "I can't remember which notebook produced Figure 3." | Script + fixed config + a committed recipe per figure. |
| Silent numerical drift after a "small refactor". | Automated tests and a pinned environment. |
| An 800-line `utils.py`. | Split by responsibility; keep a small public API in one place. |
| The same data copied into five folders. | One raw source plus generated derivatives. |
| "Just run the cells until it works." | A deterministic entry point documented in the README. |
| A hand-check nobody ever repeated. | Make it a test the day you do it. |

## 8. Order of work at the start

Layout and README → git and `.gitignore` → freeze the environment → the smallest
function that encodes the core idea, **plus one test** → a one-command tiny end-to-end
example → *only then* grow the analysis.

That order is what prevents the usual collapse: a good idea sitting in an unrunnable
archive.

## Maintaining this skill

Patch it in the session where one of these practices was missing and cost something.
Anything needing a specific project's nouns belongs in that project's layer, not here.
Full discipline: `skill-maintenance`.
