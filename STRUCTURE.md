# Master's Dissertation — Structure Blueprint

**Title (locked):** *Convolutional Neural Networks for Urban Autonomous Aerial Vehicle Situational Awareness: A Camera–Point Cloud Fusion Approach for Obstacle Detection and Tracking*

**Author:** Eric Ezequiel Yoshida de Lima — **Advisor:** Prof. Dr. Marcos R. O. A. Máximo (ITA) — **Co-advisor:** Prof. Dr. Stiven S. Dias (Embraer)

**Template:** ITA class, `\documentclass[msc, eng]{ita}` (master's, English). Portuguese *Resumo* + English *Abstract* both required.
**Master file:** `tese.tex`. **Target:** ~50 pages of body, highly technical.

---

## Organizing principle (advisor-aligned)

The thesis comprises **three distinct projects** (different problems, data, and metrics), so it uses a **per-project, self-contained structure** (a multi-study / "thesis by papers" layout): a shared/general front part, then one self-contained chapter per project (each with its own **Methodology + Results**), then a synthesizing conclusion. There is **no** single general "Results" chapter — results live with their project.

**The three projects:** (I) FUSION 2025 — global vs. local EKF (simulation); (II) TU Berlin — monocular person tracking (real UAV data); (III) AirSim — synthetic dataset + 3D detection pipeline.

## Cohesion strategy (avoid the "frankenstein" feel)

1. **Shared front part (Ch. 1–4)**: introduction, one common theoretical framework (Ch. 2), the SLR (Ch. 3), and the **pipeline hub** (Ch. 4) that locates all three projects on a single architecture.
2. **One vision, three contributions**: the overarching sense-and-avoid pipeline is the thread; each project is a contribution to it.
3. **Generality narrative**: the same EKF framework handles obstacles (3D, LiDAR; Ch. 5) and people (2D, monocular; Ch. 6) — a designed feature, not a coincidence.
4. **Cross-references** between project chapters (e.g., Ch. 6 *extends* Ch. 5; Ch. 6 is exactly the regime where the frames diverge per Ch. 5's theorem).
5. **Synthesizing conclusion (Ch. 8)** ties the three projects back to the pipeline.
6. **Unified notation** end-to-end.

---

## Chapter structure (8 chapters + 2 appendices)

Legend: 🟢 tracker content — **written here** · 🔵 detection content — **completed on the other PC** · ⚪ shared.

| # | Chapter | Role | Owner | ~pp |
|---|---------|------|-------|-----|
| 1 | Introduction | shared | ⚪ | 5 |
| 2 | Theoretical Background | shared (common framework) | ⚪ | 8 |
| 3 | Related Work and Research Gap (SLR) | shared | ⚪ | 7 |
| 4 | Perception Pipeline Architecture | shared (cohesion hub) | ⚪ | 5 |
| 5 | EKF Tracking in Global and Local Frames (FUSION) | **Project I** (methods+results) | 🟢 | 10 |
| 6 | Monocular Person Tracking from a Moving UAV (TU Berlin) | **Project II** (methods+results) | 🟢 | 9 |
| 7 | Synthetic Dataset and 3D Detection Pipeline (AirSim) | **Project III** (methods+results) | 🔵 | 9 |
| 8 | Conclusions and Future Work | shared (synthesis) | ⚪ | 4 |
| A | Detailed Jacobian and CRLB Derivations | — | 🟢 | — |
| B | Dataset Details and Additional Results | — | 🔵 | — |

**Each project chapter is self-contained:** Overview/Objectives → Methodology → Results → Discussion. Chapter labels: `cha:background` (2), `cha:pipeline` (4), `cha:tracking` (5), `cha:monocular` (6), `cha:airsim` (7).

### Source → chapter mapping
- **Defense slides** → Ch. 1 (framing, contributions), Ch. 3 (SLR, uniqueness matrix), Ch. 4 (pipeline).
- **FUSION 2025 paper + `tracking-comparison/` code** → Ch. 5 (methods + results), Appendix A.
- **TU Berlin article** → Ch. 6 (methods + results).
- **AirSim dataset + detection pipeline** → Ch. 7 (methods + results), Appendix B.

---

## Work split (this PC vs other PC)
- **Here:** 🟢 — Ch. 5 (done) and Ch. 6 (each with its own results), Appendix A, plus all ⚪ shared chapters (1–4, 8).
- **Other PC:** 🔵 — the entire Ch. 7 (AirSim dataset + detection methodology + detection results) and Appendix B. Marked `% [detection -- OTHER PC]`. Self-contained, so it does not interleave with the tracker chapters.

---

## Status
- **Written and compiling:** Ch.~1 (Introduction), Ch.~2 (Theoretical Background), Ch.~3 (Related Work \& Gap / SLR), and Ch.~5 (FUSION — EKF global/local, with the **equivalence Proposition + proof**, the divergence regimes, CRLB, and 3 figures in `Cap5/`). ~57-page PDF, 0 errors / 0 undefined refs.
- **Section-level skeletons (`% TODO`):** Ch.~4 (pipeline hub), Ch.~6 (TU Berlin), Ch.~7 (AirSim), Ch.~8 (conclusions), and the appendices.
- Ch.~5 figures were generated from the faithful Python ports (`/tmp/ch5_experiments.py`, `/tmp/fig53_pcrb.py`); regenerate from the real MATLAB if an "official" version is wanted.

## Key finding to carry through (Ch. 5)
The global and local EKF are **mathematically equivalent** under known platform pose + isotropic process noise (proven + verified to 1e-13). The published FUSION "global wins" (Global 0.181 / Local 0.297 m) was an artifact of a local filter that **omitted state reorientation**; the correct local filter equals global. The chapter presents this as an **equivalence theorem** + the regimes where they genuinely diverge (anisotropic noise; uncertain pose → Ch. 6). Affects the published paper — discuss framing with the advisor; confirm on MATLAB.

## Reconcile when writing
- Use "constant-velocity + white-acceleration", not "constant-acceleration".
- `tese.tex` metadata TODOs: PG program, concentration area, Pró-Reitor, examining board, defense date, FRD document number.

## Build
`make` runs `latexmk -pdf`. Output: `tese.pdf`. Bibliography: `Referencias/referencias.bib` (template samples + the tracking references added for Ch. 5).
