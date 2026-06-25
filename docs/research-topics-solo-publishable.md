# Solo-Feasible, Publishable Research Topics

Generated via multi-agent ideation, scoped hard to: **one researcher, modest compute** (laptop + API, or a single consumer GPU), **public data / open models, no IRB**, aiming for a **publishable paper**. Every idea names its closest prior work and the differentiator, plus a concrete experiment (named dataset, metric, baselines, pass threshold).

Novelty scores are the generating agent's honest self-rating; **scoop-risk flags are theirs too — take them seriously.**

---

## Top bets — highest novelty × lowest scoop-risk × cheapest

These are the ones with a distinct construct, a pre-built or scriptable dataset, novelty ≥4, and the clearest path to a finished paper. Ranked.

| Rank | Idea | Area | Why it wins | Cost | Novelty |
|:---:|------|------|-------------|------|:---:|
| 1 | **NEAR-MISS** — true-but-misreferenced distractor RAG benchmark | NLP/RAG | Genuinely new construct (true distractor about wrong referent ≠ counterfactual/noise); reproducible Wikidata-generated artifact | <$40 + 1 GPU | 4 |
| 2 | **Retrieval-Induced Self-Doubt** — when RAG flips correct answers to wrong | NLP/RAG | Conditions on per-item parametric correctness (nobody does this); prescriptive "when not to retrieve" gate | <$50 + 1 GPU | 4 |
| 3 | **Distractor Injection** — contamination-robust tool-selection stress test | Eval | Pure code, no training; contamination-resistant by construction; clean gap | <$80 API | 4 |
| 4 | **CacheGuard** — measuring silent prompt-cache misses from agent nondeterminism | Multi-agent | Known folklore, never quantified; ships a tool + cross-framework study; ties to this repo's theme | ~$150–300 API | 4 |
| 5 | **AttributeCheap** — cost-bounded failure attribution for multi-agent systems | Multi-agent | Who&When benchmark is pre-built; cost axis is unexplored; small-model-first cascade | ~$100–200 | 4 |
| 6 | **Compression-Robustness Tax** — which capabilities KV-compression silently kills | Efficient ML | Pure measurement, hard to scoop; one capability reversal = the paper | 1 GPU, ~$0 | 4 |

> Pattern worth noting: the strongest solo paper ideas are **measurement / diagnostic** papers — they reframe *what is measured* (a new construct, a conditioned metric, a cost axis) rather than building a big system. That's exactly what a solo researcher on modest compute can win at, because the moat is insight + careful experiment design, not GPUs.

---

## LLM & Agent Evaluation / Reliability

### Order-Robustness as a Leaderboard Metric (Rank Volatility under Permutation)
- **Problem:** MCQ leaderboard scores swing with option/few-shot ordering; adjacent ranks may not be statistically robust, yet a single number is reported.
- **Contribution:** Per-model **RVP** metric = P(rank changes under randomized permutations) + a permutation-marginalized score.
- **Vs prior art:** Zheng et al. 2024 (position bias) measures *accuracy* shifts; this is *leaderboard rank* volatility with a significance test.
- **Experiment:** MMLU/ARC/HellaSwag via MCQ harness; 8–10 open + 2–3 API models; metric RVP + Kendall-τ. Pass = ≥30% of adjacent leaderboard pairs not robust (p>0.05) on ≥1 benchmark.
- **Feasibility:** 7B on 1 GPU/API, <$100, 3–4 wk. **Venue:** EMNLP/ACL Findings, NeurIPS D&B. **Novelty 3.5** (reframing, not new phenomenon).

### CheapJudge-Calibrate — predict-then-route for small vs frontier LLM-judges
- **Problem:** Teams want a cheap local judge but have no per-task decision rule for whether it'll agree with a frontier judge.
- **Contribution:** Tiny-probe-set agreement predictor + selective routing of only disagreement-prone items to the expensive judge; cost-accuracy Pareto.
- **Vs prior art:** Judge meta-eval (2506.13639, JudgeBench) ranks judges; none give a predict-then-route procedure.
- **Experiment:** RewardBench/JudgeBench/MT-Bench/RAGTruth; Qwen2.5-7B & Llama-3.1-8B vs API reference; metric human-agreement κ at fixed budget + predictor AUC. Pass = ≥90% of frontier human-agreement at ≤40% cost, beating random routing.
- **Feasibility:** 1 GPU + <$150, 4–5 wk. **Venue:** EMNLP/ACL Findings, NeurIPS D&B. **Novelty 3.5.**

