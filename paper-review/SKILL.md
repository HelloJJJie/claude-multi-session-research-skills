---
name: paper-review
description: Review a paper draft written by another session (or by the user) before submission — judge context (importance, novelty, gap, venue scope), trace every number, term, method claim and citation to its source, check for missing key references and unmet section duties, map each central claim to the evidence that supports it, check methods and statistics in a fixed order, and report findings with location, evidence and a bounded requested action; for venue choice or acceptance odds, run an independent 3-reviewer + editor mock panel (§5c). Use when asked to review / 审稿 / 审核 / mock-review a draft, "check the numbers in the paper", "verify the citations", "投哪里 / 能中吗 / 开个独立审稿人", for a design-stage review of a new line's first spec ("设计审稿 / 设计阶段审稿", §5d), or when acting as the project's paper reviewer. Not for writing or editing the draft — the reviewer reports, the writer fixes.
---

# Paper review

You review a draft someone else wrote. The draft's author fixes; you find, report and
later confirm. Load `multi-session-protocol` first when sessions share the project; this
skill sits next to `project-audit`, which owns checking claims against the filesystem in
general, and `literature-survey`, which owns how a cited source is found and read.

Every number, method claim and citation in the paper must trace to a result file, code,
config or a cited source. A finding without a location and evidence is not a finding.

**The bar is a demanding journal's, whatever the venue** — a workshop or 4-page conference
draft is reviewed as a journal submission would be, unless the user names a paper that is
exempt for now. Record the bar used in the report header. Venue-specific rulings from the user
(e.g. no limitations section) still hold; the bar sets severity, not page budget.

