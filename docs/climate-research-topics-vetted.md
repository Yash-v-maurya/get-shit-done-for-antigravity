# Climate / Earth-Observation Research Topics — Vetted for a Solo, Modest-Compute, Publishable Paper

Two passes feed this doc: (1) an **adversarial scoop-check** of the five original climate candidates (GO / REFINE / KILL, with data reality-checks), and (2) **15 fresh ideas** across five sub-domains, each with closest prior art cited, free-data access path verified, and a scoop-risk note.

Lens for everything below: **one researcher, single consumer GPU or laptop/CPU, free public data, no IRB, target a workshop/journal paper.** "Robust to null result" = the paper is publishable whether or not the hoped-for effect shows up (the best risk profile for a solo timeline).

---

## ⭐ Recommendation

| Rank | Idea | Satellite-native? | Novelty | Scoop-risk | Null-result risk | Compute |
|:---:|------|:---:|:---:|:---:|:---:|:---:|
| **1** | **Uncertainty Equity Audit** (are AQ model error-bars wider in disadvantaged areas?) | ✅ TROPOMI + MODIS AOD | 4.5 | low–mod (move in ~6 mo) | **low — publishable either way** | CPU |
| **2** | **Conformal Carbon** (coverage-guaranteed flux gap-filling) | ❌ flux towers | 4 | **low — scoop-checked GO** | medium (effect may shrink) | CPU |
| **3** | **Falsifiable MEF Benchmark** (validate marginal-emissions estimators) | ❌ grid data | 4.5 | low–mod | **low — a discriminating benchmark is the contribution** | CPU |

**My pick: #1 Uncertainty Equity Audit.** It's the best fit for "climate + satellite + solo + publishable": satellite-native, CPU-only, all-free data, genuinely fresh framing (conformal UQ × environmental-justice fairness — nobody has connected the two for air quality), a strong social-impact venue path (AAAI AI for Social Impact, NeurIPS CCAI), and — critically — it produces a citable finding whether the disparity turns out large *or* small. The one watch-out: a fair-ML group could apply group-conditional coverage here first, so move within ~6 months.

If you want the **safest possible scoop profile**, #2 Conformal Carbon is the only idea that passed the dedicated scoop-check as a clean GO with a trivially-downloadable dataset — its only real risk is scientific (the under-coverage effect may shrink once ensembles are tuned). If you want **highest novelty and don't mind it being grid- rather than satellite-data**, #3 the MEF benchmark fills a gap the 2025 literature *explicitly says is open*.

---

## Part A — Scoop-check verdicts on the original 5 candidates

### Conformal Carbon — flux gap-filling with coverage guarantees → **GO (lean REFINE)** · solo-confidence 4
- **Status:** No paper does conformal prediction for eddy-covariance CO2 flux gap-filling. Methods (ACI/AgACI, EnbPI) and the flux-uncertainty problem each exist; the cross is unpublished. CarbonBench (2026) lists it as a *future* direction — on the radar, unclaimed.
- **Surviving differentiator:** *Gap-length-conditional coverage* — show deep-ensemble intervals systematically under-cover as artificial gap length grows, then a seasonally/diurnally **blocked** conformal scheme restores nominal coverage where generic ACI/EnbPI don't. Pitch as domain-conditional-validity, **not** "first CP for time series."
- **Data:** ICOS Warm Winter 2020 (DOI 10.18160/2G60-ZHAK), 73 stations, ONEFlux format, **8.6 GB, CC-BY-4.0**, light registration — cleaner than FLUXNET2015. Gotcha: 60–68% of half-hourly CO2 is *already* gap-filled, so build artificial gaps over QC-good data for honest eval.
- **Risk:** scientific, not data/feasibility (best profile of the five) — the effect may shrink once ensembles are tuned.

### Year-Holdout crop-yield uncertainty → **REFINE** · solo-confidence 4
- **Status:** PARTIAL scoop. An IJERT 2025 paper already claims conformal coverage under inter-annual shift (~70% of the pitch) — but low-prestige venue, uses plain ACI, not framed as overconfidence diagnosis. arXiv 2510.07350 does OOD-on-CropNet but point-prediction only.
- **Surviving differentiator:** Leave-one-**extreme**-year-out showing intervals miscalibrate *as a function of weather-distance from training years*, fixed with a **weather-distance-weighted** (Mondrian) conformal that explicitly beats ACI. Lead with the overconfidence-vs-distance curve.
- **Data:** CropNet is **2.61 TB — do NOT clone**; use the `cropnet` PyPI API for precomputed NDVI + USDA yield. Only 6 years (2017–2022) → no 2012-style droughts (real constraint). Supplement with USDA NASS QuickStats (free key) for longer history.

