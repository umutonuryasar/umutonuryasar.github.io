---
title: "Distilling RT-DETR on a 4 GB GPU"
permalink: /rt-detr-kd/
excerpt: "A controlled knowledge-distillation study on RT-DETR — three novel methods, all refuted by their own control runs, reported as it happened."
author_profile: true
toc: true
toc_label: "Contents"
---

{% include rt-detr-series.html part=0 live=3 %}

A knowledge-distillation study on RT-DETR, run end to end on a laptop RTX 3050
with 4 GB of VRAM. Five KD methods, three of them my own, each tested against a
control run designed to isolate the mechanism it claimed credit for.

All three of my methods failed their controls. That is what this write-up is about.

[**Code, full ablation tables and limitations →**](https://github.com/umutonuryasar/rt-detr-kd)

## The question

RT-DETR reaches strong detection accuracy with a ResNet-50 backbone, but that
model is a poor fit for edge hardware. Swapping in ResNet-18 costs several mAP
with no principled way to recover them. Knowledge distillation is the usual
answer — but nearly all KD literature targets CNN detectors, and it is not
obvious how logit-level, feature-level and query-level distillation interact
with a transformer encoder-decoder.

So: which of them actually helps, under a fixed and honest budget?

## Setup

| | |
|---|---|
| Teacher | ResNet-50 RT-DETR, trained here, **0.142 mAP** |
| Student | ResNet-18, 15.9M params, deliberately simplified for 4 GB |
| Data | COCO 30K subset — 27.5K train, 2.5K held out for checkpoint selection |
| Reporting | val2017, evaluated once per run, never used for model selection |

## Results

Four configurations were repeated at three seeds. The spread *within* a
configuration is what makes the differences *between* them readable.

| Configuration | mAP@[.5:.95] (n=3) | Δ baseline |
|---|---|---|
| Baseline (no KD) | 0.0388 ± 0.0028 | — |
| Query-KD, Hungarian matching *(mine)* | 0.0377 ± 0.0007 | −0.0011 |
| Query-KD, index truncation *(control)* | 0.0448 ± 0.0010 | +0.0060 |
| **Stage-Adaptive, cosine, λ=22.51** *(λ-swap control)* | **0.0676 ± 0.0005** | **+0.0288 (+74%)** |

That top row is the λ-swap control, not the calibrated-λ run of the original
inverse-curriculum hypothesis — the win tracks λ magnitude, not schedule
direction. See "What the controls said" below.

Seven further configurations were run once each and are reported in the
repository as observations rather than as a ranking.

### What the controls said

**Query matching did not help.** I argued that decoder queries have no canonical
ordering, so student and teacher queries must be matched rather than paired by
index. Index pairing — the thing I called arbitrary — beat it by +0.0071, in
every seed, with within-configuration spread of ±0.001. Hungarian matching also
landed *below* not distilling at all.

**The curriculum direction did not matter.** A first pass suggested that
reversing the feature-then-logit schedule helped. Crossing the two schedules
against the two λ values in a 2×2 showed the effect was entirely λ: at matched
λ the two directions differ by ~0.001, while the λ difference is worth
0.012–0.023.

**λ mattered more than any method-design choice under test.** Which is
uncomfortable, because λ is the parameter most KD papers report in a footnote.

**Logit distillation hurt** in both formulations tested.

**KD reduced run-to-run variance.** Baseline ±0.0028; every KD configuration
measured at n=3 sat between ±0.0005 and ±0.0010, independent of whether it
improved the mean.

## Deployment

The best configuration, measured on the same 4 GB laptop GPU the study was
budgeted around, at the 512 px it was trained and evaluated at:

| Precision | mAP | FPS | Latency | Peak VRAM |
|---|---|---|---|---|
| fp32 | 0.0672 | 95.6 ± 1.7 | 10.46 ms | 111.3 MB |
| **fp16** | **0.0672** | **160.8 ± 2.5** | **6.22 ms** | **64.0 MB** |

fp16 is free for this model: 1.68× throughput, 42% less memory, no measurable
accuracy cost. Though with the student at 0.067 mAP there is not much
precision-sensitive signal left to lose — *lossless for this model* is
supported, *lossless in general* is not.

## The posts

**1. [I argued softmax KL was wrong for detection distillation. Then I ran the control.](/posts/2026/08/rt-detr-kd-sigmoid-softmax/)**
RT-DETR trains classification with sigmoid focal loss, so its logits never lived
on a simplex — which makes Hinton-style softmax KD a distortion. The derivation
is clean. The measurement found nothing. On the difference between an argument
being correct and being consequential.

**2. [I Fixed the "Correct" Way to Align Queries in DETR Distillation. It Lost.](/posts/2026/08/rt-detr-kd-queries/)**
The principled fix for Query-KD in RT-DETR was prediction-space Hungarian
matching. I implemented it, expected it to win, and it lost — reproducibly.
Here's what the control taught me.

**3. [What I Learned Distilling RT-DETR on a 4 GB GPU](/posts/2026/08/rt-detr-kd-what-i-learned/)**
An end-to-end knowledge distillation study on a single RTX 3050. Three novel
claims, all three refuted by their own controls — and why that turned out to
be the point.

## Scope

Single teacher, 30K subset, 512 px, three seeds on the configurations that carry
a claim. The absolute numbers are small; the study is about relative behaviour
under a fixed budget. λ was swept only within one method family, so the headline
number is *the best configuration found*, not *the best method*. Full
limitations are in the
[repository README](https://github.com/umutonuryasar/rt-detr-kd#limitations).
