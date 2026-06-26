# Round 2 — Higher-Novelty, Clever-Method Project Ideas
### "The approach itself turns heads" — novel modalities & cleverer framings

Round 1 leaned on "classify satellite pixels with ML." This round deliberately hunts **underused data modalities** (audio/bioacoustics, InSAR ground-motion, AIS, altimetry, phone-mobility, radio speech) and **cleverer methods** (causal inference, physics-informed NNs, foundation-model transfer, graph connectivity, change-point detection). 30 data-verified ideas across 6 fresh domains.

Novelty score: 5 = genuinely fresh framing / new primitive; 3 = solid application twist. Everything below was web-verified for real, accessible data.

---

## ⭐ TOP 10 — the standouts (novelty × demo-ability × clean data)

1. **Tailings-dam failure precursor screener** — separate benign settling from pre-collapse acceleration across a whole dam inventory *(InSAR, novelty 5, named-disaster gravitas)*
2. **Mercury-plume forensics** — trace illegal-gold-mining poison downriver to exposed communities *(novelty 5, all-GEE)*
3. **Salar Stress Index** — link lithium-brine pumping to flamingo-wetland collapse with Granger causality *(novelty 5, green-tech irony)*
4. **Amazon river-isolation forecaster** — predict which villages lose river access & when, via network graph *(novelty 5, vivid human impact)*
5. **"Sabotage vs Rust" oil-spill auditor** — independently corroborate contested spill-cause calls that decide compensation *(novelty 5, labels already exist)*
6. **Scorched-earth attribution** — separate deliberate wartime cropland destruction from drought with a climate counterfactual *(novelty 5)*
7. **Physics-informed InSAR→groundwater inversion** — turn ground "breathing" into recoverable-vs-permanent aquifer loss via a PINN *(novelty 5, clever method)*
8. **Reef restoration-stage soundscape grader** — audit reef recovery from sound, with an interpretable recovery fingerprint *(novelty 4, brand-new 2025 labeled data)*
9. **Dark dredgers** — move dark-vessel detection off the ocean into rivers to catch illegal sand mining *(novelty 5)*
10. **Radio-to-Risk famine nowcasting** — mine local-language community radio as a famine leading indicator *(novelty 5, but has an audio-acquisition risk — scope carefully)*

**My pick for highest novelty-to-effort with cleanest data:** the **Mercury-plume forensics**, **Salar Stress Index**, or **Sabotage-vs-Rust auditor** — all run on free Sentinel/GEE, all reframe a known problem into a human-impact/accountability product, and all have a working data path today.

**Biggest "lives-at-stake wow":** the **Tailings-dam precursor screener** (mirror of the Brumadinho disaster; private capability made open).

---

## A. BIOACOUSTICS & SOUND-BASED ECOLOGY

### A1. Reef restoration-stage soundscape grader ⭐ (novelty 4)
- **Region:** Indo-Pacific reefs (Indonesia Mars/Sheba sites; also Maldives, Kenya, Mexico, Australia).
- **Problem:** Reef restoration costs millions, but verifying *ecological recovery* needs expensive diver surveys. Healthy reefs are acoustically noisy (fish, snapping shrimp) — sound is a cheap proxy.
- **Novelty:** Prior work classifies reefs as binary healthy/degraded. Angle: predict the **restoration trajectory stage** (degraded / early / mid / healthy) as a continuous recovery score with calibrated uncertainty, *and* identify which sonotypes drive it (interpretable recovery fingerprint) — an auditable progress metric.
- **Approach:** Fine-tune an audio CNN/transformer (PANNs/AST) on log-mel spectrograms as **ordinal regression** over restoration stages; multi-task on the shipped fish/invertebrate sonotype detections; SHAP/attention over shrimp-crackle and fish-call bands.
- **Data:** Coral Reef Soundscapes from a Global Restoration Programme (UCL/Figshare, Tournois et al.) — WAV, >1 yr audio + AI sonotype detections, 45 sites, 4 stages — https://rdr.ucl.ac.uk/articles/dataset/_b_Coral_Reef_Soundscapes_from_a_Global_Restoration_Programme_b_/29958062 — verified.
- **Risks:** Big download; site-confound → leave-one-site-out validation. The ordinal/interpretable framing is what makes it more than re-running a classifier.

