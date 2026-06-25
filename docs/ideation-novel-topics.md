# Novel, Problem-Solving, Determinable Research/Product Ideas

Generated via multi-agent ideation across six domains. Every idea clears three bars:
**(1)** a specific real problem, **(2)** verified novelty vs. closest prior art, and
**(3)** a concrete way to *determine* success — a metric, dataset, or experiment with a pass threshold.

Novelty scores are the generating agent's honest self-rating (1–5).

---

## Best bets (highest novelty × cleanest measurability)

| # | Idea | Domain | Why it stands out | Novelty |
|---|------|--------|-------------------|:---:|
| 1 | **Shallow-Mastery Benchmark** (counterfactual probes detect "right answer, wrong reason") | Education | Audits *correct* answers, which nothing currently does; clean ΔR² test | 5 |
| 2 | **Cache-Stable Speculative Tool Retrieval** | AI agents | Resolves a real KV-cache vs. dynamic-tools tradeoff; token + cache-hit metrics | 4 |
| 3 | **Discrepancy-Pair Med-Rec Benchmark** | Healthcare | Reusable labeled eval for an unmeasured clinical task; macro-F1 leaderboard | 4 |
| 4 | **Eval Flake Budgeter (SPRT for LLM-judge suites)** | AI agents | Adaptive judge sampling under a fixed budget; match N=100 with ≤30% calls | 4 |
| 5 | **FlareScore** (per-flare combustion efficiency from fused satellites) | Climate | Fuses 3 single-signal pipelines into a new metric; validates vs. in-situ DRE | 4 |
| 6 | **Antibiotic CDSS that predicts override, not recommendation** | Healthcare | Targets non-adoption (the real bottleneck); AUROC + alert-reduction test | 4 |

---

## Healthcare & Medical Diagnostics

### Confidence-Gated Cough Triage — abstain instead of guess
- **Problem:** Smartphone cough classifiers (TB/asthma/COVID) silently misclassify hard cases (field sensitivity ~0.70), eroding clinician trust.
- **Novel insight:** Calibrated confidence + *selective abstention*; optimize the risk-coverage curve, route only confident cases, flag the ambiguous middle for lab.
- **Vs prior art:** TBscreen/Swaasa report fixed-threshold ROC-AUC, never abstain. This optimizes coverage at a fixed clinically-acceptable error rate.
- **Determined by:** Risk-coverage curve on Coswara/CoughVID/TBscreen. Pass = ≥90% sens AND ≥90% spec on covered fraction while covering ≥60% of cases.
- **Feasibility:** Public datasets exist; ~3–4 mo. Risk: abstention may concentrate on sickest patients (check via bacterial-load subgroup). **Novelty: 3**

### Discrepancy-Pair Benchmark for Discharge Med-Rec LLMs
- **Problem:** LLM discharge summaries introduce *more* med errors than physicians (2.91 vs 1.82/summary), but there's no standard test of whether an LLM *catches* admission/inpatient/discharge discrepancies.
- **Novel insight:** Synthetic-but-grounded med triples with injected, taxonomy-labeled discrepancies (omission/duplication/dose/interaction); score detection + classification, not generation.
- **Vs prior art:** Existing work predicts high-risk *patients* or generates summaries; no labeled detection benchmark exists.
- **Determined by:** Macro-F1 on held-out split. Pass = off-the-shelf LLM >0.80 macro-F1, beats rule-based hybrid by ≥10 pts; ≥0.7 inter-pharmacist agreement on 100 cases.
- **Feasibility:** Synthesizable from RxNorm + MIMIC-IV; ~3 mo. Risk: synthetic cases too easy (mitigate w/ pharmacist-authored hard cases). **Novelty: 4**

