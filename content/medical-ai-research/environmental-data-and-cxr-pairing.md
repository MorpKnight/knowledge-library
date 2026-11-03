---
title: Environmental Data and CXR Pairing
description: Process PM2.5 and weather data while preserving geographic, temporal, and measurement provenance.
date: 2026-11-04
tags: [environment, pm25, data-provenance]
publish: true
draft: false
---

## Question

How can weather and air-quality measurements be combined with chest X-ray records without treating a regional value as an individual's actual exposure? The workspace combines data engineering, provenance review, and exploratory research; it does not establish a causal exposure effect.

## From grids to a regional time series

The workspace's LGHAP v2 project processed daily PM2.5 grids for 2018–2020. Its documented procedure opens one monthly ZIP, extracts one daily NetCDF to temporary storage, reads the grid window intersecting the target region, applies a Natural Earth country geometry, computes an area-weighted average, and removes the temporary NetCDF. Output units are µg/m³. The result is a region-level summary, not every 1 km pixel or a person's exposure. Source-grid latitude coverage also limits the geographic interpretation.

The documented raw monthly archives were large; the project processed them remotely rather than storing all extracted grids on the laptop. This is an engineering choice, not evidence that the same historical cloud machine still exists.

## Pairing requires more than a date join

1. Establish what the medical date means, including any shifting, timezone, precision, and image-to-report crosswalk.
2. Establish the location represented by the clinical record and whether a station, grid, or administrative region is a defensible proxy.
3. Record the source product, spatial aggregation, time period, units, completeness, and extraction/version for every environmental field.
4. Choose exposure windows before examining outcomes; require adequate daily coverage instead of treating missing days as clean air.
5. Audit duplicates, join cardinality, missing values, boundary dates, and sensitivity to the location and time assumptions.
6. Retain a data dictionary and source flags with the derived table. A simplified analysis view must not discard the provenance needed to interpret its measurements.

The PadChest audit recorded missing pollutant values and differences between similarly named rainfall and precipitation fields. Those observations illustrate why two column names, matching values, or a complete-looking table do not by themselves establish identical semantics. AQI components, sampling periods, and units must be checked before comparing or applying an index.

## Limits

ERA5/Open-Meteo weather and LGHAP/CAMS-derived pollutant measures in the research workflow describe modeled or regional conditions. They do not record an individual's movement, indoor conditions, or personal exposure. Spatial mismatch, historical date uncertainty, missingness, selection, and confounding remain possible. A paired dataset is a starting point for analysis, not evidence of causation.

## Sources

- [LGHAP v2 paper](https://essd.copernicus.org/articles/16/2425/2024/) and [2018 archive](https://doi.org/10.5281/zenodo.8313603).
- Local evidence: `geo-pm25-data/README.md` and `geo-pm25-data/data/metadata_audits/padchest_column_review_20261028/REVIEW.md`.
- Related: [[medical-ai-research/cxr-datasets-labels-and-provenance|CXR provenance]] and [[medical-ai-research/2026-11-01-rait-v6-detectability-and-daily-exposures|RAIT v6]].