### Distractor Injection — contamination-robust tool-selection robustness *(TOP BET #3)*
- **Problem:** Tool-calling benchmarks report static, likely-contaminated accuracy; real toolboxes have near-duplicate/decoy tools.
- **Contribution:** Procedural generator injects synonym/overlapping-schema/deprecated-duplicate distractors → **TSR** = accuracy drop per distractor density; contamination-resistant by construction.
- **Vs prior art:** ACE-Bench injects call *failures*; BFCL/τ-bench give static accuracy. None inject semantically-confusable distractor *tools*.
- **Experiment:** BFCL + τ-bench; Qwen2.5-7B, Llama-3.1-8B, Hermes-FC + 2 API; metric TSR at density 0/1/3/5. Pass = ≥15-pt drop at moderate density for ≥half the models + a ranking change vs clean.
- **Feasibility:** 1 GPU, <$80, 4–5 wk. **Venue:** NeurIPS D&B, EMNLP Findings. **Novelty 4.**

### The Self-Consistency Tax — does test-time sampling improve *reliability* or just confidence?
- **Problem:** People pay 5–20× for self-consistency without knowing if it improves calibration/abstention or just inflates confidence.
- **Contribution:** **Reliability-per-Sample** — accuracy, ECE, AURC vs sample count N; find regimes where N↑ improves accuracy but not selective reliability.
- **Vs prior art:** BrowseConf / "Confidence Dichotomy" use/analyze confidence; none isolate the calibration/selective-risk return of self-consistency. **Sharpen vs 2601.07264 before committing.**
- **Experiment:** GSM8K + MATH subset + WebShop/ALFWorld; Qwen2.5-7B, Llama-3.1-8B; metric ECE/AURC/acc@coverage over N∈{1,3,5,10,20}. Pass = confirm ≥1 "tax" regime, give actionable N*.
- **Feasibility:** 1 GPU, <$50, 4–5 wk. **Venue:** COLM, EMNLP Findings. **Novelty 3.5.**

---

## Efficient ML / Inference (single consumer GPU)

### Compression-Robustness Tax — which capabilities KV-compression silently destroys *(TOP BET #6)*
- **Problem:** KV-compression papers report aggregate LongBench; practitioners later find 2-bit KV quietly breaks arithmetic/faithfulness/instruction-following.
- **Contribution:** Capability-decomposed diagnostic + a normalized "robustness tax" comparing method *families* (eviction vs quant vs low-rank) at matched memory.
- **Vs prior art:** "What Must We Give in Return?" (2407.01527) reports task scores, not per-capability deltas across families.
- **Experiment:** RULER + GSM8K + HotpotQA-citation; Llama-3.1-8B/Qwen2.5-7B; compare H2O/KIVI/SnapKV/low-rank at matched memory. Pass = ≥1 statistically significant capability *reversal* (A>B aggregate, B>A on a capability).
- **Feasibility:** pure inference, 1 GPU/API, 5–6 wk. **Venue:** EMNLP/NAACL Findings, NeurIPS D&B. **Novelty 4** (measurement, low scoop-risk).

### Drift-Triggered Recalibration for Streaming KV-Quantization
- **Problem:** KV scales calibrated on the prefix drift during long generation → late-token error in long reasoning/agentic traces.
- **Contribution:** Near-free per-channel drift detector fires localized rescale of only affected channels (<1% overhead).
- **Vs prior art:** KVQuant/KIVI calibrate statically; reviews note drift but offer no cheap streaming fix.
- **Experiment:** long-CoT GSM8K-hard/MATH + AlpacaEval long-form; Qwen2.5-7B/Mistral-7B; accuracy vs output length vs static KIVI-2bit. Pass = recover ≥50% of the 2bit→fp16 gap at >2k tokens, <3% latency overhead.
- **Feasibility:** 1 GPU (16–24GB), no training, 4 wk. **Venue:** ENLSP, EMNLP Findings. **Novelty 3.5.**

### Per-Task KV-Cache Bit-Width Routing via cheap online probes
- **Problem:** A single global KV bit-width is wrong per-request (retrieval QA tolerates 2-bit; multi-hop reasoning collapses).
- **Contribution:** Training-free controller probes first ~16 tokens (attention-entropy/value-norm) → routes per-layer bit-width.
- **Vs prior art:** KVTuner (offline) / MixKVQ (query-aware static); differentiator is a per-request closed-loop probe. **MixKVQ is close — must beat their static policy.**
- **Experiment:** LongBench + RULER on Llama-3.1-8B/Qwen2.5-7B; accuracy vs avg bits/token. Pass = KVTuner accuracy at ≥15% lower KV memory, or +2 LongBench at equal memory.
- **Feasibility:** 1×24GB GPU, no training, 4–6 wk. **Venue:** ENLSP, MLSys poster. **Novelty 3.**