### Negation-Aware ADE Detection — measure the structured-vs-text gap, then close it
- **Problem:** Adverse drug events are underreported in structured codes; they live in notes. No one quantifies *per drug class* how much structured pharmacovigilance misses.
- **Novel insight:** Produce a per-ATC-class "recall gap" metric + a light fine-tuned LM targeting the largest-gap classes (where misses are negation/temporality-sensitive).
- **Vs prior art:** Prior work fine-tunes detectors or notes underreporting qualitatively; this produces a *quantified, drug-class-stratified* gap artifact.
- **Determined by:** On MIMIC-IV/n2c2, recall of structured vs notes per class. Pass = significant ≥20pp gap in ≥3 classes; LM recovers ≥50% text-only ADEs at precision ≥0.7.
- **Feasibility:** n2c2 2018 + MIMIC-IV (credentialed); ~4 mo. Risk: linkage noise. **Novelty: 3**

### Antibiotic CDSS that predicts override, not just recommendation
- **Problem:** Decision-support fails from non-adoption (used in only ~1-in-4 cases). The bottleneck is clinicians ignoring advice, and nobody predicts *when*.
- **Novel insight:** Make clinician override the prediction target; surface a tailored evidence "rebuttal" only for high-override-risk encounters — silent when compliance is likely (cuts alert fatigue).
- **Vs prior art:** All reviewed CDSS optimize recommendation correctness; none model override propensity as the primary objective.
- **Determined by:** AUROC for override on logged interaction data. Pass = ≥0.75; silent-mode A/B cuts alerts ≥40% while retaining ≥90% of corrected prescriptions.
- **Feasibility:** Needs AMS-program interaction logs (access-gated, main blocker); ~5–6 mo. **Novelty: 4**

---

## Climate, Energy & Environment

### FlareScore — combustion efficiency of individual gas flares from public satellites
- **Problem:** Flares are reported as *volume*, but damage depends on *destruction efficiency* (often 80–95%, not the assumed 98%). No public per-flare efficiency metric exists.
- **Novel insight:** Fuse VIIRS Nightfire (radiant heat) + TROPOMI methane anomalies + Sentinel black-carbon plumes per flare into one efficiency proxy.
- **Vs prior art:** EOG/VIIRS volumes and Sentinel-2 BC papers each quantify *one* pollutant; none publishes a fused efficiency ranking.
- **Determined by:** Build score for ~50 flares with published in-situ DRE. Pass = Spearman ≥0.6 vs measured DRE; flag bottom-decile inefficient flares at ≥70% precision.
- **Feasibility:** All inputs free; ~4–6 mo. Risk: TROPOMI ~7km resolution (restrict to isolated flares). **Novelty: 4**

### ChillGuard — refrigerant-leak warning from the compressor's electrical signature (no gas sensor)
- **Problem:** Commercial refrigeration loses 15–25% of charge/year to slow leaks, silently raising energy use 10–20%. Gas sensors are per-unit hardware and detect only after leakage reaches them.
- **Novel insight:** Slow charge loss shifts superheat → compressor load → a drift in the *electrical current signature* already captured by smart meters. Detect via NILM-style change-point detection.
- **Vs prior art:** Detection is acoustic/SAW/subcooling-temp based (sensors at the unit); NILM disaggregates for energy, not faults. Reframes charge loss as electrical-drift detection.
- **Determined by:** Rig staged charge reductions (100%→70%) logging current. Pass = detect 15% loss within 7 days at ≥85% TPR, ≤1 false alarm/unit/month on held-out units.
- **Feasibility:** One instrumented unit or HVAC-lab partner; ~5 mo. Risk: ambient/load confounds (weather-normalize). **Novelty: 4**

### ShadeCast — rank sidewalk segments for tree planting by pedestrian-time-weighted heat exposure
- **Problem:** Cities optimize canopy % over *area*, but pedestrians experience specific segments at specific times. Vulnerable groups get no targeting.
- **Novel insight:** Combine street-level LST + hour-resolved sun/shadow geometry + pedestrian-flow proxies → "person-hours-of-heat-exposure-avoided per tree."
- **Vs prior art:** Recent work optimizes temperature reduction or canopy area; none ranks sites by pedestrian-exposure-weighted benefit.
- **Determined by:** Ranked planting list vs held-out heat-EMS/complaint locations. Pass = top-decile captures ≥2× incident density of canopy-%-ranked baseline (RR ≥2.0).
- **Feasibility:** All open data (Landsat/ECOSTRESS, OSM, GTFS, lidar); ~4 mo. Risk: pedestrian-flow proxy quality. **Novelty: 3**

