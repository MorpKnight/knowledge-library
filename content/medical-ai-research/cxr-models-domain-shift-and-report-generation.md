---
title: CXR Models, Domain Shift, and Report Generation
description: Compare classification, detection, report generation, quantization, and distillation without conflating their evidence.
date: 2026-11-04
tags: [medical-imaging, domain-shift, quantization, distillation]
publish: true
draft: false
---

## Different tasks and different evidence

A CXR classifier predicts an image-level label, a detector predicts locations, and a vision-language model generates text. Success at one task does not establish success at the others. Report-text label agreement is an automated proxy; it does not independently prove diagnostic correctness or the safety of a clinical workflow.

## DAXRay and domain shift

A September 2026 static review describes a JAX/Flax NNX research library for CXR analysis with metadata and CNN baselines, Faster R-CNN inference and conversion of weights, and distribution-discrepancy measurements. The source domain was TBX11K and the target was CXR-RAIT. Preprocessing differed across the reviewed detector paths: one resized source images by stretching, while the target path preserved aspect ratio with padding. Such differences can themselves contribute to measured shift.

The review found MMD and CORAL used as discrepancy measurements, but not integrated as adaptation losses in the inspected training loop. Thus measuring a shift was not evidence that an adaptation method had been successfully trained. At that review, no new end-to-end training or clinical validation had been run as part of the static inspection. A later experiment, if any, must be evaluated against its own artifacts rather than inferred from this review.

## Report generation experiment

The inspected project README describes a Qwen3.5-9B multimodal teacher compared in FP16, INT8 and INT4, and a CheXAgent student trained with report-level and representation-level knowledge distillation. The workflow generates reports, obtains automated labels with X-Raydar, evaluates TB-14 agreement, and records latency and allocated VRAM. It describes reference-report translation and notes possible language-related label noise.

The README reports approximately 19 GB allocated VRAM for FP16 and 9 GB for INT4 on an NVIDIA L4 in its setup. These are project-reported measurements, not measurements repeated for this note; latency, quality, hardware, prompt and exact configuration affect any comparison. Lower precision trades memory against possible output and performance changes; it does not guarantee equivalent quality.

The paper workflow requires an approved institutional dataset, split definitions, case-level outputs and checkpoints for full numerical reproduction. Those materials are not publicly distributed. The project notes that the exact local Qwen model snapshot revision could not be recovered. An adapter and limited approved examples permit narrower checks, not reproduction of the paper metrics.

## Questions for future comparisons

Freeze data and patient-level splits; document preprocessing, prompts, revisions, precision, batch size, and timing; report per-label performance and uncertainty in addition to aggregate scores; audit translation and label extraction; compare across sites and subgroups when justified; and seek appropriate expert evaluation. Do not infer clinical readiness from a pipeline smoke test.

## Evidence

- Local static source: `daxray-notes/notes/2026-09-11_pemahaman-awal-daxray.md`, reviewed commit `818f9239d68f5c42d6751fb24d82f7bf64d57991`.
- Local project sources: `efficient-cxr-report-generation-tb-indonesia/README.md` and `docs/reproducibility.md`; reported results and release permissions require a separate publication review.
- [[medical-ai-research/cxr-datasets-labels-and-provenance|CXR labels and provenance]].
