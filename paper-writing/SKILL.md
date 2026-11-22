---
name: paper-writing
description: Plan and write a research paper from finished experiments — inventory the experiments and what each one establishes, decide the paper's type (diagnostic/analysis, method, benchmark/evaluation methodology, resource, negative result), survey how published papers of the same topic and type at top venues are structured, borrow that structure, write a narrative plan (claim, abstract moves, section skeleton, how conditions are ranked) and get it approved before drafting. Use before writing or rewriting any paper draft, when asked to "写论文 / 重写 / 改结构 / 设计叙事 / write the paper / restructure the draft", or when a reader says the draft's structure is wrong. Not for reviewing someone else's draft (paper-review), figures (paper-figures). Sentence-level polish is done by academic-humanizer, which this skill runs on every finished draft (§5b).
---

# Paper writing

A draft fails at the structure before it fails at the sentences. The order of work is
fixed: **inventory → type → survey → narrative plan → approval → draft → humanizer pass →
self-check**. Do not open the
.tex file before the plan is approved.

Load `literature-survey` for the survey step and `paper-figures` for figures. When a draft
is written, run it through `academic-humanizer` (§5b) before the self-check. The finished
draft goes to a reviewer under `paper-review`; the writer does not close its own findings.

Evidence for the whole skill: a diagnostic paper was drafted straight from the order in
which its experiments had been run. Its abstract opened with the model configuration, one
configuration was called "our main setting", and the other heads and encoders appeared as
"the same holds beyond". The reader who commissioned the paper stopped at the abstract and
said the conditions should have been equals in a systematic analysis. Nothing in the
numbers was wrong. Several review rounds had checked every number and never asked whether
the structure matched the paper's type.

## 1. Inventory the experiments

Before deciding anything, list every experiment that was run, including failed and
abandoned ones. One row each:

| experiment | question it answers | factor it varies | what is held fixed | result in one sentence | kind of evidence |
|---|---|---|---|---|---|

- **Kind of evidence**: observation · intervention (something was changed and the outcome
  moved) · control or counterfactual · replication on another condition · remedy or
  application · negative result.
- Then group the rows **by question, not by date**. The date order is how the project
  learned things; it is almost never how a reader should.
- Mark which factors were varied systematically (models, heads, datasets, languages,
  metrics). A factor crossed over the whole design is a **dimension of the study**. A
  factor tried once is a robustness check. The two are written differently (§4).
- A paper's theme rests on all the experiments. If an experiment fits no question in the
  list, either the list of questions is wrong or the experiment belongs to another paper.
  Say which; do not leave it as an orphan paragraph.

## 2. Decide the paper's type

The type decides what the reader expects first and what counts as the contribution. Pick
one primary type; a second can be a section, not half the paper.

| type | the contribution is | the reader asks first | conditions (models, datasets) are |
|---|---|---|---|
| **Diagnostic / analysis** | an explanation of a phenomenon, established by tests | what is the phenomenon, how general is it, what causes it, how do you know | the **scope** of the finding: equal cases the claim is tested on |
| **Method** | a procedure that does better | what problem, what is new, how much better, against what | the test bed |
| **Evaluation methodology / benchmark** | a way to measure, or a measurement of the field's practice | what is wrong or unknown about current practice, what should I do instead | the sample the conclusion generalises over |
| **Resource** | data or a tool | what is it, how was it built, what can it be used for | — |
| **Negative result / replication** | a claim that does not hold | which claim, how strong was the test | the conditions under which it fails |

Test for the type: write the one sentence a reader should repeat to a colleague. If it
names a cause, the paper is diagnostic. If it names your system, it is a method paper. If
it tells the reader how to evaluate, it is methodology.

**A wrong type shows up as a wrong first sentence.** A diagnostic paper that opens with
"we use model X with head Y" reads as a method paper with no method.

## 3. Survey the structure of published papers of the same topic and type

Do this for **every** paper, before the plan. Do not write the structure from memory or
from the last paper.