### Semantic-Acceptance Self-Speculative Decoding *(⚠ likely partly scooped)*
- **Problem:** Self-speculation wastes accepted length by rejecting semantically-equivalent drafts.
- **Contribution:** Confidence-gated *semantic* acceptance for self-spec (no draft model) with a calibrated per-token risk bound.
- **Vs prior art:** "Training-Free Loosely Speculative Decoding" (2511.22972) is **dangerously close**; pursue only if the calibrated-bound + self-spec angle is genuinely distinct.
- **Experiment:** HumanEval/GSM8K/MT-Bench on Llama-3.1-8B; speedup vs quality. Pass = ≥1.4× over exact self-spec, <1 pt quality drop.
- **Feasibility:** 1 GPU, 4 wk. **Venue:** ENLSP. **Novelty 2.5 — flagged.**

---

## NLP / Retrieval / RAG (public data)

### NEAR-MISS — true-but-misreferenced distractor benchmark *(TOP BET #1)*
- **Problem:** RAG confidently merges/substitutes the *adjacent* fact (2021 vs 2022 Nobel; predecessor vs current CEO) — distinct from generic noise, invisible to clean-corpus EM.
- **Contribution:** Construct-controlled benchmark pairing each Q with a minimally-different distractor (same entity, wrong year/holder) + a confusion-rate metric; reproducible Wikidata generation recipe.
- **Vs prior art:** RGB/RECALL use *fabricated* counterfactuals; near-miss distractors are *true but about a different referent* — harder and more realistic.
- **Experiment:** ~1.5–3k items from Wikidata temporal/role relations; Llama-3.1-8B/Qwen2.5-7B + 1 API; metric confusion rate vs generic-noise condition. Pass = near-miss degrades ≥15 pts more than equal-volume generic noise (p<0.01).
- **Feasibility:** public data, scripting, 1 GPU + <$40, 5–6 wk. **Venue:** EMNLP/ACL/NAACL dataset track + Findings. **Novelty 4.**

### Retrieval-Induced Self-Doubt — net harm of RAG conditioned on closed-book correctness *(TOP BET #2)*
- **Problem:** RAG is assumed a safety net, but mediocre passages flip already-correct parametric answers to wrong; the net harm is unquantified.
- **Contribution:** "Flip matrix" P(correct→wrong) vs P(wrong→correct) stratified by difficulty/retrieval quality + a logprob-confidence gate ("when not to retrieve").
- **Vs prior art:** Aggregate robustness audits (2510.09106, RARE) don't condition on per-item parametric correctness or prescribe a gate.
- **Experiment:** NQ-open/TriviaQA/PopQA; Llama-3.1-8B + API; BGE/Contriever, top-k {1,3,5} + hard distractors; metric flip matrix + gate AUROC. Pass = gate recovers ≥50% of regressions while keeping ≥90% of gains.
- **Feasibility:** 1 GPU + <$50, embeddings precomputed, 4–5 wk. **Venue:** EMNLP/ACL Findings, RAG workshop. **Novelty 4.**

### Paraphrase-Robust Contamination Probing (difficulty-matched clean control)
- **Problem:** Contamination detectors are defeated by paraphrase/translation; need a cheap post-hoc probe.
- **Contribution:** Consistency-gap = accuracy(original) − accuracy(paraphrase), calibrated against a *difficulty-matched clean control* benchmark; reference-free, works on any released benchmark/API model.
- **Vs prior art:** PaCoST/CoDeC/watermark methods need significance testing, tuning, or pre-release watermarking. **Partial-overlap risk — control design is the selling point.**
- **Experiment:** GSM8K/MMLU + GSM-Symbolic/post-cutoff control; 4–6 models of varied release dates; metric paraphrase-gap z-score vs known leakage. Pass = AUROC ≥0.75 separating leaked vs clean, beating perplexity baseline.
- **Feasibility:** API + small GPU, <$80, 4 wk. **Venue:** EMNLP Findings, contamination workshops. **Novelty 3.5 — flagged.**

