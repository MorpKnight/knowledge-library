---
title: GCP GPU and TPU Research Environments
description: Provisioning, validation, lifecycle, and reproducibility lessons from research environments.
date: 2026-11-04
tags: [gcp, gpu, tpu, reproducibility]
publish: true
draft: false
---

## Purpose and scope

The workspace contains GPU environment work and TPU deployment learning notes. The GPU setup and checks discussed below are historical observations; the TPU deployment notes are a study guide and do not by themselves demonstrate a TPU run. Cloud quotas, capacity, APIs, prices, and CLI flags change. Consult current provider documentation and project policy before provisioning.

## A practical sequence

1. Identify the project, region and zone, accelerator, machine type, compatible boot disk, quota, cost and maximum runtime. Check availability before creating resources.
2. Separate experimental resources, credentials, data, and virtual environments from existing research systems. Record names and ownership without publishing project identifiers or secrets.
3. Create an appropriate machine and connect over authenticated SSH. Keep services bound to loopback when using an SSH tunnel; avoid exposing unauthenticated research interfaces publicly.
4. Install a compatible NVIDIA driver for the GPU, ensure the kernel module is loaded, and inspect `nvidia-smi`. A driver package being installed does not establish that its kernel module is running.
5. Create isolated Python environments and install compatible framework/CUDA wheels. Check dependencies and run a small computation that verifies the intended accelerator is actually used.
6. Freeze package versions, driver, hardware, code commit, model identity, input hashes, random seeds, configuration, and logs. A generic CUDA test does not prove a specific experiment works.
7. Preserve approved results and reproducibility material before shutdown. Stopping compute may leave disks and other resources billable; list each resource and verify deletion only when authorized.

In the historical GPU setup, the workspace record reports NVIDIA driver 580 and separate PyTorch and JAX environments; loading `nvidia` and `nvidia_uvm` was required before the GPU appeared to the diagnostic tool. Generic CUDA/JAX checks and `pip check` passed according to that setup record. These are environment checks, not proof of a project-specific training run or current VM inventory.

## TPU and capacity

The workspace learning notes compare direct Compute Engine TPU VMs, GKE, and older TPU API/queued-resource paths. Choice depends on workload, accelerator generation, runtime, provisioning mode, storage, and scheduling needs. Flex-start can be time-bounded and capacity-dependent; a maximum runtime and stopping behavior must be configured and checked. A stopped VM and a retained persistent disk have different lifecycles. Historical commands, availability, and quota examples should not be treated as current provider guarantees.

The separate [[infrastructure-homelab/2026-11-03-qwen-image-21-comfyui-gcp|Qwen image-generation experiment]] records a bounded GPU application example, including teardown. Do not reuse a past deletion authorization for a different resource.

## Evidence and references

- Local learning materials: `tpu-project/learning/07-gcp-tpu-deployment-cli.md` and `08-tpu-builders-getting-started.md`; their source review was dated August 2026. The TPU Builders source was program-specific, and its examples are historical.
- The historical GPU environment record is not included in this public draft; inspect the authorized original setup log for exact versions and commands before reproduction.
- [Cloud TPU documentation](https://cloud.google.com/tpu/docs), [GPU documentation](https://cloud.google.com/compute/docs/gpus), and [Compute Engine provisioning models](https://cloud.google.com/compute/docs/instances/provisioning-models) are starting points, not verification of current quota or pricing.