**Start from the structure library, and leave it better than you found it.** The project
keeps one note per paper type (its location is a project fact). Each note holds the papers
read, the per-paper extraction, the templates that recur, the do / do-not rules, and a
dated log of which of our papers used it and what a reader then said about the result.
- If a note for this type exists, read it first. Then survey only what it lacks: papers on
  **this** paper's topic, papers from **this** target venue, and anything newer than the
  note's last update. A type note does not replace the topic and venue reading.
- If no note exists, do the full survey below and create the note.
- After the survey, write the additions into the note the same day. After the paper is
  reviewed or read by its commissioner, add what held up and what was rejected. A template
  that a reader rejected is recorded with the reason, not deleted.

1. Find 8–12 papers: the same **type** at top venues (they may be in another field), and
   the same **topic** at the target venue or its neighbours. Apply the sourcing bar of
   `literature-survey`. Include at least two from the target venue itself.
2. Read the papers, not summaries: title, abstract, introduction, all headings, the first
   sentence of each results subsection, the first figure and table, the conclusion. A paper
   that could not be opened contributes nothing; say so.
3. Extract, per paper:
   - the abstract as a list of moves, one per sentence, and **the subject of the first two
     sentences**;
   - where and how specific models and datasets are named in the abstract;
   - how the introduction is ordered, and whether the main finding is stated there;
   - the section skeleton, verbatim headings, and whether results are organised by
     question, by factor or by experiment;
   - how multiple conditions are treated: parallel equals, or a main case plus
     generalisation;
   - for a causal claim: the chain observation → hypothesis → intervention → alternatives
     ruled out, and where each step sits.
   - **how each section is written, not only where it sits.** For every section record
     what the exemplar writes and what it leaves out, and how long it is:
     - introduction: how the problem is derived (a funnel from a property of the field to
       the paper's question, or a list of prior work and its gaps);
     - related work: a separate section or folded into the introduction; one chained story
       or bold-lead subsections;
     - method: which standard algorithms are written out and which are only named and
       cited; whether results appear in the method;
     - settings: how data, configuration and baselines are laid out (run-in heads, one
       paragraph each), and how hyperparameters and their origin are stated;
     - results: the moves inside a paragraph, how many numbers it carries, and how
       significance is reported (a named test, a p-value, asterisks, or not at all);
     - figure and table captions: a descriptive phrase plus reading keys, or a takeaway;
       whether a legend is used; how the table's row order tells the story;
     - the first figure: what it shows (the phenomenon or the pipeline) and its layout;
     - the conclusion: its moves and how many numbers it carries;
     - the length of each section as a share of the page limit.
4. Synthesise 2–3 templates that actually recur, with the papers behind each. Write the
   result into the structure library's note for this type, with what was read at what
   level; the topic- and venue-specific part can live in the paper's own notes with a link.
5. Choose one template and say why. Borrow **structure** only: the order of moves, the way
   headings are phrased, the way conditions are tabulated, and the per-section conventions
   above. Never borrow wording.

Evidence for the per-section extraction: a short-paper draft was planned from its
exemplars' section skeletons only. Its commissioner then sent it back section by section,
each time with "look at how the exemplars write this": the introduction's derivation of the
problem, the related-work narrative, which standard recursions the method writes out, the
settings layout, results paragraphs, captions, the reporting of significance, the first
figure and the conclusion. Every one of those rounds was answerable from the papers
already read at the survey step.

A delegated survey gets a brief that names the paper type and what to extract, and does
not contain the draft or its claims.

## 4. Write the narrative plan

One page, in the project's notes, before any prose. It contains:

1. **The claim** in one or two sentences, at the strength the evidence gives.
2. **The abstract as moves**, one line per sentence, each line saying what the sentence
   does and which experiment backs it. Use the user's fixed abstract order (user ruling,
   all papers):
   1. **Background, one sentence.** The broad field the paper sits in (for example speech
      emotion recognition), and why the paper's topic matters to that field: its
      importance and meaning in general terms, not the paper's own setup.
   2. **The common problem.** What currently goes wrong on this topic, that is, the
      problem the paper sets out to solve. Name it here if the paper names it.
   3. **Analysis of the problem, optional.** One sentence that dissects the problem, for
      example the competing accounts of it. Leave it out when the paper does not need it.
   4. **What we did and what we found.** Go straight to the study (scope as counts) and
      its results, stated briefly: the key findings only, not every number.
   5. **General contribution, optional.** One sentence on what the work gives the wider
      field beyond its own result. Write it only when there is such a contribution; a
      paper whose result is a diagnosis of one problem ends at step 4.

   Evidence: a diagnostic paper's abstract went through four rounds of the user's own
   edits before it settled into exactly this order; the first draft had no background
   sentence and listed seven findings.

   Once the user has marked an abstract final, propose any later change to it (old
   sentence, new sentence, reason) and wait for approval, including during reviews and
   when new results arrive.

   Then check:
   - the first sentence is about the phenomenon or the field's practice, not about the
     authors' setup;
   - conditions appear as scope ("across N encoders, M heads and K datasets"), after the
     reader knows the question;
   - each finding gets one sentence and at most one or two numbers. The rest of the numbers
     live in tables;
   - cause and remedy are separate moves.
