---
title: Federated CXR Evaluation and Research Reproducibility
description: Lessons from evidence mapping, exploratory analyses, and retention of federated imaging results.
date: 2026-11-04
tags: [federated-learning, evaluation, reproducibility]
publish: true
draft: false
---

## Question

How can a federated-learning study communicate what its experiments show while acknowledging non-IID data, dependent observations, uncertainty, and unavailable artifacts? This note summarizes an October 2026 supervisor-review package; it does not assert that a journal accepted the work or that every historical run was reproduced.

## Evaluation concepts

Federated learning trains across participants while aggregating updates; data remaining at a participant is not by itself a complete privacy guarantee. Non-IID data can differ in prevalence, source, acquisition, label definitions, and patient mix. Comparisons should specify participant allocation, model, training budget, data splits, metric, seeds, and uncertainty. Image-level splits and patient-level splits answer different questions; leakage must be considered at the patient level where feasible.

For multilabel CXR, an aggregate metric can hide differences between common and rare findings. A useful evidence map links each reported table to source predictions or aggregate summaries, the calculation, the cohort definition, and the exact revision. Compare paired methods on the same eligible observations when appropriate, and report eligibility and missing or failed replicates.

## What the 18 October 2026 package actually says

The revision improved framing, added post-hoc analyses based on saved predictions, and documented artifact availability. The package says it did not train models, rerun checkpoint inference, create new labels, or incorporate September results. Its primary 23-label endpoint and nine Holm-adjusted tests were unchanged. Support-group intervals added on that date were exploratory. A 14-label patient-level interval was conditional on 1,983 bootstrap draws retaining all 14 labels; it is not a confirmatory superiority test.

The package distinguishes 521 original and 518 cleaned images in a cohort-metadata summary; those are specific to that manuscript's source material, not a universal count for all workspace experiments. Historical campaign checkpoints were deleted after original verification. Saved predictions, metrics, source and receipts remain, but a hash or historical receipt cannot regenerate deleted weights. Report-to-label fidelity, ascertainment, original annotation settings, authorship declarations, and permissions were still unresolved in that review. Simulated reviewer responses were internal review exercises, not editorial decisions.

## Reproducibility checklist

1. Define the claim, cohort and unit, primary outcome and planned contrasts before comparing models.
2. Preserve run manifests, configuration, code and data versions, seeds, and environment; state what cannot be shared.
3. Keep saved prediction analyses distinct from new model training and inference.
4. Report exploratory contrasts, multiplicity, eligible draws, and failed or excluded replicates transparently.
5. Preserve sufficient authorized artifacts and obtain permissions before releasing code, data, reports, or weights.
6. Explain what can be verified from aggregate data and what requires restricted inputs or missing checkpoints.

## Sources

- Local reviewed package: `fl-tb-journal-latex/revision-2026-10-18/README.md` and `REPRODUCIBILITY.md`.
- [FLamby](https://proceedings.neurips.cc/paper_files/paper/2022/hash/232eee8ef411a0a316efa298d7be3c2b-Abstract.html) and [FedScale](https://proceedings.mlr.press/v162/lai22a.html), referenced by that package.
- [[projects/federated-learning|Federated Learning for X-Ray Imaging]] and [[medical-ai-research/cxr-datasets-labels-and-provenance|CXR provenance]].
