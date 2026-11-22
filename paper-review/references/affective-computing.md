# Field checklist: affective computing / speech emotion recognition

Load this with `paper-review` whenever the draft is about emotion, affect, sentiment from
speech, face, physiology or text, whatever the target venue. Every item gets a line in the
report's **Field checklist** row: met, finding (with its ID), or not applicable and why.
Silence on an item is not allowed.

The list was compiled on 2026-11-15 at the user's request. Each item names its source and how
far that source was read. Items resting only on search summaries are marked; treat them as
prompts to check, not as rules a referee has stated.

## A. What an editor screens before review (any journal)

1. **Scope fit.** Quote the journal's scope statement against the paper's question. Several
   sources name scope mismatch as the most common desk-rejection reason. *(Publisher/vendor
   guides, read as search summaries only: Manusights, AIScholar, LetPub, CASRAI.)*
2. **Novelty and significance are separate tests.** A new result that does not move the
   problem forward is still rejected. Name both: what is new, and what changes for the field.
   *(Same sources.)*
3. **Abstract and first figure carry the significance.** An editor who cannot see why the
   paper matters from these two may not read the method. The abstract states what is new, the
   main numbers and the null or failed results; the cover letter says the same in two lines.
   *(Same sources.)*
4. Do not cite the "≈52% of desk rejections mention novelty" figure: it is a second-hand
   report of a single study.

## B. What a referee in this field expects

1. **Comparable evaluation protocol.** Report one result in the field's standard setting
   before any variant. On the benchmark a field has converged on, that usually means its
   conventional class merge (e.g. an excitement class folded into happiness to give four
   classes) with speaker-independent cross-validation; then the paper's own variant. Antoniou
   et al. (ICASSP 2023) re-ran open-source SER papers under such a standard protocol, and some
   reported results did not reproduce. *(Full text of §3.3 read.)* Flag a corpus result with no
   standard-setting anchor, and state the corpus and protocol you anchored to.
2. **Speaker independence.** Random utterance splits leak speakers and inflate scores; check
   that every split is speaker- (or session-) disjoint and that the paper says so. *(A 2026
   NeurIPS listing, abstract only, peer-review status not confirmed; consistent with item 1.)*
3. **Label validity.** A single categorical label hides annotator disagreement; say which
   label is used (actor-intended vs perceived majority vote), report agreement, and test
   whether conclusions depend on the label source. Recognising an emotional *expression* is
   not recognising an *emotion* (Gratch, essay read as search summary); claims worded as
   "recognising emotion" invite that objection, so the wording must match what was measured.
   *(Critique papers read as search summaries.)*
4. **Generalisation.** A within-corpus score alone is weak evidence. Expect cross-corpus,
   cross-language or naturalistic-data results, or a stated reason why the claim does not
   need them.
5. **Auditability and confidence.** Benchmarks, uncertainty on reported numbers, stated data
   dependence and explainability are named as field obligations in the closing paragraph of
   Schuller's outgoing-EiC TAC editorial (2019, full text read; the rest of it is journal
   news, not review criteria).
6. **Ethics and data use.** Human-subject data needs an ethics/consent statement or a
   data-use/licence statement for public corpora (§2b already requires this). TAC ran an
   ethics special issue (guest editorial Gratch et al. 2024). Whether TAC *mandates* an ethics
   statement is not verified.

7. **Grounding in emotion science.** TAC's founding editorial (Gratch 2010, DOI
   10.1109/T-AFFC.2010.11, full text read) says authors "can expect to be tested on their
   interdisciplinary knowledge": papers are reviewed from several disciplines, and it names the
   failures a psychologist reviewer catches — ignoring established distinctions, rediscovering
   documented phenomena, poor experimental design. Check that the draft's emotion constructs
   (categories vs dimensions, expression vs experience, intended vs perceived) are named and
   cited to emotion research, not only to ML papers, and that a claimed finding is not a known
   result from psychology.