**Field checklist.** When the draft is in affective computing or speech emotion recognition,
load `references/affective-computing.md` and answer every item in the report's Field
checklist row. For another field, write the equivalent list first (from the venue's author
guidelines, its editorials and the field's evaluation-protocol papers) and keep it next to
this file.

## 0. Before judging: read cold, map neutrally

Read the whole draft, including captions, footnotes and the bib, **before** reading the
writer's notes, handoff files or chat about it. Their framing is what you are checking.

Write a short neutral map first, no verdicts: research question · unit of inference
(utterance? speaker? corpus?) · design · comparator/baseline · outcome metric · the
3–5 principal claims, each with its location. Most serious findings are a mismatch
between two lines of this map.

Also record what material you **actually have** (draft, exports, code, cited PDFs) and
what is missing. Never infer absent content — write "not reported" or "not available".

Then judge **context** before details, because editors reject on it before reading the
evidence:
- Is the question important to the venue's readers?
- What exactly is new: the question, the setting, the combination, or the result?
- Does the stated gap hold against a search, not just against the bib? Was the search
  wide enough for the gap's wording?
- Does the paper fit the venue's stated scope? Quote the call for papers.
- Novelty and significance are two tests: name what is new *and* what it changes for the
  field. A new result with little consequence fails the second.
- Read the abstract and the first figure alone, as an editor screening for desk rejection
  does: do they show what is new, the main numbers and why it matters?

A weak context is a finding in its own right (MEDIUM, or HIGH if it makes the paper
unsuitable for the venue). Prose and evidence fixes do not repair it.

## 1. Trace — six independent checks

Run all six by default; they can go to parallel subagents, one check each, given file
paths rather than summaries.

| check | do | typical failure |
|---|---|---|
| **Numbers** | Every number in text, tables, figures, abstract: extract with its sentence → find its source file → compare **exactly** → record a row in the numerical audit table. | stale value from an earlier run; % vs fraction; rounding differs between text and table; count off by one; abstract not updated after the table was |
| **Denominators** | Reconcile N across abstract, method, table, figure, text (items, speakers, corpora, cells, conditions). | subset silently dropped in one place only |
| **Terms** | Every defined term: search all sections. | same thing with two names; one name meaning two things; defined never used / used never defined |
| **Method text vs code/config** | Each described step, hyperparameter, architecture, preprocessing → the code or config that ran. | paper describes the idealised version; a hyperparameter changed mid-project, text did not |
| **Formulas vs code** | Map each operation: summation bounds ↔ loop bounds, mean vs sum, normalisation factor, what is estimated on which split. | the equation normalises per class, the code per corpus |
| **Citations** | Each `\cite`: bib metadata vs DBLP / Crossref / the publisher page (authors, exact published title, venue, year; cite the published version over the preprint). For any cited **number**, locate the table or sentence in the cited PDF; for a cited **claim**, check that the source supports the exact sentence, not only its topic. If the source does not make the claim, give the narrowed replacement sentence or move the claim to a source that makes it. | preprint title on a published paper; number quoted from a secondary summary; source says something weaker |
| **Eval integrity** | Read the evaluation path: split construction, tuning set vs test set, metric aggregation (macro/micro, UAR vs accuracy), prediction–label alignment. Rerun if cheap. | normalisation statistics or thresholds fit on the evaluation data; speaker overlap across splits |

**Grade every cited source against the sourcing bar** (`literature-survey` §1: top venue *and*
strong author provenance; resolve both from a bibliographic API, not from the bib entry). Split
the references into **claim-supporting** (Intro, related work, method justification, anything
an argument rests on) and **attribution** (the corpus, tool or model actually used). A
claim-supporting reference below the bar is a finding: MEDIUM, or HIGH when a central claim
rests on it. Attributions are exempt, because the paper must cite what it used, whatever the
venue; list them but do not chase them. Evidence: a first review checked bib metadata and two
cited claims and never asked whether the sources were authoritative; the user had to ask.

Search the web with the **cited work's title**, never with sentences from the draft.

**Completeness: check what is not cited, not only what is.** For each principal claim and
each method, ask which classic work and which recent key work (last ~3 years) a referee in
the field would expect.
- Look first in the project's own literature notes and earlier surveys, then search by
  topic.
- A missing nearest-neighbour paper is MEDIUM. Examples: the closest prior work in the
  same field, or the published origin of a result the draft presents as its own finding.
- A reference that supports nothing ("decorative") is a deletion candidate.

Evidence: a draft went through several citation rounds that each checked only the
entries in the bib. Two omissions were found only on a completeness pass:
- the paper that had established the draft's calibration-under-shift result;
- the closest same-field study.
The project's own literature notes had read both in full.

A writer's verification log checks metadata, not support. For a named metric, method or
abbreviation, find the name in the cited text itself; it often comes from a neighbouring paper
by the same authors. Do the same for each qualifier the draft adds to a cited finding ("on frozen
representations", "introduced"). Evidence: a draft passed several citation rounds while citing
AURC to a 2017 paper that never defines it (the 2019 follow-up does). The same draft credited
a corpus abbreviation to a paper that never uses it, and said "frozen" about a benchmark that
fine-tuned its models. All three were caught only when the full texts were read.

## 2. Claim → evidence

For each principal claim record: location · the table/figure/analysis that supports it ·
whether direction, magnitude, population and uncertainty all match · the alternative
explanation not ruled out · the bounded remedy. Mismatch patterns to look for:

- causal or mechanistic wording supported only by association or prediction;
- an exploratory or post-hoc analysis presented without that label;
- magnitude or precision overstated, though the direction is right;
- extrapolation beyond what was sampled (acted → natural, these languages → "languages");
- "no effect / equivalent / no difference" from a non-significant result — the interval
  must rule out effects that would matter, or the claim must be narrowed;
- "significant in A, not in B" used as evidence that A and B differ;
- an abstract or conclusion that drops the null results, caveats or the failed arm;
- a novelty claim ("first", "no prior work") broader than the search behind it;
- a rate read as a composition. "75% of class A is kept vs 41% of the others" does not
  mean "most kept items are A" when A is a quarter of the data. Check the base rates.
  Evidence: a principal claim said the retained set was "mostly" the collapse class. A
  reconstruction from the class counts gave about 39%;
- a qualitative word that the paper's own interval contradicts. "Close to the 0.8 target"
  was written while the reported CI excluded 0.8, and "raises confidence" while the CI
  included the reference value;
- a comparator implemented in a simplified form and reported as "the method" fails. The
  draft must name what was left out, or the result reads as a strawman.

**When the paper's contribution is a cause or mechanism, softer wording is not a remedy.**
Ask whether the cause is identified, meaning an intervention or controlled contrast changes
the proposed cause and the effect changes with it. Also ask which alternatives that test
rules out. Then judge whether the answer is at a publishable standard, rather than
whether the wording now matches weak evidence. Hedges on the central claim ("suggests",
"partly", "is consistent with") are a HIGH finding, and the requested action names the
missing identifying test. Narrowing applies only if the author decides the paper becomes
descriptive. Evidence: the user had to tell the reviewer to check whether a
collapse-mechanism paper had actually found and verified its cause, not just hedged it.

**Flag defensive writing as its own finding (MEDIUM; HIGH when it buries the main result).**
It is wording aimed at heading off an objection, not at telling the reader something:
- stacked hedges on a result the data support ("may potentially suggest", "to some extent");
- answers to objections nobody raised ("we do not claim…", "this is not to say…", "it
  should be noted that…");
- the same caveat repeated in the abstract, results and conclusion;
- justifying a method choice at length instead of stating it and citing the reason once;
- saying what the paper does not do before saying what it does.

Test each one: does the evidence support the plain claim? If yes, ask the writer to delete
the hedge and state the claim at the strength the evidence gives. If no, the problem is the
claim, not the wording: report it under the patterns above. Never fix it with a softer
word. Limitations go once, in their own place.

**Flag stiff, juxtaposed narration as its own finding (MEDIUM).** Facts set side by side
without a stated relation read as a list, not an argument. Patterns:
- a finding with no actor ("Evaluations established X (Author)") where the paper's
  argument needs to say who did what;
- several responses to one problem listed one after another instead of grouped and
  contrasted ("some did A to solve P; others questioned Q and did B");
- "widely recognised", "consensus" or "several studies" resting on one citation;
- a paragraph whose first sentence is not its point, or a topic switch with no reason
  given.
The requested action is the rewritten sentences, in order, not "improve the flow".
Evidence: a reader found four of these in one introduction paragraph after several
review rounds had checked every number and passed the prose.

**The reviewer causes this too.** A requested action of "narrow the wording" or "add a
caveat" produces defensive prose. Ask for one exact replacement sentence, or for a test
that settles the question. Evidence: the user told the reviewer to watch for defensive
writing after many rounds of reviews whose remedies were mostly caveats and narrowed
wording.

**When a draft gains an experiment, re-review its framing as a whole.** A paper's theme
must rest on everything it now contains. Reviewing only the inserted passage (the diff
against the previous version) lets the title, abstract, introduction, contribution list,
discussion and conclusion keep describing an earlier, smaller paper.

- List the experiments and results actually in the draft, from its tables, figures and
  result sections.
- Check every framing sentence against that list. Stale scope words are the usual sign:
  one model, one head, one corpus, a count of targets.
- A framing section that no longer matches the evidence is MEDIUM, or HIGH when the
  abstract or contribution list misstates the scope. Ask for the rewritten sentence, not
  an appended clause.
- A pure number correction may be reviewed as a diff. Anything that adds evidence
  triggers the whole-paper pass.

Evidence: a draft gained three more encoders, a non-linear head and two fine-tuned models
over several rounds. Each round the reviewer checked the new paragraph and closed it. The
introduction still said the study used one frozen encoder with a linear head, and the
user found it.

## 2b. Section duties

Read each section for what it owes the reader. A missing duty is usually LOW–MEDIUM.

| section | owes |
|---|---|
| Title / abstract | Title names the core finding or setting. Abstract has background, aim, method, main numbers and conclusion, and keeps the null or negative results. |
| Introduction | Background, the specific question, the gap (bounded by the search behind it), and concrete objectives or contributions that the results section answers one to one. |
| Methods | Why these samples, corpora or cases: inclusion and exclusion criteria. Recruitment and ethics approval for human-subject data, or a data-use/licence statement for public data. How bias and confounding are handled, or that they are not. |
| Results | Every planned analysis reported, including the failed ones. Order follows the stated objectives. Each figure and table stands alone from its caption. No figure that a sentence would replace. |
| Discussion / conclusion | Findings mapped back to the aims. Meaning stated. Limitations: what the design cannot generalise to. Results related to prior literature, both agreement and disagreement. |

The author may decline a duty-level finding, e.g. no limitations section in a 4-page
paper. Record the ruling in the review file and do not raise it again for that draft.

## 3. Methods and statistics, in this order

1. What quantity is being estimated, and on what unit.
2. Design and dependence: repeated items per speaker, per corpus, per run are not
   independent; the uncertainty method must resample the unit that varies.
3. Precision: is there an interval, and what does it range over (seeds? speakers?
   folds?). Do not request observed/post-hoc power; ask for the estimate and interval.
   With few clusters the interval itself is suspect: a cluster bootstrap over n units
   understates variance by about (n−1)/n and has few distinct resamples. Evidence: an
   MDE bootstrapped over 4 speakers was reported as a detection threshold. Subsampling
   4 speakers from large corpora gave larger values, and a first review had passed the
   number without checking the estimator.
   Before endorsing an estimator or a ratio of two estimators as a main readout, work
   out its expected value when the effect is zero. "Biased in a known direction" is not
   enough; you need the size. Evidence: a reviewer accepted the ratio of a two-way
   (pigeonhole) bootstrap variance to a one-way bootstrap variance as the measure of
   extra variance from a second random factor. Under no effect at all, the ratio's
   expected value was about 3, because the two resampling schemes count the residual
   differently. A dry run caught it before the formal run.
4. Multiplicity: name the family (conditions × corpora × metrics) before asking for a
   correction; exploratory labelling is often the right remedy, not Bonferroni.
5. Analysis flexibility: was the analysis fixed before the result it feeds? Are all
   arms reported, including the ones that failed? Before you ask for a test of a group,
   check how the group was chosen. If the outcome picked it, a permutation or
   significance test on the same data is circular. Evidence: a reviewer requested an
   exact permutation test for "the six sources that collapse most". By construction it
   returned the smallest possible p; the build session caught it.
6. Prediction/ML specifics: tuning separated from evaluation; preprocessing fit inside
   the training split only; dataset shift stated; calibration when confidence is used.
   Any prior adjustment (p·π_new/π_old, EM label-shift) must divide by the classifier's
   *implicit* training prior: with class-weighted or balanced training that is uniform, not
   the raw source class proportions. Check the loss before accepting the formula. Evidence: a
   reviewer proposed, and later approved, π_t/π_s for heads trained with class-weighted
   cross-entropy; a cold-read referee caught it.
7. Interpretation and generalisation.

Escalate rather than bluff when a central claim rests on a method outside competence;
say which claim and what expertise.

## 4. Figures and tables

Axes, units, denominators, what the error bars are, consistency with the text, legibility
at the printed size, no colour-only encoding, no truncated axis implying an effect.
The full checklist, including caption duties and the render-and-read loop, is in
`paper-figures`.

## 5. Classify, rank, phrase

Each finding gets a **type** from §5e (correctness · wording · added analysis), a **kind** — not reported · design/analysis problem · demonstrated
inconsistency (two locations or a location and a file disagree) — and a **severity**:

- **HIGH** — wrong number, claim the evidence does not support, citation that does not
  say what it is cited for, eval leak. Must fix before submission.
- **MEDIUM** — terminology drift, unlabelled exploratory result, missing interval, stale
  but close number, defensive writing (§2).
- **LOW** — wording, formatting, optional additions.

Keep **verified** findings apart from **suspected** ones. Every comment has five parts:
**location · observation · evidence (file:line, exact values) · why it matters ·
requested action**. The action is proportionate: give the exact replacement sentence for
an overclaim, label it exploratory, or report the existing analysis before asking for a new
experiment. Do not ask for "a caveat" or "softer wording": that is how a review produces
defensive prose (§2).
A new experiment is requested only when a central claim cannot stand without it.

When your requested action is to cite a number you computed during the review (a base rate, a
paired interval), make the action "add this computation to a script or export with a fixed
seed, then cite that". Do not hand the writer your value to paste in. Evidence: a paired
bootstrap interval the reviewer computed ad hoc went into a draft as 0.006. Re-running it with
a fresh RNG stream gave 0.005, and no file could reproduce either value.

Before requesting a new analysis, check that an identity does not already fix its result.
Subtracting a per-group constant only moves that group's main effect, and a quantity that
cancels out cannot carry an effect. Evidence: a reviewer recommended a "reference-mean"
counterfactual to find out whether the source mean decided which class a model collapsed onto.
The collapse class was already independent of the source mean by identity, and the
counterfactual only subtracted a per-source constant. The writer spotted it; the reviewer
retracted.

Before sending, re-check: every finding has evidence; comparisons are exact; no invented
citation, analysis or reproduction ("I re-derived" only if you actually ran it, with the
command and inputs); placeholders gone.

## 5b. Venue fit and acceptance odds

When the user asks how likely a draft is to be accepted, calibrate against **papers the venue
actually accepted**, preferably from a previous edition run by the same organisers. Download the
versions of record and compare scale, statistical rigour, topic fit and contribution type. State
the limit every time: accepted papers only, with no rejected sample and usually no acceptance
rate. That gives a comparison, not a probability, so do not put a percentage on it.

## 5c. Independent mock panel (3 reviewers + 1 editor)

Use this when the user asks for an objective venue choice, the odds at a venue, or a mock journal
review, and once on every compressed draft before it is called final. Set the standard to the
target venue: a conference panel scores on the venue's form (relevance, novelty, correctness,
clarity, overall, confidence) at the length and tone of a real conference review, not at journal
depth. Evidence: on a compressed 4-page draft, a conference-standard panel found within one pass
a caveat that compression had cut and that two number-checking review rounds had missed. Run it as one fresh subagent. You are not the panel: you have reviewed the draft for
rounds and share the writer's framing.

**Brief: paths, not conclusions.**
- Give the subagent the draft, the result notes and exports, any venue-facts file, and specs of
  planned work, marked as not yet run.
- Give no findings, no headline numbers, no praise from supervisors and no earlier
  recommendation.
- Forbid it to read earlier reviews, fit assessments and venue surveys, and to read other
  sessions' messages.
- Forbid draft text, numbers or findings in web queries. It may search a venue's own pages and
  prior work by title.
- It must not write files.
- Evidence: a first "objective" venue subagent was briefed with the paper's headline result and
  a supervisor's line calling it the more valuable paper. The user asked whether it was really
  independent, and it was only partly so.

**Panel, chosen for the venue.**
- R1 is an insider from the community whose practice the paper touches.
- R2 is a methods or statistics expert.
- R3 is a generalist from the venue's neighbouring discipline. For an affective venue this is an
  emotion scientist, who catches already-known concepts being "rediscovered".
- Each writes a full report: summary, strengths, major and minor concerns tied to locations,
  the evidence that would resolve each, and a recommendation.

**Editor (AE) meta-review, in two scenarios.**
- Scenario A: the current evidence, expanded to the venue's format.
- Scenario B: the planned work completed, with results that do not overturn the current findings.
- For each scenario give the desk-reject risk, the most likely first decision and what would
  sink the paper.
- Then list the 3–5 changes that most improve the odds, in order.

**On odds.**
- No invented percentage.
- Give the venue's published acceptance rate with its source, or "not found".
- Then a qualitative position relative to typical accepted papers (§5b).

**After it returns.**
- Archive the report in the paper's review folder, marked "not yet verified by the reviewer".
- Send it to the writer.
- Verify every factual flag and every new reference before anyone acts on or cites it. A mock
  panel's citations and inconsistency flags are leads, not findings.
- Turn the accepted items into a fixed to-do list for the writer, separating new experiments
  (spec → review → approval) from analyses on existing data and from writing.

## 5d. Design review of a new line's first spec

The rule and its reason are in `multi-session-protocol` §1b. This section says how the
reviewer runs it.

- **Run it as one fresh subagent, not yourself.** You know the project, and a briefed reader
  agrees with the team.
- **Brief: the spec path only.** No motivation, no history, no prior results, no review files.
  If the writer sends a summary with conclusions, send it back and ask for the spec alone.
- **One question:** "If this design produced its best possible result, would a referee buy the
  claim it exists for?" This is not a check of whether the spec is correct; that is your own
  spec review, run separately.
- **Ask the subagent to look for three shapes** that results never reveal:
  - the key contrast is decided in advance by how the arms are built;
  - there is no standard baseline the field would expect;
  - the factor to be separated moves together with a confound.
- **Output:** one page. It must answer "buy / not buy" and, if not, name the design element
  that decides it.
- **After it returns:**
  - Archive the reply in the line's review folder.
  - Verify any factual claim in it before relaying, but do not soften or overrule its
    judgment. The judgment of an unbriefed reader is what the step exists to buy.
  - Send it to the writer before the first run.
  - A design review after the runs does not count.

## 5e. When to stop

A review by a model always finds one more thing. Without a stopping rule the paper never
ships, and each extra analysis added after the results are known weakens the claims it was
meant to protect. These rules were set by the user after a day in which one draft collected
several rounds of "one more analysis" requests.

**Tag every finding with one of three types, next to its severity.**

| type | what it is | what happens |
|---|---|---|
| **correctness** | text and what ran disagree; a wrong number; a claim beyond its evidence; a gate that cannot pass or cannot fail | must be fixed |
| **wording** | strength of phrasing, positioning, order | one round of changes, then stop; do not polish in further rounds |
| **added analysis** | "it would also be good to compute…" | not done by default |

**An added analysis is requested only when the reviewer writes two things with it:**
- which sentence of the paper it would change;
- which result would make that sentence not hold.

If either cannot be written, do not request it. A reviewer who has already requested one
re-reads it against this test and withdraws it when it fails. Evidence: a reviewer asked for a
three-term attribution of a metric difference; no sentence of the planned paper depended on
it, and only one of its three parts (the one that decided whether a specific claim could be
written) survived the test.

**Freezes.**
- Once the user approves the narrative plan, the claims are frozen. After that, only an
  analysis that could overturn a main conclusion is accepted. Supplementary ones wait for a
  journal version or for a real referee.
- An experiment freeze date is set per venue by the user. After it, only text changes.
- A journal draft gets at most two internal review rounds. A round with no correctness
  finding ends internal review; the remaining medium and low items go to the user once, to pick
  from.

**Mock panels (§5c) run once per stage:** once at design, once before submission, none between.

**Ready to submit** means: the pre-submission mock panel says major revision or better, and no
correctness finding is open.

**Each round, give the user the three most important findings only.** The rest go to the writer
to fix or archive; they stay in the review file.

## 6. Report

```
## Review: <paper> — <draft file + its mtime or commit>
Bar applied           journal (default) / exempt per user ruling <date>
Materials checked / not available
Neutral map (question, unit, design, baseline, metric, principal claims)
Context               importance · novelty · gap holds? · venue scope fit
HIGH / MEDIUM / LOW   — each: [check] location · observation · evidence · why · action
Numerical audit       | claim | .tex location | source file | source value | match? |
Claim–evidence table  | claim | location | evidence | aligned? | remedy |
Citations             | key | metadata ok? | claim supported? | source read at what level |
Missing references    | work | why a referee expects it | where it belongs |
Section duties        unmet duties from §2b, or "all met"
Field checklist       each item of references/<field>.md: met · finding ID · n/a + why
Verified (checked and correct)
Could not check, and why
```

Send it to the writing session that owns the draft. **Do not edit the draft.** When the
writer reports a fix, re-check that location against the source yourself and close the
finding only then (multi-session-protocol §3).

## Not adopted from the sources, and why

- A confidentiality/authorisation intake gate (K-Dense): it exists for reviewing other
  people's submissions for a journal. Drafts here belong to the user. Kept only: do not
  put draft text into external search queries.
- An auto-fix step (labrat): the reviewer does not edit; fixing belongs to the writer.
- Accept/reject recommendations: say what the evidence supports; the user decides.

## Provenance

Adapted, not copied, on 2026-11-11 from two MIT-licensed skills:
`fcakyon/labrat` `plugin/skills/paper-check` (commit 0487762) — the six trace checks,
HIGH/MEDIUM/LOW, numerical audit table; `K-Dense-AI/scientific-agent-skills`
`skills/peer-review` + `references/common_issues.md`, `statistical_reproducibility.md`
(commit 2d2065f) — neutral map, claim–evidence mismatch patterns, methods order,
five-part comment, proportionate requests, finding kinds. The context check in §0, the
completeness check in §1 and the section duties in §2b were added on 2026-11-12 from two
review checklists the user supplied. The first was by section: introduction, methods,
results, discussion, title and abstract. The second covered context, quality and
completeness. On 2026-11-15 the user asked for a summary of what makes a strong journal paper,
especially in affective computing, to be applied in every review: the editor-screen items in §0
and `references/affective-computing.md` came from it, with each source's read level recorded
there.

## Maintaining this skill

Patch it in the session where a review missed something this text should have caught —
see `skill-maintenance`. Project facts (where drafts live, which claims are retracted)
go in the project layer, not here.