### PM2.5 drift-aware calibration → **REFINE (crowded)** · solo-confidence 4
- **Status:** PARTIAL→heavily scooped. Drift-triggered LCS recalibration is published (Sensors 2024, W-UDDR 2025); **Veli (AAAI 2026)** + the AQ-SDR benchmark (23,737 sensors) will out-compete a vanilla integration. CP+drift on generic streams also exists.
- **Surviving differentiator:** *Valid coverage under drift* — interval blow-up itself as the drift trigger, with conditional coverage reported during wildfire-smoke high-concentration regimes where calibrations collapse. Benchmark on AQ-SDR.
- **Data:** EPA AQS bulk CSVs free (no key). **PurpleAir is a paid points API since March 2024** — use AQ-SDR / published colocation sets instead.

### Precipitation post-processing regime map → **REFINE (fix data design)** · solo-confidence 3
- **Status:** PARTIAL. Horat & Lerch 2023 use circulation regimes as model *inputs*; an explicit "regime map of when ML beats classical quantile-mapping" doesn't exist.
- **Surviving differentiator:** Flip to a *diagnostic decision-map* — stratify CRPS/Brier-skill *gain of ML over QM* by objective regime; honest "QM is good enough for stratiform/low-intensity, ML pays off for convective/extreme."
- **Data fix (important):** ERA5 is reanalysis, **not** a forecast — don't use it as forecast-to-correct or as truth. Correct roles: MeteoNet **AROME/ARPEGE = forecast**, MeteoNet **radar/stations = truth**, ERA5 only for regime-defining fields (Z500/MSLP). MeteoNet is NW+SE France, **2016–2018 only (3 yrs)** — thin for per-regime statistics; accept narrow proof-of-concept scope.

### FlareScore — per-flare combustion efficiency from fused satellites → **REFINE (high-effort moonshot)** · solo-confidence 3
- **Status:** PARTIAL. In-situ DRE (Plant et al., *Science* 2022, F3UEL) and each satellite stream individually exist; tri-sensor fusion-to-DRE is unpublished, but the validation target is identical to Plant's, so "DRE from space" is heavily anticipated.
- **Surviving differentiator:** The fusion inversion led by **black-carbon-as-incomplete-combustion-proxy** (soot = direct optical signature of incomplete combustion, unexploited by DRE-from-satellite work). Frame as "satellite-only DRE proxy validated against F3UEL aircraft truth."
- **Data:** VIIRS Nightfire free (EOG). TROPOMI CH4 free but **~5.5×7 km — single-flare attribution only for large/clustered flares**. Sentinel BC: **no operational product, derive it yourself** (time sink). F3UEL validation data is public (U-Michigan Deep Blue) — but it's the same data behind the scoop paper.

---

## Part B — 15 fresh vetted ideas

### Carbon / GHG / Methane (satellite + flux)

**Conformal methane super-emitter alerts — distribution-free false-positive control on TROPOMI** · ✅satellite · novelty 4
- Give regulators alerts with a *guaranteed* false-positive rate (e.g. "≤5% at 90% confidence"), stratified by land-cover (cities/rivers/mountains where models misfire). Wrap a small ResNet-18 plume classifier in split + Mondrian conformal.
- **Vs prior art:** ensemble-uncertainty (2511.07719) and ViT+SAM detectors give *heuristic* uncertainty; none give finite-sample coverage or stratum-conditional calibration. Conformal not yet applied to methane alerting.
- **Data:** TROPOMI CH4 free (Copernicus / NASA GES DISC); labels from Schuit et al. 2023 supplement + UNEP IMEO MARS. Pass = marginal FPR within ±2% of nominal, worst-stratum gap <5%, beating temperature scaling by >30%. 1 GPU, 4–6 wk. **Scoop-risk moderate — stratum-conditional methane angle is the moat.**

**Where Inventories Lie — pixel-level attribution of EDGAR–satellite methane discrepancy** · ✅satellite · novelty 4
- XGBoost + SHAP predicts the gridded EDGAR-minus-satellite CH4 residual from open covariates (land cover, livestock, O&G infra, wetlands) → an interpretable "inventory-error driver" map showing *where and why* the inventory is wrong.
- **Vs prior art:** Worden 2023 / 2026 US benchmark quantify discrepancies *regionally*; none learn a spatially-explicit, covariate-attributed error model. **CPU-only**, all-open data (EDGAR v8, GLW, OGIM, WAD2M). Pass = spatial-holdout R²>0.4, top-decile hotspot recall >60%. 5–7 wk. **Scoop-risk low-moderate.**