### Cross-Lingual Retrieval Asymmetry — query-lang vs doc-lang factorial
- **Problem:** Multilingual RAG fails in low-resource langs but practitioners don't know whether to translate the query, the corpus, or fix the embedder.
- **Contribution:** Factorial query-lang × doc-lang design on parallel data isolating each factor's marginal cost → actionable "translate-the-query-not-the-corpus"-type rule.
- **Vs prior art:** MMTEB benchmarks breadth; this is a *separable causal-style attribution*.
- **Experiment:** MIRACL/MLQA/XQuAD across resource tiers; multilingual-e5/BGE-M3/LaBSE + MT API; metric nDCG@10 + ANOVA variance attribution. Pass = significant dominant factor (p<0.01) + one strategy closes ≥40% of the gap.
- **Feasibility:** 1 GPU + <$60, 4–5 wk. **Venue:** EMNLP/ACL multilingual track. **Novelty 3.5.**

---

## Applied ML on Public Scientific Data (no IRB)

### Conformal Carbon — coverage guarantees for eddy-covariance flux gap-filling *(strong, journal-fit)*
- **Problem:** 30–60% of FLUXNET CO2 data are gaps; ML gap-fillers' uncertainty is miscalibrated during long gaps, biasing carbon-budget error bars.
- **Contribution:** Blocked/seasonal conformal prediction keyed to gap structure → distribution-free coverage; show deep ensembles under-cover on long gaps while conformal recovers nominal.
- **Vs prior art:** Deep-ensemble gap-filling (2025) has no formal guarantee; conformal × flux appears unaddressed.
- **Experiment:** FLUXNET2015 / ICOS Warm Winter 2020; XGBoost + MC-dropout MLP + conformal; artificial-gap validation; metric empirical coverage vs nominal 90% by gap length. Pass = conformal 88–92% across strata, ensembles <80% on long gaps, RMSE within 5%.
- **Feasibility:** laptop CPU/GPU, <5GB, 6–8 wk. **Venue:** Biogeosciences / AgForMet, or NeurIPS Climate-Change-AI. **Novelty 4 — move fast, scoop risk.**

### Year-Holdout Stress Test — honest uncertainty for crop-yield models under extreme weather
- **Problem:** Satellite yield models fail in the extreme years that matter most and become *overconfident*; uncertainty is validated only in-distribution.
- **Contribution:** Leave-one-extreme-year-out benchmark showing overconfidence OOD + cheap conformal fix calibrated on weather-distance to training years.
- **Vs prior art:** OOD-yield papers (2510.07350, VITA) evaluate point accuracy/transfer, not uncertainty calibration under extreme-year shift.
- **Experiment:** CropNet (HF) or county NDVI + USDA NASS; LSTM/1D-CNN + GBM; baselines deep ensemble/MC-dropout/quantile; hold out 2012 drought etc. Pass = conformal-weather-distance ≥85% coverage in extreme years where ensembles fall <60%.
- **Feasibility:** county-feature path is laptop-scale, 7–9 wk. **Venue:** NeurIPS Climate-AI, ERL/Remote Sensing. **Novelty 4.**

### Drift-Aware Transferable Calibration of Low-Cost PM2.5 Sensors
- **Problem:** Low-cost PM2.5 sensors drift; ML calibrations degrade over time and transfer poorly far from reference monitors.
- **Contribution:** Couple calibration with an online conformal change-detector (guaranteed false-alarm rate) triggering recalibration only when warranted; quantify how much reference data a distant sensor needs.
- **Vs prior art:** Crowded (2023–26); novelty hinges on the conformal drift+transfer unification. **Most "already-worked-on" of the four.**
- **Experiment:** EPA AirNow + PurpleAir co-located pairs; RF/GBM + conformal martingale; baselines static/periodic/ADWIN. Pass = ≥15% RMSE reduction post-drift with ≤50% as many recalibrations.
- **Feasibility:** CPU-only, 6 wk. **Venue:** AMT / Sensors, KDD ADS. **Novelty 3 — flagged.**

### When Does ML Beat the Reanalysis? — regime-stratified precipitation post-processing
- **Problem:** Post-processing papers report aggregate skill; practitioners don't know which regimes ML actually beats simple baselines.
- **Contribution:** Regime-stratified skill decomposition (rain intensity × lead time × season) + a "when ML is worth it" decision rule; negative-result-friendly.
- **Vs prior art:** SPPM/multi-stream CNN optimize overall skill; no honest stratified failure map.
- **Experiment:** MeteoNet + ERA5; small U-Net vs quantile-mapping/analog/climatology; metric CRPS/Brier/reliability stratified. Pass = ≥1 regime ML significantly wins AND ≥1 regime it doesn't.
- **Feasibility:** 1 GPU, regional subset, 8–10 wk (data engineering heavy). **Venue:** Monthly Weather Review / QJRMS, Climate-AI. **Novelty 3.5.**