### GridSlack — auditable carbon-savings receipt for EV/heat-pump charging
- **Problem:** Carbon-aware charging tools forecast *when* to charge but give no trustworthy after-the-fact accounting; naive average-emissions advice can *increase* emissions.
- **Novel insight:** Issue a *settled, auditable counterfactual* — reconstruct marginal-emissions trajectory post-charge, report kg CO2 saved vs. charge-on-plug-in with confidence bounds.
- **Vs prior art:** 2025 papers optimize the *schedule*; none deliver a defensible post-hoc savings *settlement*.
- **Determined by:** Backtest on 2023–25 public MOER + session timestamps. Pass = savings within published MEF uncertainty bounds AND correctly flags emissions-increasing "optimal" advice ≥90% of held-out days.
- **Feasibility:** Public MOER + session logs; ~3 mo (lightest). Risk: marginal-emissions ground truth is itself estimated (report bounds). **Novelty: 3**

---

## AI Agents, LLM Systems & Developer Tooling

### Cache-Stable Speculative Tool Retrieval
- **Problem:** Dynamic just-in-time tool retrieval cuts tokens ~85% but invalidates the KV-cache prefix every time a schema is injected — you get cheap caching OR adaptive tools, not both.
- **Novel insight:** Pre-allocate a fixed-length, append-only "slot table" in the static prefix; bind retrieved tools via a late-position pointer index so new tools *extend* (never mutate) the cached prefix.
- **Vs prior art:** Tool Search / context-engineering retrieve just-in-time but accept cache invalidation or freeze the toolset; none make retrieval grow-only and cache-aligned.
- **Determined by:** τ-bench-style benchmark, 200+ tool registry. Pass = retain ≥70% of just-in-time's token savings, cache-hit ≥90%, task success within 1 pt of all-tools-upfront.
- **Feasibility:** vLLM prefix caching + public benchmark; ~6–8 wk. Risk: slot placeholders may hurt selection (ablate slot count). **Novelty: 4**

### Eval Flake Budgeter — Sequential Testing for LLM-as-Judge Suites
- **Problem:** Judge outputs are flaky; N=1 gives false regressions, N=100/item is slow/expensive, and nobody knows how many samples each item needs.
- **Novel insight:** Per-item SPRT — sample the judge more on high-variance/near-threshold items, stop early on confident ones, minimizing suite-level verdict variance under a fixed budget.
- **Vs prior art:** Conformal-judge papers quantify uncertainty but sample uniformly; this is adaptive, budget-constrained, CI-aware.
- **Determined by:** Ground-truth judge set (HealthBench/MT-Bench rubrics). Pass = match uniform-N=100 verdict accuracy with ≤30% of judge calls, false-alarm <5%.
- **Feasibility:** Open judges + existing rubrics; ~4–6 wk. Risk: unstable variance at low N (warm-start floor). **Novelty: 4**

### Counterfactual Replay for Multi-Agent Regression
- **Problem:** Change one sub-agent's prompt and you can't tell if a downstream change is from your edit or LLM nondeterminism — regressions are invisible.
- **Novel insight:** Record traces; on a change, deterministically replay every node *except* those causally downstream of the edit (via interaction graph), re-sampling only the affected subtree.
- **Vs prior art:** AgentRR generalizes traces for *reuse*; observability tools *log*. Neither does causal-graph-scoped selective re-execution for attribution.
- **Determined by:** 3–5 agent crew, inject known regressions vs harmless edits. Pass = ≥85% attribution precision/recall with ≥40% fewer LLM calls than full re-run.
- **Feasibility:** AutoGen/LangGraph expose graphs; ~8–10 wk. Risk: shared state breaks the cut (conservative taint-tracking). **Novelty: 4**