3. **The introduction as moves**, in the user's fixed three-paragraph order (user ruling,
   all papers):
   1. **Background, current work, the challenge.** Open with the field's broad context in
      more detail than the abstract's background sentence. In the same paragraph, say what
      current work looks like and what challenge it faces; that challenge is the one the
      paper sets out to solve.
   2. **How existing work tackles the challenge.** What has been done about it, how, and
      with what results.
   3. **Our work against that background.** What we do, what is the same as and what
      differs from the work in paragraph 2, and what we achieved. A paper that proposes no
      new method (diagnostic, analysis) says what it examines and what it found instead.
      Our own results belong here, not in paragraph 1. Write it as one chain, in this
      order of moves:
      1. **Aim, one sentence.** What the paper does ("This paper examines …" /
         "this paper introduces …").
      2. **Study and first step.** The scope as counts (data, pairs, models), then what we
         did first ("we first …", "first, we analyse …").
      3. **What it revealed.** The finding of that step ("this analysis reveals …", "the
         split shows …").
      4. **The next step, built on it.** Say how the finding leads on ("building on this
         insight, we …", "the cause in turn decides …") and what that step found. Repeat
         3–4 once more if the paper has a further step.
      5. **The end result**, with the one or two numbers that carry it.

      Each sentence follows from the one before. Contributions the user names are woven
      into this chain as its steps. Do not write "our contributions are threefold: first,
      … second, …", do not list numbered research questions or findings, and do not add
      section pointers that break the story.

   Evidence for paragraph 3: the user rejected a numbered findings list, then a
   "contributions are threefold" version of the same content, and pointed to an
   Interspeech 2024 cross-lingual SER paper (Upadhyay, Busso and Lee) as the model. Its
   last introduction paragraph runs: what the paper introduces → which models and data
   it uses → "first, we analyse …" → "this analysis reveals …" → "building upon this
   insight, we implement …" → the result with its UAR. The rewrite in that shape was
   accepted.

   Evidence: a diagnostic paper's introduction put its own grid result at the end of the
   background paragraph; the user then fixed this order for every paper.
4. **The section skeleton**, with each results heading written as the finding it states,
   not as a question (user ruling). Group the results by what they establish (for a
   diagnostic paper: the cause first, then further findings). Map every experiment in
   the inventory to a section. Map every section to at least one experiment.
5. **The standing of conditions.** For each dimension of the study (§1), state that its
   levels are equals and show them side by side: one table or figure with a row or panel
   per level, the same quantities in every row. A baseline may be the reference for
   differences. It is not "the main setting", and other levels are not introduced with
   "also" or "the same holds for".
   - If a result exists for only some levels, the table shows the gap. The text does not
     hide it by reporting only the complete level.
   - If a derivation is exact for one case only (for example a linear model), say that the
     derivation motivates the test and that the **test** is run on every level.
6. **What the first figure and first table are**, since a reader looks there before the
   text. For a diagnostic paper the first figure usually shows the phenomenon and its
   generality, not the pipeline.
7. **What is left out and where it goes** (supplement, another paper).

Get the plan approved by whoever commissioned the paper before drafting. Changing the
plan later is cheap; changing a finished draft's structure is a rewrite.

## 5. Draft

- Write to the plan, section by section. If a section needs content the plan does not
  have, change the plan first.
- Write in full. Compression to a page limit is a separate, later step and belongs to
  whoever owns the page budget. "In full" means **all the content, at the exemplars'
  density** (§3): detail the exemplars leave out (standard recursions, implementation
  constants, secondary analyses) goes into a supplement file from the first draft, not
  into the paper. Typeset in the venue's own template from the first draft, so page counts
  are real. Evidence: a "full" draft for a 4-page venue came out at 11 pages; most of the
  dozen compression rounds that followed removed detail the exemplars never carried, and a
  late template change still threatened the count.
- **Before proposing a change of style, check the exemplars first.** A writer's own idea of
  a better caption or paragraph is a guess until an exemplar confirms it. Evidence: captions
  were rewritten to lead with a takeaway on the writer's initiative; the exemplars use a
  descriptive phrase plus reading keys, and the captions were rewritten again.
- **Internal evidence standards stay internal.** A project may judge its own results by a
  stricter rule than the field (several seeds, cluster-drop checks, agreement across
  losses, a log of test-set use). Every claim in the paper must pass that rule, but the
  paper reports significance the way its exemplars do, in one sentence. Evidence: a draft
  defined its own "established" verdict, three alternative losses and its test-set usage
  count; the exemplars report a p-value or asterisks, and the commissioner asked why the
  paper needed a rule they did not.
- A parallel version in another language keeps its section, equation and table numbers in
  step by script (renumber, then diff the numbering against the main version), not by hand.
  Evidence: a hand renumbering after a deleted section decremented the subsections twice.
- Lead each paragraph with its finding. Cite the table inside the sentence. Do not open
  with "Table X shows".
- State limitations once, in their place. Do not pre-empt objections sentence by sentence.
- Every number comes from a script or an export, never from a hand calculation.
- **Write materials and methods in full, even for methods common in the field.** Every
  condition that could affect a result goes in the text: data selection and sampling,
  label mapping, splits, preprocessing, model and training settings, seeds and how they
  are aggregated, evaluation thresholds and their reasons, resampling for intervals.
  A reviewer who has never used the method will ask about any condition left out, and a
  less patient one will suggest it explains the result. Space pressure moves detail to
  an appendix or supplement; it does not delete it. User ruling, all papers; the user's
  example was a paper that reported the field temperature but not the culture
  temperature.
- **Every formula goes on its own line** (a numbered display equation; one per line, or
  aligned when several define parallel quantities), never inside a running sentence. Only
  single symbols and short names of quantities stay inline (`$h_i$`, `$D$`, `$\kappa_{st}$`),
  and so does a short expression the sentence only mentions without defining anything
  (the user moved a lone `$\log(\pi'_k/\pi_k)$` back into the text).
  Mirror the numbering in any parallel version of the draft (another language). User
  ruling, all papers. Evidence: a method paragraph that defined eight quantities inline
  read as clutter to the user, who asked for every formula to be set on its own line.
- **Each citation must support the exact sentence it sits in, not just its topic.** Check
  the claim against the quote you verified when reading the source (`literature-survey`
  §2). If the source says less, narrow the sentence or move the claim to a source that
  says it; if neither works, drop the sentence. A claim-supporting citation must also
  pass the sourcing bar (`literature-survey` §1): a preprint-only or low-tier source is
  removed with the sentence it supports. Citations that only identify data or tools
  used are exempt, and support nothing.
- **Narrate, do not juxtapose.** Each paragraph makes one point and its first sentence
  states it. Each next sentence is tied to the previous one by an explicit relation
  (cause, contrast, consequence, example, response).
  - Prior work has actors: "X evaluated A and found B", not "Evaluations of A
    established B (X)".
  - Responses to one problem are grouped and contrasted: "Some treated the gap as P and
    did Q; others questioned R and did S", each with what it solved.
  - "Recognised by several studies" or "widely" needs several citations that each say
    it; otherwise write "X noted".
  - A paragraph that changes topic says why the new topic follows.
  Evidence: one introduction paragraph cited a benchmark for a claim it only partly
  made, stated a finding as "evaluations established…" with the authors only in the
  parenthesis, set two responses to the same problem side by side with no contrast, and
  leaned on a preprint. Several review rounds that checked every number had passed it;
  the reader who commissioned the paper found all four in one read.
- **Results paragraphs interpret; they do not list numbers.** Each paragraph runs
  observation → explanation → meaning:
  1. **Observation**: the finding, with the one or two numbers that carry it. Every other
     number stays in the table, and the text does not repeat what the table shows.
  2. **Explanation**: why it happens, as a mechanism ("this may be because…"; "X fails
     because it treats every Y as…"). How the baselines or ablations fail is often the
     best explanation of why the method works.
  3. **Meaning**: what it says about the paper's claim or for practice ("this indicates
     that…", "more importantly…", "so the two components serve different budgets").
  Mark an explanation that was not tested as a reading, or leave it out under space
  pressure; never state it as a finding.
- **The conclusion interprets the contribution; it does not restate the results.** Five
  moves, short, almost no numbers:
  1. what was proposed, in one sentence;
  2. what the results suggest, as a general statement, not a list of outcomes;
  3. why it works, that is, which insight the results support;
  4. the wider meaning for the field ("more broadly…");
  5. future work, one sentence.
  Evidence for both: a draft's results section and conclusion reported every condition's
  numbers and read as a list. The user asked for them to be rewritten after the published
  papers the draft was modelled on (method papers at ICASSP and Interspeech from two
  speech-emotion groups), whose results paragraphs explain each
  observation and whose conclusions state an insight with at most one or two numbers.
- After adding or removing any experiment, redo §1 and §4 and re-read title, abstract,
  introduction, contributions, discussion and conclusion. Inserting a paragraph is not
  enough.

## 5a. Compressing to the page limit

Compression is done by whoever owns the page budget; the writer proposes and applies.
- **Map claims to their support before cutting.** List each kept claim with the sentences
  that support or qualify it. A sentence that is the only support or the only caveat of a
  kept claim cannot be cut alone: cut the claim with it, or keep both. Evidence: a
  robustness paragraph was cut to half a sentence; it carried the caveat that much of one
  dimension's gain came from the reference raters' own offset. Two internal review rounds
  that checked every number missed the loss of the caveat; a venue-standard mock panel
  found it at once.
- Estimate each proposed cut in lines and recheck the page count after every change, with
  floats placed: a table that drifts behind the references still counts against the body.
- Keep every cut, verbatim and in each language version, in a dated backup file, so
  anything can be restored.
- After compression and before the draft is called final, run a mock panel at the target
  venue's standard (`paper-review` §5c).

## 5b. Humanizer pass on every finished draft

When a full draft, or a rewrite of a whole section, is done, load `academic-humanizer`
and run it over the text before §6. Treat this as a fixed step: the user asked for it on
every paper written with this skill.
- It edits wording only. No number, result, citation or claim strength may change. Check
  this afterwards with a script that lists every number before and after the pass.
- If it proposes a change to a claim's strength, treat that as a content question for
  §5 and the plan, not as a wording edit.
- Mirror the edits in any parallel version of the draft (another language, a .tex copy).
- Then run §6 on the result.

## 6. Self-check before handing to review

- Read only the title and abstract. Can a reader say what the phenomenon is, how general
  it is, what the cause or contribution is, and what to do about it, without knowing the
  authors' setup?
- Read only the headings. Do they tell the story in order?
- Search the draft for "main setting", "also holds", "the same holds", "in addition we",
  "we further". Each hit is a place where a parallel dimension may have been demoted.
- Count the numbers in the abstract. More than about eight means findings are being listed
  rather than stated.
- Compare the skeleton with the template chosen in §3 and note every deviation and its
  reason.
- Read the introduction and related work aloud, one paragraph at a time. For each
  sentence, name its relation to the previous one; a sentence with none is a cohesion
  fix (§5). For each citation, say in one line what the source shows and check that the
  sentence claims no more.

## Maintaining this skill

Patch it in the session where a draft's structure was rejected for a reason this text
should have caught (`skill-maintenance`). Templates per paper type belong in the project's
structure library (§3), not here: they are evidence that grows and dates, and this file
holds only the procedure. Project facts (where the library is, venues, which session owns
which draft) belong in the project layer.
