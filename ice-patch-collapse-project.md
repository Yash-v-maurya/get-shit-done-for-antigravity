# Automated Satellite-AI Detection of Unstable Ice Patches for Cryo-Hydrological Hazard Early Warning in the Himalaya

**Project documentation / research concept note**
**Domain:** Remote Sensing · Geospatial Machine Learning · Natural Hazards
**Region:** Central Himalaya (India) — pilot basin: Bhagirathi/Srikanta around Dharali, Uttarkashi, Uttarakhand
**Level:** Undergraduate research internship → conference/journal paper

---

## 1. One-line thesis

> Existing research is **forensic** — it explains past ice-patch collapses *after* they occur. This project develops the **first automated satellite-AI pipeline** to (a) **detect** exposed, unstable ice patches in deglaciating nivation zones and (b) **map collapse-susceptibility** across a Himalayan basin, turning ISRO's identified early-warning signal into an operational, scalable screening system — *before* disasters occur.

---

## 2. Problem statement

Climate-driven deglaciation is exposing unstable patches of bare glacier ice on steep high-altitude slopes (nivation zones). When such a patch collapses, it releases a high-speed surge of ice, meltwater, and debris — a flash flood with **no warning**.

Critically, this hazard is **invisible to existing early-warning systems**:
- It is **not a cloudburst** (no triggering rainfall).
- It is **not a classic GLOF** (Glacial Lake Outburst Flood) — there is **no growing lake** to monitor as a precursor.

The 5 August 2025 **Dharali (Uttarkashi) flash flood** was traced by ISRO to exactly this mechanism: collapse of an exposed ice patch in the nivation zone of the **Srikanta Glacier**. The ISRO study (*npj Natural Hazards*, 2026) reconstructed the event after it happened and concluded that **exposed ice on steep slopes is an early-warning signal** — but stated the precursors "remain poorly constrained" and built **no predictive or automated detection system.**

**The gap this project fills:** there is currently no automated, scalable method to *find* such unstable ice patches across a region and *flag* the locations most likely to fail. That is the contribution.

---

## 3. Background and motivation (why now, why this is novel)

| Fact | Source |
|---|---|
| Dharali flash flood (Aug 2025) caused by ice-patch collapse, not cloudburst/GLOF | ISRO, *npj Natural Hazards* 2026 |
| Cryo-hydrological hazards under deglaciation are "emerging and under-recognized" | same |
| India has 7,500+ glacial lakes; ~2 million people downstream at risk | NDMA / Eos |
| GLOF (lake-based) prediction is heavily studied; ice-patch collapse has ~1 paper | literature scan |

**Novelty position:**
- Lake-based GLOF prediction → **saturated** (many ML papers: logistic regression, ANN, Bayesian NN, Fuzzy-AHP).
- Ice-patch / nivation-zone collapse → **brand-new hazard class (named 2026), one forensic paper, zero predictive/automated work.**

This is the right kind of gap for a publishable undergraduate paper: real-world urgency, a clearly identified opening, and a tractable scope.

---

## 4. Scope and claims (calibrated to evidence — read this carefully)

The reliability of the paper depends on claiming **only what the data can support.**

**What we claim (defensible):**
1. **Detection** — automatically locate exposed, unstable ice patches from satellite + terrain data. *Measurable with standard accuracy metrics.*
2. **Susceptibility mapping** — rank slope-units by how closely their conditions match those that preceded Dharali. *Validated as a case study + condition analysis.*
3. **Automation & scalability** — do across a whole basin what ISRO did by hand for one slope.

**What we do NOT claim (would get rejected):**
- ✗ Predicting the *exact time* a patch will collapse (only ~1 labeled event exists — impossible to validate).
- ✗ A live binary "collapse alarm."

**Framing:** this is a **screening / prioritization tool** ("monitor these N slopes closely"), analogous to landslide-susceptibility or seismic-hazard zoning — which reliably tell you *where to worry*, not *when* the event hits. Real-time timing = explicitly **future work**.

---

## 5. Objectives

1. Build an automated pipeline to detect exposed bare-ice patches in a Himalayan basin from open satellite data.
2. Derive terrain and deglaciation precursor features (slope, aspect, elevation, multi-year ice-area change).
3. Produce a **collapse-susceptibility map** combining ice detection + precursor features.
4. Validate against the Dharali/Srikanta event and known-stable control sites.
5. Quantify accuracy and explicitly characterize limitations.