**Honest Error Bars for Methane Flux Gap-Filling — cross-site calibrated UQ at FLUXNET-CH4** · ❌flux · novelty 3.5
- Leave-one-site-out (OOD) + *contiguous* gaps + **annual-cumulative** uncertainty validation (the quantity scientists actually use). Conformalized quantile regression vs deep ensembles.
- **Vs prior art:** Irvin/Knox compared gap-fillers within-site on point fluxes; the OOD + contiguous-gap + annual-cumulative + conformal combo is open. **Scoop-risk moderate-high** (V2.0/2025 release will spawn papers) — frame as a benchmark/best-practices contribution and publish promptly.

### Weather / Forecasting (reanalysis, no global training)

**When Does Conformal Prediction Fail on AI-Weather Extremes? — spatial coverage audit + EVT fix** · reanalysis · novelty 4
- CP is marketed as "rigorous guarantees" for AI weather, but exchangeability fails spatially and coverage collapses on extremes. Audit where vanilla CP under-covers on GraphCast/Pangu/GenCast, then a spatial-adaptive + extreme-value-theory CP fix.
- **Vs prior art:** 2606.19642 applies vanilla CP; Extreme-CP (2505.08578) isn't on gridded AI weather. **No training, mostly CPU/numpy**, all from WeatherBench2 public GCS. Pass = restore conditional-on-extreme coverage to within ±3% at 99% without >25% width inflation. 4–5 wk. **Scoop-risk moderate-low — prioritize.**

**Sharpness-Aware Recalibration of AI-Weather Ensembles for Heavy-Precip Thresholds over India** · reanalysis · novelty 3.5
- Tail/threshold-conditional post-processor that corrects AI-model heavy-rain underdispersion, evaluated vs IMD gauges by monsoon phase. WeatherBench2 forecasts + IMD 0.25° rainfall. Pass = ≥15% twCRPS gain at the 50mm threshold. 4–6 wk. **Scoop-risk moderate** (post-processing crowded; tail+IMD+monsoon niche defensible).

**LoRA Fine-Tuning of a Weather Foundation Model for Extreme-Rain Downscaling in Data-Poor Africa** · reanalysis+satellite · novelty 3.5
- PEFT (LoRA) of Aurora/Prithvi-WxC for regional extreme-precip downscaling beats from-scratch CNN on scarce African data. ERA5→CHIRPS. **VRAM is the risk** (Aurora needs ~16–24GB → tile + freeze backbone). 6–8 wk. **Scoop-risk medium-high** (PEFT-of-Aurora active) — African data-poor extreme-rain specialization + from-scratch baseline is the moat.

### Land / Vegetation / Disaster (optical + SAR)

**Calibrated Cross-Event Flood Mapping — can SAR flood models be trusted when the region changes?** · ✅SAR · novelty 4
- Leave-one-region-out *calibration* benchmark (ECE, selective-prediction risk-coverage), not accuracy — shows SAR flood models stay overconfident on wrong pixels in new regions, and cheap post-hoc fixes recover trustworthy confidence for responders.
- **Vs prior art:** DeepSARFlood *reports* uncertainty but never evaluates calibration under domain shift. **Sen1Floods11 (14GB, CC-BY, public GCS); fits 8–12GB GPU.** Pass = cut ECE >40%, lift IoU-at-80%-coverage by >5 pts. 4–6 wk. **Scoop-risk moderate-low** (benchmark framing is hard to scoop).

**How Much Does Hansen Label Noise Cost You? — noise-robust deforestation training** · ✅satellite · novelty 4
- Quantify accuracy lost to Hansen GFC label noise (using INPE PRODES as clean truth) and benchmark noise-robust losses (GCE, symmetric CE, co-teaching) for Sentinel-2 deforestation segmentation → a practitioner recipe for training on noisy free labels.
- **Vs prior art:** generic label-noise benchmark (2603.00604) exists; deforestation-specific + *real* Hansen noise + PRODES-clean triad is open. 1 GPU, 5–7 wk. **Scoop-risk moderate — move fast on the specificity.**

**Few-Shot Burned-Area Delineation — when is a geo-foundation model actually worth it for a solo lab?** · ✅satellite · novelty 3.5
- Compute-vs-accuracy frontier: LoRA-tuned Clay/Prithvi vs from-scratch U-Net at 5/10/20/50 shots for burn mapping; find the break-even shot count and whether an FM ever wins within 12GB VRAM. CaBuAr (HF, CC-BY). 6–8 wk. **Scoop-risk moderate-high** — burn-specific + solo-compute-frontier framing differentiates.

### Renewable Energy / Grid (open data, mostly CPU)

**Falsifiable MEF Benchmark — validate marginal-emissions estimators via demand-shock quasi-experiments** · ❌grid · novelty **4.5** (highest)
- The literature *admits MEFs have no ground truth*, so all carbon-aware scheduling rests on unvalidated signals (and recent work shows shifting often yields ~0 or negative real reductions). Construct pseudo-ground-truth from natural demand shocks (DST shifts, holidays, industrial trips) in ENTSO-E data, then score MEF estimators against these quasi-experiments.
- **Vs prior art:** 2025 papers propose estimators or argue eval is hard; none build an open quasi-experimental validation harness. **CPU-only**, ENTSO-E free (registration). Pass = the benchmark *discriminates* (≥1 estimator class statistically distinguishable from average-EF and from others). 5–7 wk. **Scoop-risk low-moderate — strongest novelty, gap explicitly acknowledged open.**