### Tool-Output Groundedness Gate via Schema-Typed Provenance
- **Problem:** Agents assert structured facts no tool returned (invented field values/rows). LLM-judge guardrails are expensive and themselves hallucinate.
- **Novel insight:** Tag every emitted value with a provenance pointer to the exact span/field of a tool response (typed by the tool's output schema); a cheap deterministic verifier fails any structured claim lacking a valid pointer — no second LLM.
- **Vs prior art:** GSAR/StackAI use an LLM to classify groundedness; this is deterministic, schema-anchored, for structured outputs.
- **Determined by:** Agent benchmark with known structured records + injected fabrications. Pass = ≥90% recall on fabricated facts at <10% the cost/latency of an LLM-judge, FP <5%.
- **Feasibility:** Instrumented tool wrapper + JSON-API tasks; ~6 wk. Scope: structured outputs only (the differentiator). **Novelty: 4**

---

## Education & Learning Science

### Shallow-Mastery Benchmark — students who answer right for wrong reasons
- **Problem:** Items marked correct via guessing/surface heuristics inflate knowledge-tracing and cause premature advancement. No standard measure of how often correct answers rest on wrong reasoning.
- **Novel insight:** Pair each item with an auto-generated *counterfactual probe* (near-isomorph that breaks the surface heuristic but preserves the principle); true mastery stays correct, shallow flips. Flip-rate = shallow-mastery index.
- **Vs prior art:** Prior work models *wrong* answers or correctness probability; none audits whether *correct* answers are well-founded.
- **Determined by:** Item/probe-pair benchmark on a STEM unit. Pass = adding the index improves delayed-exam prediction (ΔR² ≥0.05) over correctness-only KT, n≈300; probes ≥85% principle-preserving by experts.
- **Feasibility:** Existing item banks + LLM isomorph generation; moderate. Risk: generating probes that isolate the principle without changing difficulty. **Novelty: 5**

### Productive struggle vs. flailing — real-time detector from process signals
- **Problem:** Tutors can't tell desirable-difficulty struggle (effortful + learning) from flailing (effortful + nothing). Intervening wrong kills retention or breeds frustration; ITS use only correctness/hints.
- **Novel insight:** Classify struggle *quality* from process telemetry (keystroke timing, edit-undo churn, re-read patterns, dwell entropy); scaffold only on "flailing."
- **Vs prior art:** Desirable-difficulty theory states the goal but gives no measurable signal; knowledge tracing predicts mastery, not struggle type.
- **Determined by:** Label logged sessions by post-hoc transfer outcome. Pass = AUC ≥0.80 predicting struggle type *before* resolution; then RCT (n≈200) +0.3 SD on 1-week transfer with no added frustration.
- **Feasibility:** PSLC DataShop / code-edit logs; moderate. Risk: cross-domain generalization. **Novelty: 4**

### Explanation-quality-gated worked-example fading
- **Problem:** Adaptive fading triggers on *correctness*, which lags understanding and over-supports students who solve mechanically without grasping the principle.
- **Novel insight:** LLM-score the *quality* of each step's self-explanation (cites governing principle vs. restates procedure); fade only when explanation quality crosses threshold.
- **Vs prior art:** Salden's adaptive fading used correctness-based estimates; self-explanation is studied as a static intervention, not as the fading *control signal*.
- **Determined by:** 3-arm RCT (fixed / correctness-adaptive / explanation-adaptive), n≈150–240. Pass = explanation arm beats correctness-adaptive by ≥0.25 SD on 1-week transfer; LLM scoring κ ≥0.7 vs humans.
- **Feasibility:** Existing tutor + cheap LLM scoring; moderate. Risk: LLMs weak at detecting flawed reasoning (human calibration). **Novelty: 4**

### Misconception-targeted interleaving
- **Problem:** Interleaving works by forcing discrimination, but current systems shuffle types randomly — wasting contrast on pairs the student already separates.
- **Novel insight:** Infer each student's *confusion graph* (which types they conflate, from error patterns) and schedule interleaving to juxtapose their specific confusable pairs.
- **Vs prior art:** Interleaving uses fixed/random sequences; misconception-aware systems target recall scheduling, not interleaving *order* for discrimination.
- **Determined by:** RCT random vs confusion-targeted, equal practice. Pass = +0.25 SD on transfer AND a steeper drop in category-confusion errors (significant interaction); confusion graph validated vs diagnostic pretest.
- **Feasibility:** Needs categorizable item bank; moderate. Risk: enough items/type to estimate confusions. **Novelty: 4**

---

## Finance, Fintech & Fraud/Security

### Pre-send "coercion window" detector for authorized push payments
- **Problem:** Scam-coached transfers (~$442B 2025 losses) are authenticated, so transaction-risk models miss them. Banks score the payee/device, not the victim's manipulated intent.
- **Novel insight:** Build a "coercion signature" from in-session telemetry (hesitation latency, paste-vs-typed payee, remote-tool presence, time-of-day deviation, abandon-retry loops) to classify the *user's state*; add friction only under predicted duress.
- **Vs prior art:** Behavioral-biometrics vendors score device/transaction; LLM scam detection needs *call audio*. This is audio-free, on the bank's own session signals.
- **Determined by:** Synthetic/replayed coached-vs-normal sessions. Pass = recall ≥0.80 on coerced transfers at ≤2% added friction on legit; beat transaction-only baseline by ≥15pp at matched FPR.
- **Feasibility:** No public victim-session data → simulate (sim-to-real gap is the risk); moderate. **Novelty: 4**

### LLM fee-audit agent — reconcile disclosed terms vs. actual statement charges
- **Problem:** Surprise/junk fees (overdraft, NSF, APR drift) are hard to cross-check against multi-page disclosures. Tools detect anomalous *spend*, not contract-vs-charge mismatch.
- **Novel insight:** Ingest the account's terms doc → structured fee schedule → reconcile each statement charge, flagging undisclosed/miscalculated/over-cap fees with a dispute-ready citation.
- **Vs prior art:** Enterprise AI scans filings for *bank-side* compliance; no consumer-side per-charge reconciliation against *your* terms.
- **Determined by:** Labeled (disclosure, statement, injected-error) triples. Pass = precision ≥0.90 (low false accusations), recall ≥0.75, correct citation in ≥90% of true positives.
- **Feasibility:** Reg DD docs public, statements synthesizable; low-moderate. Risk: long-context extraction errors. **Novelty: 4**

### Temporal "dormancy-to-burst" detector for synthetic-identity bust-out
- **Problem:** Synthetic identities pass KYC, build credit for months, then bust out. Onboarding models see a clean file; even graph-at-open misses the slow burn.
- **Novel insight:** Model each account's credit-building *trajectory shape* (utilization ramp, micro-payment cadence, cross-institution application velocity, tradeline stacking) and flag curves matching bust-out templates *before* the burst.
- **Vs prior art:** Graph ML finds shared infra; imbalance work targets account opening. This is lifecycle time-series classification on the pre-burst window.
- **Determined by:** Synthetic genuine-vs-synthetic growth sequences. Pass = ≥30-day median lead time at recall ≥0.70, 1% FPR on genuine credit-builders.
- **Feasibility:** Real data proprietary → simulate; moderate. Risk: genuine thin-file customers look similar (fairness). **Novelty: 3**

### SMB "survival runway" forecaster with counterfactual action prescriptions
- **Problem:** SMBs fail on cash *timing*. Forecasters draw a cash-flow line but don't say *which lever* most extends runway.
- **Novel insight:** From open-banking streams, predict per-counterparty payment timing, simulate runway, then run counterfactual optimization over controllable actions → ranked, dollar-quantified moves.
- **Vs prior art:** Existing tools forecast curves; this adds a prescriptive layer over per-counterparty timing models.
- **Determined by:** Backtest on Berka/synthetic open-banking. Pass = per-counterparty timing MAE beats "pays on due date" by ≥25%; prescribed action extends simulated runway in ≥70% of held-out scenarios.
- **Feasibility:** Public datasets exist; moderate. Risk: sparse counterparty history. **Novelty: 3**

---

## Accessibility & Assistive Technology

### Repair-aware AAC for dysarthria — conversational breakdown recovery
- **Problem:** Dysarthric speech is understood only intermittently; the real cost is the *repair loop* (re-spelling the same misheard word). Personalized ASR optimizes single-utterance WER and ignores the aftermath.
- **Novel insight:** Model conversation-level repair — on a confidence dip, offer escalating error-aware fallbacks (phonetic-neighbor list, "spell the hard syllable only," partner confirmation card) chosen to make the *next* attempt succeed.
- **Vs prior art:** MetaICL personalization / SpeakFaster cut keystrokes per utterance; none model multi-turn repair success conditioned on failure type.
- **Determined by:** Wizard-of-Oz + live study, N=8–12. Pass = ≥30% fewer repair turns and ≥15pt gain in successful-message rate vs re-prompt baseline.
- **Feasibility:** Speech Accessibility Project audio for offline sim; live study is the cost. Risk: small N, scripting naturalistic breakdowns. **Novelty: 4**

### Spatial-layout "mental map" mode for screen readers
- **Problem:** Screen readers linearize the DOM, losing 2D relationships (which label sits next to which field, dashboard grid structure) → form errors and disorientation.
- **Novel insight:** Generate an on-demand, queryable spatial model from rendered geometry + a11y tree ("what's right of the submit button?", "describe this as a grid").
- **Vs prior art:** Conversational screen readers answer *content*; landmarks give structure but not relative 2D position. This is spatial-relational, grounded in bounding boxes.
- **Determined by:** Task study, N=10–15. Pass = ≥80% spatial-question accuracy and a significant drop in form errors/time vs standard screen reader.
- **Feasibility:** Browser extension over a11y tree + getBoundingClientRect; no new dataset. Risk: canvas/dynamic pages expose poor geometry. **Novelty: 4**

### Pre-overload "runway" cue for autistic adults (self-directed)
- **Problem:** Predictive overload wearables target *children* and alert *caregivers* — useless for an independent adult who wants private warning with actionable lead time.
- **Novel insight:** Personalized physiological model (EDA/HR/temp/motion) outputs a calibrated *time-to-overload* ("~4 min runway") delivered privately, optimized for lead time and low false-alarm burden.
- **Vs prior art:** Existing systems classify meltdowns post-hoc and alert others, mostly in youth. This is adult self-agency + a lead-time target.
- **Determined by:** In-the-wild study, N=10–15, EMA ground truth. Pass = ≥60% of episodes flagged with ≥3 min lead time at ≤1 false alarm/day.
- **Feasibility:** Empatica-class wearable + seed datasets. Risk: high individual variability, self-report ground truth. **Novelty: 3**

### Latency-budgeted tactile-ASL relay for deaf-blind conversation
- **Problem:** Tactile-ASL users depend on scarce interpreters. Existing haptic devices render the alphabet at fingerspelling speed — too slow for turn-taking, so never tested as real conversation tools.
- **Novel insight:** Treat it as a *latency-budgeted relay*: compress speech into Pro-Tactile-style chunked signals (sign-level/contracted units + backchannels) sized to fit a real turn's time budget, not letter-by-letter.
- **Vs prior art:** TATUM / Braille gloves render 26 letters with no conversational-rate metrics. This optimizes end-to-end turn latency + chunked output.
- **Determined by:** Comprehension study, N=5–8. Pass = ≥2× throughput over fingerspelling, median latency <2–3 s/turn, ≥85% comprehension.
- **Feasibility:** Reuse actuator hardware; main work is chunked encoding. Risk: tiny population, learning-curve confound. **Novelty: 4**

---

*Continue any thread:* each domain agent is still live — its `agentId` can be resumed to go deeper, generate more ideas in that domain, or pressure-test feasibility.