8. **Good practice as stated by the speech community in TAC.** Batliner, Hantke, Schuller,
   "Ethics and Good Practice in Computational Paralinguistics", TAC 2020 (DOI
   10.1109/TAFFC.2020.3021015; §4 full text read):
   - test a new method on *points of reference*: standard public corpora with reproducible
     partitions, a well-understood baseline (e.g. SVM + an openSMILE feature set), and a
     standard measure (UAR); a new corpus gets the same standard baseline;
   - evaluate on unseen speakers *and* unseen material;
   - an overall UAR hides speakers the model fits well and badly ("sheep and goats"): look at
     the per-speaker spread;
   - report effect sizes and parameter estimates with confidence intervals; argue from effect
     sizes, not p-values alone;
   - outputs should be interpretable; knowledge-based features are a useful reference.

## D. What highly cited recent TAC papers actually look like (the bar in practice)

Top-cited TAC articles 2021–2025 (OpenAlex, 2026-11-15, ranked by citations within each
year): EEG emotion recognition and multimodal sentiment dominate, then facial expression,
surveys and new datasets; speech papers rank lower. Four highly cited speech papers had their
evaluation sections read in full on IEEE Xplore:

| paper | corpora | speaker-independent | variance over runs | significance test | code |
|---|---|---|---|---|---|
| EmoNet, multi-corpus transfer (2021, 10.1109/TAFFC.2021.3135152) | 26 | yes, partitions built | — | yes (Almost Stochastic Order) | GitHub |
| Self-supervised adversarial DA, cross-corpus/language (2022, 10.1109/TAFFC.2022.3167013) | 5 | yes | mean ± SD over 10 runs | none | — |
| Adversarial domain-generalised transformer, cross-corpus (2023, 10.1109/TAFFC.2023.3290795) | 4 (two scripted-acted, one improvised-dyadic, one children's spontaneous) | partly | mean ± SD over 5 runs | none | — |
| Vesper, pretrained SER model (2024, 10.1109/TAFFC.2024.3369726) | 3 (one dyadic benchmark under its conventional four-class merge, one TV-dialogue, one crowd-acted) | yes | none | none ("significant" used informally) | GitHub |

Reading: the accepted bar is speaker-independent evaluation on several public corpora, UAR,
and often public code. Formal uncertainty is rare (1 of 4 tests significance; 2 of 4 report
run-to-run SD; none report speaker-level intervals). This is what the venue *accepts*, not the
review bar: per the journal-standard rule and item 8, a missing interval is still a finding.
It does mean that speaker-level intervals and effect sizes are a point of difference a draft
can claim, not a box every TAC paper already ticks.

## C. Venue facts read at the source

- **IEEE Transactions on Affective Computing, Author Information page** (computer.org, read
  2026-11-15): regular paper 12 double-column pages including references and biographies,
  submissions up to 18 pages with mandatory overlength charges; brief contribution 8;
  comments 2; survey 20, with a cover-letter list of recent surveys and why this one adds;
  supplementary material in separate files, no page limit.

- **Hoey et al., EiC editorial 2026** (DOI 10.1109/TAFFC.2026.3690258, full text read):
  - submissions ~120/month, up from ~30 in early 2023;
  - each paper goes to one of four Associate EiCs, who either picks the handling AE or
    recommends a desk reject; speech/conversation/language/sentiment is one of the four areas;
  - 2022–2025: speech/audio is the largest theme (41% of papers); recognition-oriented work
    outnumbers generation about 5:1;
  - no review criteria are stated.
- **André, EiC editorial 2023** (DOI 10.1109/TAFFC.2023.3236419, full text read): 697
  submissions in 2022. The 2021 best paper and runner-up are an ordinal view of emotion
  (Yannakakis, Cowie, Busso) and cross-corpus SER via domain generalisation (Gideon et al.).
  These show what the board rewards; they are not criteria.
- Gratch "State of the Journal" 2014 (full text read): journal news only.

## Not read / pending

- Ethics guest editorial (Gratch et al. 2024, DOI 10.1109/TAFFC.2023.3322918): abstract only.
- André 2021, 2022; Gratch 2011–2013; Schuller 2016–2018: not read. The editorials read so far
  are journal news, so these are unlikely to state criteria; read them only if a question needs
  them.