---

## Multi-Agent LLM Systems (ties to this repo's theme)

### CacheGuard — silent prompt-cache misses from agent nondeterminism *(TOP BET #4)*
- **Problem:** Agents lose 40–90% of caching discount silently when tool ordering / JSON keys / dynamic prompt fields rotate. Invisible — just a higher bill.
- **Contribution:** Deterministic canonicalization layer + a "cache-fragility linter" + the first cross-framework measurement (AutoGen/LangGraph/CrewAI) of forfeited discount.
- **Vs prior art:** "Don't Break the Cache" (2601.06007) asks *whether* caching helps; practitioner posts note ordering bugs anecdotally. None quantify cross-framework loss or ship a canonicalizer.
- **Experiment:** τ-bench + GAIA-val with Claude Haiku/Sonnet + open model (vLLM); metric cache-hit rate, cached-token fraction, $/task. Pass = ≥30% $/task reduction at equal success, recover ≥50% of cacheable tokens.
- **Feasibility:** API ~$150–300, optional 1 GPU, 4–6 wk. **Venue:** MLSys/efficiency workshops. **Novelty 4** (risk: reviewers may call it engineering → lead with the measurement study).

### AttributeCheap — cost-bounded failure attribution for multi-agent systems *(TOP BET #5)*
- **Problem:** Automated failure attribution (Who&When) is expensive (whole-trace context or O(n) LLM calls); solo builders can't afford it.
- **Contribution:** Cost-accuracy Pareto + small-model-first cascade (cheap heuristics localize the window, expensive model verifies only that window).
- **Vs prior art:** Who&When (ICML 2025) and causal-graph follow-ups optimize accuracy, not cost; no one reports the frontier or a small-model cascade.
- **Experiment:** Who&When (pre-built labeled traces); Qwen2.5-7B/14B filter + Claude verifier; metric attribution accuracy vs tokens/$. Pass = ≥90% of best step-accuracy at ≤40% token cost.
- **Feasibility:** 1×24GB GPU + modest API, ~$100–200, 4–5 wk. **Venue:** ACL/EMNLP Findings, agents workshop. **Novelty 4.**

### ReplayKit — determinism harness making tool-using agent runs bit-reproducible
- **Problem:** Agent runs are nondeterministic even at temp 0 (tool latency, decoding, external state); a failure seen once often can't be reproduced — the top agent-debugging pain.
- **Contribution:** Record-and-replay of the full agent boundary (tool I/O, seeds, model responses via caching proxy) + a *divergence-attribution metric* quantifying how/why replay diverges.
- **Vs prior art:** "Replayable Financial Agents" (2601.15322) is domain-specific; "Agents Disagree With Themselves" measures consistency. None offer a general harness + divergence attribution on public benchmarks.
- **Experiment:** τ-bench + WebArena-lite; Claude + open model; metric replay fidelity + divergence breakdown + steps-to-reproduce. Pass = ≥95% step-level fidelity vs <60% naive rerun.
- **Feasibility:** API + 1 GPU, ~$150–250, 5–6 wk (engineering-heavy). **Venue:** MLSys, ICSE/FSE tooling track, reproducibility workshop. **Novelty 4.**

### TopologyOracle — predict when multi-agent beats single-agent per task *(⚠ crowded)*
- **Problem:** Multi-agent costs 3–10× for often marginal/negative gains; no cheap per-task predictor of whether decomposition helps.
- **Contribution:** Pre-execution classifier (task length, tool-graph branching, sub-goal count) routes single vs multi; headline = *cost-adjusted* win rate counting wasteful multi-agent.
- **Vs prior art:** AdaptOrch (2602.16873) selects among topologies but assumes multi-agent + optimizes accuracy. **Moderate overlap — lead with cost-regret or it reads as "AdaptOrch minus topologies."**
- **Experiment:** GAIA-val + SWE-bench-lite, model held constant; metric accuracy-per-dollar. Pass = match always-multi within 2 pts at ≥35% lower cost.
- **Feasibility:** API/1 GPU, ~$200–400, 5–7 wk. **Venue:** COLM, EMNLP. **Novelty 3 — flagged.**

