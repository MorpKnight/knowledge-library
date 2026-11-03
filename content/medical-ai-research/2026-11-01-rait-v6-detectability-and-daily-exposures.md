---
title: RAIT v6 - Detectability and Daily Environmental Features
description: An evidence-bounded record of a retrospective analysis and simulation of uncertainty.
date: 2026-11-04
tags: [experiment, statistics, calibration, medical-research]
publish: true
draft: false
---

## Research question and status

The v6 analysis, completed in early November 2026, asked whether counts of high-PM2.5 days and day-to-day temperature variation carried information beyond average environmental conditions, and how well the chosen methods detected known hypothetical relationships. The v6 design was fixed before its run but after v1–v5 results were known: it is retrospective exploration. This note was written on 4 November. It summarizes the inspected local aggregate report and design; it is not an independent rerun.

## Inputs and operational definitions

| Analysis |                              Records | Exposure window                    | Outcome meaning                                                |
| -------- | -----------------------------------: | ---------------------------------- | -------------------------------------------------------------- |
| Findings |                                  787 | Days 2–8 before CXR                | Eight numerical CV/NLP finding scores, not validated diagnoses |
| BTA      | 219, including 25 recorded positives | Days 9–38 before the recorded test | Recorded binary test result, not TB incidence                  |

PM2.5 high days counted values strictly greater than **43.4838 µg/m³**, the 90th percentile of the Jakarta daily reference for 2018–2020. It is an operational relative threshold, not a clinical or WHO threshold. Temperature variability was sample standard deviation of daily mean temperatures (`ddof=1`) in each window. The design required all days to be available. The high-day model controlled for mean PM2.5, temperature, humidity, precipitation, and calendar; the temperature-SD model controlled mean weather and calendar. The BTA model also included age and gender. Selection, linkage, label and date semantics, unmeasured confounding, and spatial proxy uncertainty remained unresolved.

## Method and comparison

The real-data primary analysis used 999 calendar-block bootstrap replicates with 28-day blocks, and block-length and calendar alternatives as sensitivities. The analysis retained all prespecified features and outcomes rather than selecting winners after inspecting outcomes. Fractional QMLE and Firth/FLIC were used for the relevant outcome types; inspect the frozen protocol and design for the exact specification. Seed values and complete package versions belong to the run's frozen protocol and runtime snapshot; they are not reproduced in this public-facing summary.

The simulation specified 60 primary scenarios with 200 synthetic datasets each. True target coefficients were −1, −0.5, 0, 0.5, and 1 per raw feature SD. Simulations compared independent and shared-calendar dependence and used generic binary or bounded score-generating scenarios. Primary scenarios used 399 bootstrap refits, an approximation of the 999-refit real-data procedure. Eight 56-day-block sensitivity scenarios used 100 datasets; an additional paired check increased bootstrap draws for selected null scenarios. The simulated mean model was correct by construction, making this a conditional and favorable test, not proof of robustness to real-world misspecification.

## Observations and interpretation

- None of the **16 findings** or **2 BTA** main simultaneous bands excluded zero. This does not establish that the true associations are zero.
- In one null scenario, pointwise false detection was **11%** (22 of 200; Wilson 95% interval approximately 7.4%–16.1%), versus a nominal 5%. At 200 replicates the maximum Monte Carlo standard error for a proportion is approximately 3.54 percentage points. This is a scenario-specific calibration warning, not proof that the entire simultaneous family is miscalibrated.
- All 36 real-data point estimates succeeded. Of 62,928 real-data bootstrap fits, one failed. The report counted 12,800 synthetic datasets and 5,299,200 simulation bootstrap fits, of which 764 failed; failed fits were reported and not replaced. The 12,800 comprise 12,000 primary and 800 block-sensitivity datasets; the paired higher-draw checks reuse datasets.
- Independent checks reported 190 fits and 7,042 daily values or summaries, plus reconstruction of intervals and simulation summaries. This checks computation, not causality or clinical validity.

An initial engineering smoke test failed when an independent GLM checker did not converge with redundant calendar columns. The checker was amended using pivoted QR column selection and an explicit convergence requirement; the estimator and research hypotheses were not changed. The failed run was retained and its smoke output was not used as research evidence.

## What remains open

Before claiming reliable inferential calibration, test simultaneous coverage directly and examine methods that preserve temporal and cluster dependence. Prespecify sensitivity to date uncertainty, BTA misclassification, and regional exposure mismatch. These are proposed next steps, not results from v6. A small or non-significant coefficient cannot resolve unmeasured confounding, unknown selection, or clinical label validity.

## Evidence and access

- Inspected local aggregate sources: `rait-detectability-extremes-v6-20261101/aggregate_export/LAPORAN_V6.md`, `DESIGN.md`, and `RUN_AMENDMENT.md` under the workspace outputs.
- The analysis delivered allowlisted aggregate artifacts; patient-linked rows, synthetic row arrays, and bootstrap arrays are excluded from this note. Underlying controlled data and exact frozen inputs are needed to independently reproduce the run and may not be publicly available.
- Related: [[medical-ai-research/environmental-data-and-cxr-pairing|Environmental pairing]] and [[ai-machine-learning/structural-causal-models-and-counterfactuals|Causal questions]].