---

## 6. Datasets (all free / open)

| Dataset | Role | Resolution | Access |
|---|---|---|---|
| **Sentinel-2 MSI** | Ice/snow/rock classification (NDSI, SWIR bands) | 10–20 m, 5-day | Copernicus / Google Earth Engine (GEE) |
| **Sentinel-1 SAR** | All-weather surface change, cloud penetration | 10 m | Copernicus / GEE |
| **Copernicus DEM (GLO-30)** | Slope, aspect, elevation | 30 m | OpenTopography / GEE |
| **ALOS PALSAR / HMA 8 m DEM** | Higher-res terrain (optional) | 8–12 m | OpenTopography / NSIDC |
| **Randolph Glacier Inventory (RGI 7.0)** | Glacier outlines (define study area) | vector | GLIMS / NSIDC |
| **Hi-MAG / HMA glacial-lake inventory** | Distinguish lake vs non-lake hazard | 30 m | Zenodo `10.5281/zenodo.4275164` |
| **ERA5-Land** | Temperature / melt forcing | ~9 km | Copernicus CDS |
| **News + public video/photo archives** | Event labels (Dharali; other reported sites) | — | manual compilation |

> Recommended platform: **Google Earth Engine** (free for research) — handles Sentinel + DEM at scale without downloading terabytes.

---

## 7. Step-by-step model approach

### Stage 0 — Study area & data preparation
- Define the pilot basin (Srikanta / Bhagirathi headwaters near Dharali) using RGI glacier outlines + a watershed boundary.
- Assemble cloud-free Sentinel-2 composites for the **ablation season (Jun–Sep)** across multiple years (e.g., 2017–2025).
- Co-register all layers to a common grid (e.g., 10 m).

### Stage 1 — Surface classification (detect bare ice)
- Compute spectral indices: **NDSI** (snow/ice), **NDWI**, SWIR ratios to separate **bare ice vs snow/firn vs rock vs debris**.
- Train a pixel classifier (**Random Forest** is the reliable baseline; optionally a small **U-Net / CNN** for spatial segmentation) on hand-labeled training polygons.
- Output: per-pixel class map → extract the **"exposed bare-ice" mask**.
- *Metric:* overall accuracy, per-class precision/recall, IoU on a held-out validation set.

### Stage 2 — Precursor feature engineering
For each ice-bearing slope-unit, derive the conditions ISRO linked to collapse:
- **Slope angle** (steep = unstable) — from DEM.
- **Aspect** (N/NE-facing matched Dharali) — from DEM.
- **Elevation** (nivation-zone altitude band) — from DEM.
- **Multi-year exposed-ice-area change** (thinning/shrinking trend) — Stage-1 mask differenced across years.
- **Distance to/absence of a glacial lake** (confirms non-GLOF mechanism) — from Hi-MAG.
- **Melt-season temperature** — ERA5-Land.
- *(optional)* SAR coherence loss = surface disturbance — Sentinel-1.

### Stage 3 — Susceptibility scoring
Two routes, pick per ambition:
- **3a. Knowledge-driven (robust, interpretable):** weighted index of the precursor features (e.g., **AHP / Fuzzy-AHP**, the accepted method in GLOF literature). Produces a transparent 0–1 susceptibility score with no need for many training events.
- **3b. Data-driven (higher novelty):** train a classifier (**Random Forest / XGBoost**) where the positive class = the Dharali precursor conditions and confirmed-stable slopes = negatives; use **probability output as susceptibility**. Use **feature importance / SHAP** to show *which* conditions drive risk.
- Output: a basin-wide **collapse-susceptibility map** (low → high).

### Stage 4 — Validation
- **Positive check:** does the Srikanta/Dharali ice patch fall in the **high-susceptibility** class in *pre-event* imagery? (the key result)
- **Negative/control check:** stable glaciated slopes with exposed ice that did *not* collapse should score lower → demonstrates discrimination, not just "all ice = high."
- **Temporal check:** run the model on 2017–2024 imagery; confirm the Dharali patch trended toward higher susceptibility before Aug 2025.
- **Sensitivity analysis:** vary feature weights/thresholds; report how stable the ranking is.
- *Metrics:* classification accuracy (Stage 1); ROC-AUC / success-rate curve for the susceptibility map (standard in landslide-susceptibility validation); qualitative case-study confirmation.

