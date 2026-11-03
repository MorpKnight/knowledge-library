---
title: CXR Dataset Provenance, Labels, and Linkage
description: How to interpret imaging cohorts, reports, labels, dates, and source metadata.
date: 2026-11-04
tags: [medical-imaging, data-provenance, tuberculosis]
publish: false
draft: true
---

## Why this matters

An image, report, model-derived finding, and laboratory result are different observations. Combining them requires both a defensible join and an understanding of what each field measures. A successful technical match does not itself validate a clinical label or establish an acquisition date.

## Questions to ask for each dataset

- What is the unit: patient, encounter, study, image, report, or extracted finding? Can one patient have several images or identifiers?
- What is the source of the label: laboratory test, report, clinician annotation, or algorithm? What reference standard, timing, and missingness apply?
- Is a date the image acquisition, report, de-identification-shifted date, or another event? Is site a verified facility or only an identifier prefix?
- Is a given collection image-only, text-only, or a linked multimodal cohort? Are crosswalks authorized and complete?
- What license, institutional restrictions, and aggregation rules apply to any derived output?

## Examples from the workspace

The DAXRay static review of 11 September 2026 describes a matched 470-record CXR-RAIT manifest with a seed-7 split of 373/49/48. This is the reviewed matched subset, not the size of the entire raw collection. The review noted that identifiers, the meaning of groups, and the clinical reference standard required confirmation; it also noted that target bounding boxes were unavailable. Image-level classification therefore does not establish lesion-localization validity. These are findings of the review of commit `818f9239d68f5c42d6751fb24d82f7bf64d57991`, not a new reproduction.

A 28 October 2026 read-only PadChest column audit described an enriched 160,845-row, 135-column table. It proposed a compact view but retained provenance and status information in an archive; the proposal did not change the source. Missing measurements must not silently become zeros. For example, the audit recorded missing PM2.5 values and three records whose AQI period-duration check was false. The audit concerns this specific derived table and should not be generalized to every PadChest release.

For report-only versus multimodal collections, verify that any image-to-report crosswalk, date semantics, and site mapping are authorized and documented before a weather join. Do not assume that a report date is an unshifted acquisition date. Environmental values are regional or modeled proxies rather than measured personal exposure; see [[medical-ai-research/environmental-data-and-cxr-pairing|environmental data and CXR pairing]].

## Review sequence

Inventory provenance and permissions; map identifiers and units; test one-to-many relationships and duplicates; confirm date semantics and timezone; quantify missingness; document label origin; and record the exact transformation and version. Preserve original data and report only approved aggregate outputs. If critical semantics cannot be confirmed, leave the join or its interpretation explicitly provisional.

## Evidence

- Local read-only review: `daxray-notes/notes/2026-09-11_pemahaman-awal-daxray.md`.
- Local audit: `geo-pm25-data/data/metadata_audits/padchest_column_review_20261028/REVIEW.md`.
- [[projects/monash-research|Monash Research]]; clinical records, individual rows, and private crosswalks are deliberately not reproduced here. Independent access to controlled evidence and permission are needed to verify the underlying joins.