### A2. Congo acoustic poaching + logging risk fusion (novelty 4)
- **Region:** Northern Republic of Congo rainforest (Elephant Listening Project grid).
- **Problem:** Forest elephants poached toward extinction; rangers can't cover roadless forest. Gunshots/chainsaws travel far acoustically.
- **Novelty:** Rainforest Connection/ELP detect gunshots or rumbles separately. Angle: **jointly** model elephant infrasonic rumbles AND gunshot/chainsaw events on the same array → "animals here AND a gun fired 2 km away" → spatial poaching-risk map for ranger pre-positioning (matches the mentor's "pre-position responders" flavor).
- **Approach:** Two CNN detectors (low-band rumble + impulsive gunshot/chainsaw) + classical TDOA localization across the 50-unit grid → gridded risk heatmap.
- **Data:** Congo Soundscapes Public Database (Cornell ELP, AWS S3 public) — 8 kHz WAV, 50-unit grid — https://www.birds.cornell.edu/ccb/elephant-listening-project/congo-soundscapes-public-database/ — verified.
- **Risks:** Grid TDOA localization is ambitious (scope per-sensor first); sparse gunshot labels; 8 kHz caps high-freq cues.

### A3. Western Ghats few-shot rare-bird detector (novelty 4)
- **Region:** Western Ghats sky-islands, India.
- **Problem:** BirdNET/Merlin perform worst on endemic/endangered/nocturnal species — exactly the conservation-critical ones — because Xeno-canto has few recordings of them.
- **Novelty:** Build a **few-shot / metric-learning** detector (prototypical networks on bird embeddings) + self-supervised domain adaptation from clean Xeno-canto to messy in-situ soundscapes. Goal: beat BirdNET on the rare-species long tail, not the common birds.
- **Approach:** Pretrained embeddings (Perch / Google Bird Vocalization) → contrastive domain adaptation on unlabeled Ghats soundscapes → prototypical few-shot head.
- **Data:** BirdCLEF 2024 (Western Ghats, Kaggle, ~182 species, focal + unlabeled soundscapes) — https://www.kaggle.com/competitions/birdclef-2024 — verified; Xeno-canto API; Perch embeddings.
- **Risks:** <5-recording species are genuinely hard → curate a rare-but-feasible subset; report per-species long-tail gains.

### A4. Wingbeat acoustic vector-surveillance (mosquitoes) (novelty 3.5)
- **Region:** Tropical disease zones (India/SE Asia/SSA); phone-mic deployable.
- **Novelty:** HumBug classifies wingbeats in lab cups. Angle: **noise-robust species ID from cheap real phone mics in the wild** + couple with circadian timing → "Aedes active now" alerts.
- **Data:** HumBugDB (Zenodo, CC-BY, ~20h labeled, 36 species, Bayesian-CNN baseline) — https://zenodo.org/records/4904800 — verified.
- **Risks:** Wild-mic generalization is the hard/novel part; class imbalance toward cultured specimens.

### A5. Dawn-chorus phenology-shift predictor (novelty 3.5)
- **Region:** US Pacific Northwest forests (dataset region; method generalizes).
- **Novelty:** Most ecoacoustics computes static indices. Angle: model dawn-chorus timing/composition as a function of climate covariates → phenology *forecasting* from sound (space-for-time).
- **Data:** USFS PNW Avian Dawn Chorus (Zenodo, 1,575 soundscapes + 141 annotated, paired with 48 climate/forest covariates) — https://zenodo.org/records/8047850 — verified.
- **Risks:** Single year (2022) → frame as space-for-time climate-response, be explicit.

---

## B. InSAR & GROUND-MOTION HAZARDS
*(Feasibility lever: use ready displacement products — OPERA DISP-S1 for Americas/Alaska, EGMS for Europe, COMET-LiCSAR for volcanoes — to skip raw SLC processing. Drop to Sentinel-1 SLC + MintPy/LiCSBAS only where uncovered.)*

### B1. Tailings-dam failure precursor screener ⭐ (novelty 5)
- **Region:** Minas Gerais (Brazil), Andean copper belt, or Western Australia — pick one belt.
- **Problem:** Tailings dams fail catastrophically (Brumadinho 2019: 270 dead). InSAR showed ~2 months of accelerating anomalous deformation preceded collapse. Commercial per-dam monitoring exists privately; **no open, automated, multi-dam screening tool** flags which dams are entering pre-failure acceleration.
- **Novelty:** The unsolved-in-the-open problem is separating benign **consolidation settling** (normal) from **failure-precursor acceleration** (dangerous) automatically across a whole inventory. A 2025 Weibull consolidation-separation framework can become a deployed ML screener.
- **Approach:** OPERA DISP-S1 (or MintPy/LiCSBAS) displacement per dam wall → fit & subtract a consolidation model → feed residuals to a TCN/LSTM-attention classifier (stable vs destabilizing, trained on Brumadinho/Cadia/Feijão signatures) → inverse-velocity (Fukuzono) time-to-failure on flagged dams → ranked risk dashboard.
- **Data:** Sentinel-1 SLC (ASF); OPERA DISP-S1 (NASA/JPL/ASF, Americas, ready); EGMS (Europe); Global Tailings Portal (1,800+ dams, GRID-Arendal — bulk download by request). All verified.
- **Risks:** Few labeled failures → lean on consolidation-subtraction + anomaly detection, not supervised failure classification. Start in OPERA coverage to skip SLC processing.
- *Why it's the headline:* named disaster, lives at stake, private capability made open + automated.

### B2. Physics-informed InSAR→groundwater inversion ⭐ (novelty 5)
- **Region:** California Central Valley (OPERA) or Alto Guadalentín, Spain (EGMS).
- **Problem:** Aquifers compress/rebound poroelastically as heads change — InSAR sees it at mm scale — but converting deformation to water-storage change needs unknown aquifer properties.
- **Novelty:** Invert deformation through a **physics-informed neural network** embedding the poroelastic consolidation PDE, jointly learning hidden aquifer parameters and outputting groundwater-storage change — crucially splitting **reversible (elastic) vs permanent (inelastic) loss**, which is what water policy needs.
- **Approach:** InSAR displacement + sparse well hydrographs + GRACE basin constraint → PINN (data-fit + PDE residual loss; trainable skeletal/inelastic storativity per zone) → fine-resolution storage-change maps + elastic/inelastic split.
- **Data:** OPERA DISP-S1; EGMS; GRACE/GRACE-FO; USGS NWIS / CA DWR wells. All verified.
- **Risks:** PINN training instability → start 1D at well sites; the elastic/inelastic split is the research contribution, not guaranteed.

### B3. Differential-subsidence "infrastructure strain" mapper (novelty 4)
- **Region:** Mexico City (best-documented); transferable to Jakarta/Tehran.
- **Novelty:** Don't map where it *sinks* (pixel-grade) — compute the spatial **gradient/angular distortion** (what actually tears buildings, metro, pipes) and **forecast** where strain localizes next, intersected with infrastructure.
- **Approach:** Dense displacement field (OPERA/EGMS, V + E-W) → differentiate to curvature/strain → GCN or ConvLSTM forecast 1-2 yr ahead with groundwater/geology covariates → overlay OSM assets crossing the 1/500 damage threshold.
- **Data:** OPERA DISP-S1 (Mexico); EGMS; OSM; GRACE. Verified.
- **Risks:** Sparse damage labels → validate against published angular-distortion maps; E-W needs ascending+descending.

### B4. Permafrost thaw-subsidence early warning for Arctic infrastructure (novelty 4)
- **Region:** Alaska — Dalton Highway / Trans-Alaska Pipeline corridor, Arctic communities.
- **Novelty:** Separate the recoverable **seasonal freeze-thaw** cycle from the **secular (permanent)** settlement, and flag assets whose secular rate is *accelerating*.
- **Approach:** OPERA DISP-S1 → state-space/Kalman seasonal+secular decomposition → GP/LSTM forecast of secular subsidence with thaw-degree-day/snow/soil-ice covariates → per-asset risk.
- **Data:** Sentinel-1 SLC / OPERA DISP-S1 (Alaska); NASA ABoVE / Permafrost Discovery Gateway; ERA5-Land; OSM/Maxar footprints. Verified.
- **Risks:** Winter decorrelation → design for seasonal gaps; needs multi-year series.

### B5. Pre-eruption deformation onset detector for under-monitored volcanoes (novelty 4)
- **Region:** Global, prioritizing under-instrumented volcanoes (Indonesia, Andes, East African Rift).
- **Novelty:** COMET CNNs detect big fringe patterns *now*. Angle: detect subtle **onset/rate-change in the time series** and forecast escalating-vs-stalling unrest — change-point detection + trajectory forecasting fused with Smithsonian GVP eruption labels.
- **Approach:** LiCSAR/LiCSBAS ready time series → Bayesian online change-point onset flag → Mogi source inversion (depth/volume) → LSTM/TCN escalation classifier → global "unrest watchlist."
- **Data:** COMET LiCSAR/LiCSBAS; Sentinel-1 SLC; Smithsonian GVP; GNSS (validation). Verified.
- **Risks:** Atmospheric delay mimics deformation (GACOS correction); extreme label imbalance → anomaly + physical validation framing.

---

## C. ENVIRONMENTAL-CRIME & POLLUTION FORENSICS
*(Note: free NICFI Planet tropical basemaps were discontinued in 2025 — only the 2015-2025 archive remains on GEE for non-commercial use. All ideas stand on free Sentinel-1/2; treat Planet as optional corroboration.)*

### C1. Mercury-plume forensics ⭐ (novelty 5)
- **Region:** Madre de Dios, Peru (Amazon); extensible to Tapajós (Brazil), Ghana galamsey rivers.
- **Problem:** Artisanal gold mining dumps mercury + raises suspended sediment; mercury bioaccumulates in fish eaten hundreds of km downstream. Enforcement targets the *pits*; nobody maps who's downstream of the plume.
- **Novelty:** Turn the river itself into the sensor — model the suspended-sediment plume as a downstream-propagating signal, exploit the **seasonal turbidity inversion** that mining causes (PNAS 2019), calibrate against field mercury↔turbidity data → a **public-health mercury-exposure risk map**, not a mining detector.
- **Approach:** Sentinel-2 turbidity-index time series along river centerlines → temporal CNN/LSTM flags mining-impacted reaches via the seasonal-inversion anomaly → Amazon Mining Watch upstream polygons as weak labels → overlay WorldPop for downstream exposure.
- **Data:** Sentinel-2; Amazon Mining Watch (open GeoJSON); MAAP/ACA alerts; HydroRIVERS; WorldPop; PNAS 2019 Hg field calibration. All verified.
- **Risks:** Sparse Hg ground-truth → exposure layer is a risk *proxy*; the seasonal-inversion feature is the differentiator and the wow.

### C2. Dark dredgers — illegal river sand mining ⭐ (novelty 5)
- **Region:** Mekong, Ganga-Yamuna/Brahmaputra, or Pearl River. (Sand mining = world's largest unregulated extraction.)
- **Problem:** Illegal dredgers operate at night without AIS, collapsing banks and deltas. No continuous river-wide fleet picture.
- **Novelty:** Move dark-vessel detection **off the ocean into narrow rivers** (hard: banks/bridges confound detectors; a 2023 paper proved it's possible) → persistent "dredger pressure map" fused with morphological bank-change to separate dredges from cargo.
- **Approach:** Sentinel-1 GRD + adaptive river mask → CFAR detection → dwell-time classifier (dredge cluster vs transit) across the S1 stack → fuse GFW SAR detections + AIS gaps → corroborate with Sentinel-2 channel-widening.
- **Data:** Sentinel-1/2; Global Fishing Watch SAR; OSM waterways; MDPI 2023 method ref. Verified.
- **Risks:** Hardest of this cluster — bridge/bank false positives, 10 m misses small dredges, sparse river AIS. Dwell-time + morphology fusion is what makes it credible.

### C3. "Sabotage vs Rust" oil-spill cause auditor ⭐ (novelty 5)
- **Region:** Niger Delta, Nigeria.
- **Problem:** Operators are liable for corrosion spills but *not* "sabotage" — so cause classification on official spill reports is fiercely contested and decides compensation for thousands.
- **Novelty:** Use the open **NOSDRA spill database as labels** and ask whether satellite context independently corroborates or challenges the official cause — proximity to illegal-refining burn scars, spill morphology, recurrence, access paths. An **accountability/audit** model over a contested dataset, not another detector. Also regression-check reported volume vs NDVI die-off.
- **Approach:** NOSDRA records → per-spill satellite feature vector (Sentinel-1 slick, Sentinel-2/VIIRS burn scars & refining smoke, pipeline distance, recurrence, NDVI die-off) → gradient-boosted classifier with SHAP → flag disagreements with official cause as audit candidates.
- **Data:** NOSDRA / Nigerian Oil Spill Monitor (scrapeable); Sentinel-1/2; VIIRS; OSM pipelines; UNEP-GRID NOSDRA mirror. All verified.
- **Risks:** JIV labels are themselves biased (the thing audited) → frame as "independent corroboration"; SAR slicks over vegetated creeks are harder than open ocean. Highest social-impact-per-novelty.

### C4. Ship-breaking pollution forensics ledger (novelty 4)
- **Region:** Alang-Sosiya (India), Sitakunda/Chattogram (Bangladesh).
- **Novelty:** Prior work measured recycling *activity*. Angle: **event-resolved forensics** — detect individual beaching events, quantify the pollution *pulse* after each (intertidal oil-sheen/discoloration, mangrove NDVI loss), link to vessel identities from NGO Shipbreaking Platform lists → an accountability ledger (vessel → date → pollution signature).
- **Approach:** Sentinel-2/1 change detection (Siamese/U-Net) on the plot-by-plot beach grid → per-event pre/post anomaly indices → align to vessel arrival records.
- **Data:** Sentinel-2/1; NGO Shipbreaking Platform lists; field pollution baselines. Verified (Planet optional).
- **Risks:** Tidal normalization needed; vessel attribution is timing-match, not AIS-confirmed.

### C5. Plastic choke-point flush forecaster (novelty 4)
- **Region:** Top emitting small urban rivers — Philippines (Pasig/Manila) or Jakarta's Ciliwung.
- **Novelty:** Global Plastic Watch maps *land* waste; Ocean Cleanup gives *static annual* emissions. Angle: detect **floating plastic-raft accumulation at choke-points**, correlate buildup with upstream rain, and **forecast flush timing** — a "when to deploy the boom" tool.
- **Approach:** Sentinel-2 Floating Debris Index + small CNN at fixed choke-point AOIs → accumulation time series → GPM IMERG rain + river stage → LSTM flush-event forecast.
- **Data:** Sentinel-2; Global Plastic Watch; Meijer 2021 outfalls (Figshare); GPM IMERG; HydroRIVERS. Verified.
- **Risks:** 10 m misses dispersed plastic (works for large visible rafts — the actionable case); 5-day cadence misses fast flushes → rain as leading indicator.

---

## D. LATIN AMERICA & MIDDLE EAST / CENTRAL ASIA

### D1. Salar Stress Index — lithium brine vs flamingo wetlands ⭐ (novelty 5)
- **Region:** Salar de Atacama (Chile) + Lithium Triangle (Argentina/Bolivia).
- **Problem:** Brine extraction draws ~1,750-1,950 L/s; Ramsar flamingo-breeding lagoons are shrinking. The pond-expansion ↔ lagoon-collapse link is asserted but not continuously quantified.
- **Novelty:** A **causal-lag attribution pipeline** connecting measured monthly evaporation-pond growth to peripheral lagoon area + littoral vegetation NDVI decline + InSAR subsidence, testing whether pond expansion **Granger-leads** habitat loss — an attribution tool, not a monitor. (Green-tech harming wetlands = strong story.)
- **Approach:** Sentinel-2 classify ponds vs lagoons + fringe NDVI → Sentinel-1 InSAR pumping subsidence → JRC Surface Water baseline → distributed-lag / Granger-causality regression with CHIRPS/ERA5 drought as confounders → per-lagoon Salar Stress Index (extraction- vs climate-attributed).
- **Data:** Sentinel-2; Sentinel-1; JRC Global Surface Water; Copernicus DEM; flamingo abundance baseline (validation). Verified.
- **Risks:** Causal attribution is hard (separate drought from pumping); wet salt crust decorrelates InSAR → "evidence consistent with," not proof.

### D2. Amazon river-isolation forecaster ⭐ (novelty 5)
- **Region:** Brazilian + Peruvian Amazon (Rio Negro, Solimões, Madeira, Purus).
- **Problem:** Record 2023-24 droughts dropped levels >3 m, cutting navigation and isolating 130+ Indigenous communities from food/fuel/medicine. No forward map of *which settlements lose river access, and when*.
- **Novelty:** Combine altimetry reach levels + SAR open-water masks + a **river-network graph** to predict, weeks ahead, the **connectivity** of specific communities to ports/hospitals — model the transport network's failure, not just water level. An "isolation risk" layer per village.
- **Approach:** DAHITI/Hydroweb virtual-station levels (LSTM/Prophet forecast) → Sentinel-1 wetted-width → HydroSHEDS/OSM river graph + settlements → navigability-threshold reachability (NetworkX) → weeks-ahead isolation map.
- **Data:** DAHITI; Hydroweb.next; Sentinel-1; HydroSHEDS/HydroRIVERS; CHIRPS/ERA5; SWOT (optional). Verified.
- **Risks:** Navigability needs bathymetry → use width/stage proxies, validate vs 2023/24 isolation reports; incomplete settlement geolocation → OSM + populated-places.

### D3. Desert dust solar-soiling forecaster (novelty 4)
- **Region:** North Africa / Arabian Peninsula (Morocco Noor, Egypt Benban, Saudi/UAE PV).
- **Novelty:** Site-level soiling studies exist. Angle: a **MENA-wide, spatially continuous soiling-loss-rate nowcast** fusing aerosols + irradiance + wind + rain (= natural cleaning reset), predicting the optimal next-clean date per site (revenue vs water trade-off).
- **Approach:** Sentinel-5P UV-AI + MODIS/VIIRS AOD → deposition proxy; ERA5 wind + CHIRPS/IMERG rain (soiling reset) + NASA POWER GHI → soiling-accumulation state model + GBM/temporal-CNN → regional loss raster + cleaning schedule.
- **Data:** Sentinel-5P; MODIS MAIAC AOD; NASA POWER; Global Solar Atlas; ERA5; CHIRPS/IMERG. Verified.
- **Risks:** Ground-truth PV soiling is proprietary → calibrate to published rates; strong as a *relative* prioritization tool.

### D4. Caspian "next Aral" dust-emergence + habitat map (novelty 5)
- **Region:** Caspian Sea, esp. shallow NE shelf (Kazakhstan) + seal zones.
- **Problem:** Caspian level falling up to ~30 cm/yr since 2020; exposed seabed will emit salt + industrial-contaminant dust (Aral-style) and could cut seal habitat ~81%. Unfolding now, largely unmodeled.
- **Novelty:** A **forward hazard-emergence map**: intersect altimetry level with DEM/GEBCO bathymetry → annual newly-emergent seabed → detect when fresh sediment becomes **dust-active** (S5P UV-AI spikes + S1 dryness/roughness) → overlay seal haul-out zones. Deliberately sidesteps the saturated Aral-salinity topic.
- **Approach:** DAHITI/Hydroweb level + DEM/GEBCO → emergent-land masks → Sentinel-2 salt-crust/bare-soil indices → S5P UV-AI + ERA5 wind dust-event classifier → "new dust-source frontier" + habitat overlay.
- **Data:** DAHITI/Hydroweb; Copernicus DEM/GEBCO; Sentinel-2/1; Sentinel-5P; ERA5. Verified.
- **Risks:** Coarse nearshore bathymetry; seal locations from literature (approximate); dust-from-seabed link is emerging → validate vs observed UV-AI spikes.

### D5. Glacier-to-Tap dry-season water-tower shortfall (novelty 3)
- **Region:** Tropical Andes — Rímac/Lima, Cordillera Real/La Paz, Antisana/Quito.
- **Novelty:** Most work maps glacier area loss. Angle: predict the **melt-buffer depletion timeline** — translate snow/ice state into days-of-supply-buffer remaining at a named utility intake.
- **Approach:** Sentinel-2 NDSI snow/ice + Landsat baseline + RGI outlines + ERA5-Land melt energy + CHIRPS → XGBoost/LSTM mapping pre-dry-season state to dry-season minimum flow → transfer to ungauged intakes.
- **Data:** Sentinel-2; Landsat; RGI 7.0/GLIMS; ERA5-Land; CHIRPS. Verified.
- **Risks:** Tropical cloud → add Sentinel-1 wet-snow; sparse SENAMHI gauges → transfer uncertainty.

---

## E. HUMANITARIAN & CONFLICT
*(All ethically framed as monitoring for relief/accountability, not targeting.)*

### E1. Scorched-earth attribution — deliberate cropland destruction vs drought ⭐ (novelty 5)
- **Region:** Tigray/Amhara (Ethiopia), Darfur (Sudan).
- **Problem:** Cropland is destroyed deliberately as a famine tactic, but satellite cropland-loss can't separate war destruction from drought/abandonment — which matters hugely for famine response and accountability.
- **Novelty:** Per-field **attribution** via a climate **counterfactual** — classify loss as drought-consistent vs anomalous-given-climate (crop failed when neighbors' rainfall/NDVI were fine) AND coincident with ACLED violence + burn signals → a "likely deliberate destruction" map.
- **Approach:** Sentinel-2 NDVI/NBR per-field time series → CHIRPS expected-yield counterfactual regression → flag NDVI-collapse + NBR burn that climate doesn't predict → GBM classify drought-loss vs conflict-anomalous-loss with ACLED features.
- **Data:** Sentinel-2; CHIRPS; ACLED; FEWS NET. Verified.
- **Risks:** Probabilistic, never proof → "consistent with conflict"; exclude normal agricultural burning via phenology calendar.

### E2. Flood→road→population-stranded watchlist (novelty 4)
- **Region:** Sudan & South Sudan floodplains (Nile/Sobat); extensible to Sahel.
- **Novelty:** Not a flood map — fuse flood extent into the **road-network graph** to output "which settlements just became unreachable from the nearest functioning hub, and how many people are stranded," updated every Sentinel-1 pass. Matches the mentor's "pre-position responders" flavor.
- **Approach:** Sentinel-1 SAR water (Otsu + Siamese U-Net) → cut OSM/HOT road edges intersecting water → NetworkX connectivity from WFP hubs → WorldPop stranded population → ACLED insecurity overlay → ranked access-loss dashboard.
- **Data:** Sentinel-1; OSM/HOT roads (HDX); WorldPop; ACLED; WFP logistics. Verified.
- **Risks:** OSM road completeness varies; SAR misses flooding under canopy/urban.

### E3. Conflict-blackout → displacement early-warning (novelty 4)
- **Region:** Sudan (Khartoum/Gezira/Darfur); cross-check Tigray, Gaza.
- **Novelty:** Existing nightlight work reports aggregate dimming. Angle: **per-settlement, time-resolved blackout-event detection** fused with ACLED events and IOM DTM displacement → "did this town go dark, and did displacement spike within N weeks?" → a settlement-level watchlist.
- **Approach:** VIIRS Black Marble per-settlement radiance → Bayesian change-point blackout detection (moon/cloud corrected) → join ACLED + DTM → XGBoost P(blackout → displacement surge).
- **Data:** VIIRS VNP46A2/A3; ACLED; IOM DTM (HDX); WorldPop. Verified.
- **Risks:** Blackouts have many causes → risk indicator, not atrocity proof.

### E4. Famine market-microstructure nowcaster (novelty 4)
- **Region:** Sudan, South Sudan, Sahel.
- **Novelty:** Leave the slow IPC target behind — model **market-collapse microstructure directly**: price-volatility regime shifts, breakdown of spatial price co-movement between markets (route-collapse signature), staple substitution → nowcast food-access collapse weeks ahead of IPC.
- **Approach:** WFP price series → per-market volatility + cointegration-breakdown features → HMM/Bayesian change-point regime detection + ACLED + climate → LightGBM forecast of near-term IPC deterioration.
- **Data:** WFP Food Prices (HDX); FEWS NET prices + IPC; ACLED; CHIRPS/NDVI. Verified.
- **Risks:** Price gaps in worst-hit markets (missingness is informative); careful out-of-time backtesting.

### E5. SAR-coherence damage mapping for neglected Sudanese cities (novelty 4)
- **Region:** Sudan — Khartoum, El Fasher, Nyala, Wad Madani (under-mapped vs Gaza/Ukraine).
- **Novelty:** Coherence-change methods exist but aren't applied at scale to Sudan. Angle: **transfer** detectors trained on UNOSAT/xBD-labeled Gaza/Ukraine/Syria to unlabeled Sudanese cities (domain adaptation), fused with ACLED to validate.
- **Approach:** Sentinel-1 SLC coherence time series → pixel-wise t-test baseline + U-Net trained on UNOSAT/xBD (adversarial domain alignment) → ACLED validation → per-neighborhood destruction index.
- **Data:** Sentinel-1 SLC; UNOSAT (HDX); xBD/xView2; Maxar Open Data; Google/Microsoft buildings; ACLED. Verified.
- **Risks:** Heaviest pipeline (SLC coherence); no Sudan ground-truth → cautious accuracy, validate vs UNOSAT spot checks.

---

## F. NOVEL-MODALITY / CLEVER-METHOD

### F1. Radio-to-Risk — multilingual radio speech-NLP famine nowcasting ⭐ (novelty 5)
- **Region:** Somalia / Somali Horn (extensible to Hausa Niger/N. Nigeria).
- **Problem:** IPC/FEWS bulletins lag; community radio is the dominant rural channel discussing rain failure, livestock deaths, prices, displacement.
- **Novelty:** Mine a modality formal systems ignore — **local-language radio audio**. Pipeline: Whisper ASR fine-tuned for Somali/Hausa → LLM structured event extraction → temporal model predicting next IPC phase. Quantify radio's **lead-time over satellite-only**.
- **Approach:** Whisper-large fine-tuned on Common Voice Somali/Hausa → multilingual LLM event/aspect extraction (rain failure, livestock mortality, price spikes, displacement) → admin-2 weekly features → Temporal Fusion Transformer vs HFID IPC labels, with satellite/price baseline to beat.
- **Data:** Harmonized Food Insecurity Dataset (HFID, Nature Sci Data 2025, monthly admin-2 IPC); FEWS NET; Whisper + Common Voice Somali/Hausa; GDELT (baseline); ACLED. Verified — **except the radio audio itself**, which must be recorded live / partnered (the main risk).
- **Risks:** Audio acquisition is the bottleneck — scope to a curated corpus / one station; Somali/Hausa ASR WER → fine-tune + spot-check. High novelty, but plan the data first.

### F2. War-Economy Nightfall — causal staggered DiD of conflict on electrification (novelty 4)
- **Region:** Sudan, admin-2, 2019-2025 (pre-war baseline + civil war).
- **Novelty:** Sudan nightlights work is purely descriptive. Angle: a **Callaway-Sant'Anna staggered difference-in-differences event-study** with ACLED timing as staggered treatment + an explicit **Tobit correction for the rural detection floor** (answering a 2025 Nature critique). Nightlights as a causal *outcome*.
- **Approach:** Monthly admin-2 VNP46A3 radiance panel → treatment = first sustained ACLED violence month → CS-DiD event-study (dynamic dose-response) + Bayesian change-point outage dating + censored regression for rural floor.
- **Data:** VIIRS Black Marble (blackmarbler); ACLED; WorldPop. Verified.
- **Risks:** Rural detection floor (→ feature via censored regression); reverse causality from displacement (lag treatment); test parallel-trends with pre-period leads.

### F3. Ship-breaking footprint via frozen geospatial foundation model (novelty 4)
- **Region:** Sitakunda (Bangladesh); transfer test to Alang (India), Gadani (Pakistan).
- **Novelty:** Yards are visually chaotic so spectral indices fail. Angle: **freeze a geospatial foundation-model encoder (Prithvi-EO-2.0) and few-shot/linear-probe** with ~tens of labels (a rare, label-scarce, ~5-site-globally class — ideal frozen-backbone transfer stress test). Show the MAE backbone already encodes the dismantling-yard signature.
- **Approach:** Multi-temporal HLS/S2 stacks → frozen Prithvi encoder embeddings → lightweight decoder / prototypical few-shot head → annual footprint masks + expansion curve + Bangladesh→India/Pakistan transfer table.
- **Data:** Prithvi-EO-2.0-300M-TL (HuggingFace); Clay v1.5 (alt); Sentinel-2 (Planetary Computer); HLS; NGO Shipbreaking Platform + OSM (seed labels you digitize). Verified.
- **Risks:** Must hand-label tens of patches (the premise); 30 m HLS coarse → use 10 m S2; small global sample limits transfer set.

### F4. Transfer-learned deep SDM for East African bird decline (novelty 4)
- **Region:** Kenyan highlands (data-poor target; USA = data-rich source).
- **Novelty:** SatBird models satellite+eBird per-snapshot, USA & Kenya separately. Angle: **cross-domain transfer** of a multi-species joint deep SDM (shared species-embedding) USA→Kenya + a **learned observer-effort bias model** so sampling artifacts aren't mistaken for decline + eBird Trends as the decline target.
- **Approach:** Two-branch (CNN satellite + env MLP → shared species-embedding) joint SDM trained on USA → fine-tune to Kenya → observer-effort sub-model → compare predicted abundance vs eBird Trends + habitat covariates → community decline-risk maps.
- **Data:** eBird Status & Trends (ebirdst); GBIF; SatBird dataset (ships preprocessed Kenya tensors); Deep Multi-Species Embedding method. Verified.
- **Risks:** Limited Kenya Trends coverage → validate on a subset; effort model essential; habitat attribution correlational.

### F5. Colocation-informed metapopulation cholera spread (novelty 4)
- **Region:** Eastern DRC / Great Lakes corridor (Goma, Bukavu, Uvira), 2022-2025.
- **Novelty:** African cholera models use gravity/SIR or post-hoc genomics. Angle: use **Meta Colocation Maps** (empirical phone-derived mixing network) as the connectivity matrix of a metapopulation **SEIR with a water-reservoir compartment**, fit by particle-filter/EAKF, forecasting onset via network effective-distance.
- **Approach:** Health-zone SEIR + reservoir → colocation connectivity matrix → EAKF data assimilation on WHO weekly cases → effective-distance onset ranking → 2-4 wk vaccine/ORS pre-positioning map.
- **Data:** Meta Colocation Maps (DfG, approval needed); Meta Movement Range (HDX fallback); WHO AFRO cholera bulletins; WorldPop. Verified (colocation needs DfG approval).
- **Risks:** DfG approval time + patchy rural phone density (→ Movement Range/gravity blend); noisy WHO counts (model observation error); the reservoir compartment is what keeps it from being a generic respiratory clone.

---

## Cross-cutting notes
- **Cleanest path to start (all free Sentinel/GEE, no special access):** Mercury-plume forensics, Salar Stress Index, Sabotage-vs-Rust, Scorched-earth attribution, Amazon river-isolation, Desert-dust soiling, ship-breaking forensics.
- **Use ready InSAR products** (OPERA DISP-S1 / EGMS / LiCSAR) to avoid raw SLC processing — decisive for tailings-dam, subsidence, permafrost, volcano ideas.
- **Approval/acquisition gating (plan data first):** Meta Data-for-Good colocation (cholera), local-radio audio (Radio-to-Risk), Global Tailings Portal bulk download, ACLED + NASA Earthdata (quick free registration).
- **Brand-new labeled datasets = head starts:** Coral restoration soundscapes (2025), HFID food-insecurity (2025), SatBird Kenya tensors, BirdCLEF 2024, NOSDRA spill records, Amazon Mining Watch.