### Stage 5 — Output product & interpretation
- Deliver a **prioritization map**: "these N slope-units carry collapse-prone conditions — monitor closely."
- Discuss operational use (where authorities should focus ground/drone monitoring).
- Clearly state what the map does **not** say (timing).

---

## 8. Reliability & honest limitations (put this in the paper)

| Limitation | Mitigation / framing |
|---|---|
| **Very few labeled collapse events (~1: Dharali)** | Use *susceptibility* (condition-matching), not event-forecasting. Knowledge-driven scoring (3a) needs no large positive set. |
| **False positives** (ice ≠ guaranteed collapse) | Frame as screening/prioritization, not a binary alarm. |
| **No collapse-timing capability** | Explicitly future work. |
| **Cloud cover / seasonal snow masks ice** | Use ablation-season composites + Sentinel-1 SAR as backup. |
| **DEM resolution limits steep micro-terrain** | Use highest-res DEM available (ALOS/HMA 8 m); note as constraint. |
| **Single-basin pilot** | Frame as proof-of-concept; multi-basin = future work. |

**Reliability summary:** reliable as a **detection + screening** system with quantified accuracy; *not* a deterministic collapse predictor — and the paper succeeds precisely because it does not overclaim.

---

## 9. Expected outcomes / contributions

1. First **automated** detector of exposed unstable ice patches for this hazard class.
2. First basin-scale **collapse-susceptibility map** for nivation-zone ice patches.
3. Operationalization of ISRO's qualitative "early-warning signal" into a quantitative, scalable product.
4. An open, reproducible GEE/Python workflow others can extend to new basins.

---

## 10. Suggested tech stack

- **Google Earth Engine** (Python API / `geemap`) — data access + large-area processing
- **Python**: `numpy`, `pandas`, `rasterio`, `geopandas`, `scikit-learn`, `xgboost`
- **Deep learning (optional)**: `PyTorch` / `TensorFlow` for U-Net segmentation
- **Explainability**: `SHAP`
- **Visualization**: `matplotlib`, `QGIS` for final maps

---

## 11. Indicative timeline (8–12 week internship)

| Weeks | Work |
|---|---|
| 1–2 | Literature lock-in, study-area + data setup in GEE |
| 3–4 | Stage 1: ice/snow/rock classification + validation |
| 5–6 | Stage 2–3: precursor features + susceptibility model |
| 7–8 | Stage 4: validation against Dharali + controls |
| 9–10 | Maps, figures, sensitivity analysis |
| 11–12 | Paper writing + submission draft |

---

## 12. Candidate publication venues

- *npj Natural Hazards*, *Natural Hazards* (Springer), *Remote Sensing* (MDPI), *Journal of Mountain Science*
- Conferences: IGARSS, ISRS (Indian Society of Remote Sensing), regional climate/hazard symposia

---

## 13. Key references

1. ISRO et al. (2026). *Ice-patch collapse and early-warning implications from a Himalayan flash flood: emerging cryo-hydrological hazards under deglaciation.* npj Natural Hazards. https://www.nature.com/articles/s44304-026-00191-x
2. The Tribune (2026). *ISRO study identifies ice-patch collapse as cause of August 2025 Uttarkashi flash flood.*
3. ML-based probabilistic prediction of glacial lake formation (2025). Scientific Reports. https://www.nature.com/articles/s41598-025-17401-7
4. Fuzzy-AHP susceptibility assessment of GLOFs in the Indian Himalaya (2026). Applied Geomatics. https://link.springer.com/article/10.1007/s12518-026-00698-y
5. Annual 30 m dataset for glacial lakes in HMA 2008–2017 (Hi-MAG). ESSD. https://essd.copernicus.org/articles/13/741/2021/
6. Automated satellite-based glacial lake inventory and change detection in HMA (2026). Scientific Reports. https://www.nature.com/articles/s41598-026-35446-0
7. A Monitoring Network for Mitigating Himalayan GLOFs (2025). BAMS.

---

*Prepared as an internship research concept note. Claims are deliberately calibrated to available evidence: the system performs detection and susceptibility screening, not deterministic event-time prediction.*
