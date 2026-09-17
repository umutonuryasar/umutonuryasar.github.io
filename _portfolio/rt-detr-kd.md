---
title: "Distilling RT-DETR on a 4 GB GPU"
excerpt: "A controlled RT-DETR distillation study: 11 configurations across five KD methods, with transformer-specific ideas tested against controls on a single 4 GB GPU."
collection: portfolio
classes: portfolio-project
author_profile: false
share: false
---

## Overview

This project asks a practical question: **which knowledge-distillation mechanisms actually help a lightweight RT-DETR student when the entire study has to fit on a 4 GB GPU?**

I built an **11-configuration controlled ablation across five KD methods**, including two transformer-specific contributions — Query-KD and Stage-Adaptive KD — and paired the central hypotheses with controls designed to isolate the mechanism being tested.

The useful result was not that a new method won. The controls changed the interpretation of the experiments.

## Experimental Design

The teacher is a ResNet-50 RT-DETR trained in the same setup; the student uses a ResNet-18 backbone with 15.9M parameters. Training uses a 30K COCO subset at 512 px, with a 2.5K selection split carved from the training pool so that COCO val2017 does not drive checkpoint selection.

The study covers **Logit-KD, Feature-KD, Channel-Wise Distillation (CWD), Query-KD, and Stage-Adaptive KD**. Four headline configurations are repeated at seeds 42/43/44; the remaining runs are treated as single-seed observations rather than ranked evidence.

Two ideas receive explicit controls:

- **Query-KD:** prediction-space Hungarian matching versus index truncation.
- **Stage-Adaptive KD:** cosine versus inverse-cosine schedules crossed with two λ values, separating schedule direction from distillation weight.

## Key Findings

**Query matching did not help.** Hungarian matching scored below the no-KD baseline, while the deliberately simpler index-truncation control performed better in every repeated seed.

**The apparent curriculum-direction effect was a λ effect.** Once schedule direction and λ were crossed in a 2×2 control, changing λ moved mAP by 0.012–0.023 while matched-λ schedule directions differed by roughly 0.001.

**Logit distillation hurt in both formulations tested.** Binary KL and softmax KL both fell below the corresponding single-seed baseline in this regime.

**KD reduced run-to-run variance.** The baseline measured ±0.0028 mAP across three seeds; the repeated KD configurations measured between ±0.0005 and ±0.0010, whether or not they improved the mean.

## Results

| Configuration | mAP@[.5:.95] (n=3) | Δ baseline |
| --- | ---: | ---: |
| Baseline (no KD) | 0.0388 ± 0.0028 | — |
| Query-KD, Hungarian matching *(proposed)* | 0.0377 ± 0.0007 | −0.0011 |
| Query-KD, index truncation *(control)* | 0.0448 ± 0.0010 | +0.0060 |
| **Stage-Adaptive, cosine, λ=22.51** *(λ-swap control)* | **0.0676 ± 0.0005** | **+0.0288 (+74%)** |

The highest-scoring configuration is the **λ-swap control**, not evidence that Stage-Adaptive KD is the best method. Other method families were not swept at the same λ, so the defensible claim is narrower: it is the best configuration found under this study's budget and search procedure.

## Deployment

The best configuration was benchmarked on the same RTX 3050 that constrained the study:

| Precision | mAP | FPS | Latency | Peak VRAM |
| --- | ---: | ---: | ---: | ---: |
| fp32 | 0.0672 | 95.6 ± 1.7 | 10.46 ms | 111.3 MB |
| **fp16** | **0.0672** | **160.8 ± 2.5** | **6.22 ms** | **64.0 MB** |

For this model, fp16 gives **1.68× throughput and 42% lower peak memory with no measurable mAP change**. That is a result for this checkpoint and evaluation setup, not a general claim that fp16 is lossless.

## Interpretation & Limits

The project is a **budget-regime study**, not a claim about RT-DETR distillation in general. It uses one teacher, a 30K COCO subset, 512 px inputs, and three seeds only for the configurations carrying the main claims. Absolute detection quality is intentionally modest; the value of the study is in matched controls, leakage-free model selection, measured variance, and reporting negative results rather than optimizing them away.

The main lesson is methodological: plausible transformer-specific mechanisms can look convincing until the control isolates what is actually producing the gain. Here, Hungarian query alignment did not survive that test, and the apparent curriculum effect reduced to distillation-weight magnitude.

## Tech Stack

<div class="project-tags">
  <span>Python</span>
  <span>PyTorch</span>
  <span>RT-DETR</span>
  <span>COCO</span>
  <span>Knowledge Distillation</span>
  <span>RTX 3050 · 4 GB</span>
</div>

## Links

- **Technical report:** [Distilling RT-DETR on a 4 GB GPU](/rt-detr-kd/)
- **Repository:** [github.com/umutonuryasar/rt-detr-kd](https://github.com/umutonuryasar/rt-detr-kd)
