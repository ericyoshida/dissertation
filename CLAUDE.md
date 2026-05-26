# Master's Dissertation — Project Guide

This repository is the LaTeX source of Eric E. Y. de Lima's master's dissertation at ITA.

**Title (locked):** *Convolutional Neural Networks for Urban Autonomous Aerial Vehicle Situational Awareness: A Camera–Point Cloud Fusion Approach for Obstacle Detection and Tracking.*
**Advisor:** Prof. Dr. Marcos R. O. A. Máximo (ITA). **Co-advisor:** Prof. Dr. Stiven S. Dias (Embraer).

## Research context
The thesis grew out of three connected efforts on UAV sense-and-avoid perception, all within the FlyMov Engineering Research Center (ITA / Embraer / FAPESP):
1. **Tracking first** — an EKF tracker for a moving platform was developed and studied (global vs. local reference frames) and published at IEEE FUSION 2025. This is the validated, result-bearing core (Ch.5).
2. **TU Berlin exchange** — a three-month research stay adapted the same tracker to monocular person tracking from a real UAV (camera-only; ground-plane + known-height assumptions; explicit camera-pose-uncertainty propagation). Real quantitative results are still being finalized (Ch.6).
3. **AirSim dataset** — to overcome the scarcity of annotated 3D aerial data, a synthetic dataset was built in AirSim to train the detection front-end (YOLO 2D → frustum → PointNet 3D) of the pipeline (Ch.7).

The unifying idea is a single modular camera–point-cloud pipeline; the three efforts are its contributions — "one vision, three contributions."

## How to build
- `make` (runs `latexmk -pdf`), or `latexmk -pdf tese.tex`. Output: `tese.pdf` (gitignored — build it locally).
- Class: `\documentclass[msc, eng]{ita}` (master's, English). A Portuguese *Resumo* and an English *Abstract* are both required.
- Bibliography: `Referencias/referencias.bib`, abntcite author–year style — cite with `\cite{key}`.

## Structure (per-project, self-contained)
Shared front part, then one self-contained chapter per project (each = Overview → Methodology → Results → Discussion), then a synthesizing conclusion. **There is no single general "Results" chapter** — results live with their project.

- **Ch.1** Introduction · **Ch.2** Theoretical Background (shared framework) · **Ch.3** Related Work & Gap (SLR) · **Ch.4** Perception Pipeline (cohesion hub).
- **Ch.5** EKF Tracking in Global & Local Frames (FUSION; simulation) — **written**.
- **Ch.6** Monocular Person Tracking from a Moving UAV (TU Berlin; real data) — tracker work, written on the primary machine.
- **Ch.7** Synthetic Dataset & 3D Detection Pipeline (AirSim) — **the chapter to write on the data machine** (see workflow).
- **Ch.8** Conclusions. Appendix A (Jacobian/CRLB derivations), Appendix B (dataset details).

Full blueprint: **`STRUCTURE.md`** (in this repo) has the per-chapter detail, the source→chapter mapping, the metadata TODOs for `tese.tex` (PG program, area, Pró-Reitor, examining board, defense date, FRD number), and the items to reconcile when writing.

Chapter labels: `cha:background`(2), `cha:relatedwork`(3), `cha:pipeline`(4), `cha:tracking`(5), `cha:monocular`(6), `cha:airsim`(7), `cha:conclusions`(8).

## Source materials (NOT in this repo)
This repository contains only the dissertation text; the supporting materials are not cloned with it.
- On the **primary machine** (a separate `master-thesis-tracker` workspace): the FUSION 2025 article, the TU Berlin article draft, the `tracking-comparison` MATLAB code (the EKF implementation behind Ch.5), and the systematic-review documents behind Ch.3.
- On the **data machine**: the local AirSim and TU Berlin datasets and the detection code (for Ch.7).

If you need a source that is not in this repo, it lives on the other machine — ask the user rather than inventing facts, numbers, or references.

## Two-computer workflow
This dissertation is written across two machines sharing this repo over Git:
- **Primary machine:** tracker + shared chapters (Ch.1–6, 8) and Appendix A.
- **Data machine (this one, if the AirSim/TU Berlin data lives here):** **Ch.7 (`Cap7/cap7.tex`)** — the AirSim dataset, the detection methodology (YOLO 2D → frustum → PointNet 3D), and the detection results — using the **local** datasets. The datasets are **gitignored and stay local**; never commit them to this repo. Appendix B too.
- Each machine edits different chapter files, so `git pull` / `git push` merges cleanly. **Pull before editing; push when done.**

## Status
Written: Ch.1, Ch.2, Ch.3, Ch.5. Section-level skeletons (`\section` headings + `% TODO`): Ch.4, Ch.6, Ch.7, Ch.8, appendices. ~57-page PDF, builds with 0 errors.

## Conventions (keep consistent across chapters)
- Notation follows Ch.2 and is reused verbatim: EKF symbols `x, F, Q, G, H, P, K, S, ν, R`; rotation `R = R_z R_y R_x` (body→world); spherical measurement `ρ, α, β`. Reuse — do not redefine.
- A chapter's figures live in that chapter's own folder (`CapN/*.pdf`), included with `\includegraphics`. Figures so far were generated from faithful Python ports; regenerate "official" versions from the real code/data if desired.
- Cite real references only. Some niche UAV references in `referencias.bib` carry provisional titles flagged `verify in Zotero` — confirm them before final submission.

## Important facts — do NOT contradict
- **EKF equivalence (Ch.5):** the global- and local-frame EKF are *mathematically equivalent* under known platform pose + isotropic process noise (proven + verified to machine precision). The published FUSION "global wins" result was an artifact of a local filter that omitted the inter-frame state reorientation. **Do not reintroduce "global beats local" as a headline result.**
- The motion model is **constant-velocity with white-acceleration process noise** — not "constant-acceleration".

## When writing Ch.7 (AirSim / detection) on the data machine
Fill `Cap7/cap7.tex`: (1) the AirSim synthetic dataset — scenarios, sensor channels, annotations, splits; (2) detection methodology — 2D detection, frustum proposal, PointNet 3D; (3) detection results — 2D/3D metrics, generalization. Keep it self-contained and consistent with the pipeline of Ch.4 (`cha:pipeline`) and the foundations of Ch.2 (`cha:background`). Do not edit the tracker chapters (5, 6) unless coordinating with the primary machine.