**Sharp-Window Calibration — probabilistic forecasting specialized for solar/wind ramp events** · ❌energy · novelty 3.5
- Conformal recalibration *conditioned on a ramp-likelihood detector*, optimizing calibration on the rare ramp windows that drive reserve/curtailment decisions; ramp-stratified reliability protocol. NREL WIND Toolkit/NSRDB (free key). Pass = ≥15% CRPS gain on ramp windows, ramp coverage error <5%. CPU/1 GPU, 4–6 wk. **Scoop-risk moderate** — ramp-conditional twist is the delta.

**Honest Cold-Start — auditing leakage/selection bias in zero-shot PV transfer** · ❌energy · novelty 3
- Re-evaluate booming zero-shot PV-transfer claims (SPIRIT/SolNet/Time-LLM) under leakage-controlled, geography-stratified protocols + a strong clear-sky physics baseline. Hypothesis: >30% of reported skill evaporates. NSRDB free. 6–8 wk. **Scoop-risk moderate-high** (eval-critique genre crowding) — pair the audit with the physics-baseline so it stands regardless.

### Air Quality / Urban Climate (satellite + sensors, mostly CPU)

**The Uncertainty Equity Audit — are AQ model errors systematically larger in disadvantaged communities?** · ✅satellite · novelty **4.5** · ⭐ TOP PICK
- High-res PM2.5/NO2 maps drive environmental-justice policy, but are trained on monitors concentrated in wealthier areas. Produce spatially-resolved conformal prediction intervals, then test whether interval width / miscoverage correlates with demographic disadvantage (SVI/CalEnviroScreen). Introduce an **Uncertainty Disparity Ratio** metric.
- **Vs prior art:** EJ exposure maps report *mean* disparity (point estimates, no UQ-equity); GeoConformal gives spatial CP machinery but no EJ framing. Marrying the two is fresh — found no AQ paper doing it.
- **Data:** EPA AQS/AirNow (free), TROPOMI NO2 + MODIS MAIAC AOD (free via GEE), ERA5, Census ACS + CDC SVI + CalEnviroScreen (all free). **CPU sufficient** (GEE does raster pulls server-side). Pass = spatial conformal ≥90% coverage AND statistically significant UDR ≠ 1 (intervals ≥20% wider in disadvantaged tracts, p<0.05) — **clean result either way.** 5–7 wk.
- **Venue:** NeurIPS/ICLR CCAI, AAAI AI for Social Impact; *Environmental Data Science* / *Environmental Research: Health*. **Scoop-risk moderate-low — move within ~6 mo.**

**Cross-City Conformal Transfer of Low-Cost PM2.5 Sensors (no target reference monitor)** · ❌sensors · novelty 3.5
- Weighted/Mondrian conformal under covariate shift gives valid intervals in an unseen target city without local reference data, and quantifies the "transfer cost" (how much intervals must widen). Leave-one-city-out across US climate regions + one Global-South city. **Scoop-risk moderate-HIGH** (conformal sensor-calibration actively publishing in 2025–26) — only the cross-city OOD + transfer-cost angle is defensible; PurpleAir paywall applies.

**Did the Low-Emission Zone Work? — spatially-resolved multi-city causal attribution with TROPOMI** · ✅satellite · novelty 4
- Weather-normalized DiD / generalized synthetic control on *gridded* TROPOMI NO2 across ≥6 LEZ cities → maps the causal NO2 change inside/at-boundary/around the zone, tests for pollution displacement and equity spillover, and pools cities for a transferable effect.
- **Vs prior art:** Birmingham/Madrid studies are single-city and monitor-based; gridded causal maps + cross-city pooling is the contribution. **CPU + GEE.** Main risk: identification rigor (satellite trend confounding) + LEZ-boundary curation labor. 7–9 wk. **Scoop-risk moderate.**

---

## Suggested next step
Pick one and I'll write a full proposal: precise research questions, dataset-construction steps with the exact access paths, experiment matrix, baselines, an expected-results table, ablations reviewers will demand, and a 6-week solo timeline. My recommendation is the **Uncertainty Equity Audit** (satellite, CPU, fresh, publishable either way), with **Conformal Carbon** as the safest-scoop alternative and the **MEF Benchmark** as the highest-novelty non-satellite option.

*All eleven ideation/scoop agents are resumable for deeper scoop-checks or feasibility drilling on any single idea.*