---

## ML Evaluation Science / Reproducibility (cheapest, measurement papers)

### Confidence Coherence — same fact, different phrasing, same confidence?
- **Problem:** Abstention/routing systems treat verbalized confidence as a stable (model, fact) property; if it swings under paraphrase, every threshold is unreliable.
- **Contribution:** New metric **Confidence Coherence** (dispersion of confidence across K meaning-preserving paraphrases); finding: dispersion (not just mean miscalibration) drives abstention failures.
- **Vs prior art:** SteerConf etc. use paraphrase ensembles as a *fix* and report aggregate stability; this makes per-item dispersion the object of study.
- **Experiment:** TriviaQA/PopQA/SciQ, K=10 NLI-validated paraphrases; GPT-4o-mini/Haiku/Llama-3.1-8B/Qwen2.5-7B; metric per-item confidence std + ECE + abstention AUROC. Pass = top-quartile-dispersion items have significantly worse selective accuracy (p<0.01).
- **Feasibility:** API + 1 GPU, <$80, 4–5 wk. **Venue:** Reliable-ML/Eval workshops, EMNLP Findings. **Novelty 4.**

### The Decontamination Illusion — perturbation contamination detectors just measure brittleness?
- **Problem:** Perturbation-based contamination detectors flag a model when accuracy drops on rewritten items — but perturbations degrade *clean* models too (surface brittleness), causing false accusations.
- **Contribution:** Use guaranteed-clean (post-cutoff/freshly-templated) benchmarks to establish the brittleness-only drop, then quantify the false-positive rate of popular detectors.
- **Vs prior art:** "Fragility of Benchmark Contamination Detection" (2510.02386) argues detectors are robust; this cross-wires brittleness work to deliver a calibrated FP estimate. **Position sharply or it reads incremental.**
- **Experiment:** MMLU/GSM8K + clean control (GSM-Symbolic/post-cutoff); Llama-3/Mistral/Qwen/Phi (open, for logprobs); decompose drop into brittleness vs excess. Pass = show fixed-threshold detector flags a measurable % of clean control as contaminated.
- **Feasibility:** 1 GPU, no training, few GPU-days, 5–6 wk. **Venue:** COLM, EMNLP Findings, NeurIPS D&B. **Novelty 4.**

### Judge Disagreement Cartography — predict where LLM-judge verdicts are unreliable
- **Problem:** Judges decide leaderboards/RLHF; aggregate bias rates don't tell you *which items* to escalate to humans.
- **Contribution:** Predict per-item flip risk (under position swap / paraphrase / judge change) from cheap features → targeted human escalation policy.
- **Vs prior art:** Reliability papers report population rates; this is a predictive flip model validated as an escalation tool. **Crowded — novelty is the routing framing.**
- **Experiment:** MT-Bench/Arena-Hard + RewardBench; 2–3 judges; logistic/GBT on cheap features; metric flip-AUROC + escalation lift. Pass = AUROC >0.75, lift significant vs verbosity-only.
- **Feasibility:** mostly API, ~$150–250, 4–5 wk. **Venue:** Eval/Reliable-ML workshops, ACL/EMNLP Findings. **Novelty 3 — flagged.**

### Seed Lottery in Pass@1 — reproducibility audit of reasoning leaderboard claims
- **Problem:** Reasoning leaderboards report single-run Pass@1; seed/temperature variance can move scores 5–15 pts and invert ranks. Many "A beats B by 2 pts" claims may be noise.
- **Contribution:** Empirical per-benchmark "minimum reportable gap" (effect-size floor with CIs) + audit of recent published gaps that fall below it.
- **Vs prior art:** "On Randomness in Agentic Evals" / "Do Repetitions Matter?" prescribe "run more"; this delivers a significance threshold + a published-claim audit.
- **Experiment:** GSM8K/MATH-500/HumanEval/GPQA-Diamond; 4–6 open models; N=20–30 seeds; derive min gap via bootstrap; tabulate violating claims. Pass = a measurable fraction of recent gaps fall within seed-noise CIs.
- **Feasibility:** 1 GPU, open models, few GPU-days, 4–5 wk (lowest cost). **Venue:** NeurIPS D&B, MLRC/reproducibility. **Novelty 3 — must deliver the audit, not just re-measure.**

---

*All six area agents are still live and resumable — to go deeper on any idea (scoop-check, full proposal, experiment plan), continue the corresponding agent.*
