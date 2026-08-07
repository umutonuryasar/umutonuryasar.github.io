---
title: "What I Learned Distilling RT-DETR on a 4 GB GPU"
date: 2026-08-06
permalink: /posts/2026/08/rtdetr-kd-what-i-learned/
description: "An end-to-end knowledge distillation study on a single RTX 3050. Three novel claims, all three refuted by their own controls — and why that turned out to be the point."
mathjax: true
use_math: true
tags:
  - knowledge-distillation
  - object-detection
  - rt-detr
  - efficiency
  - negative-results
---

An end-to-end knowledge distillation study on a single RTX 3050. Three novel claims, all three refuted by their own controls — and why that turned out to be the point.

---

The final post in this series. I set out to distill RT-DETR into a smaller student on a 4 GB RTX 3050 laptop GPU, on a 30K COCO subset, as a self-contained tech report. I had three ideas I wanted to demonstrate. All three lost to their controls. That became the report.

## Three claims, three controls

I did not want a "look, KD helps" post. I wanted to test specific mechanisms, each against a matched control:

1. **Logit-KD:** softmax KL is the wrong loss for a sigmoid-trained detector; a binary formulation should win. → **Refuted.** Both logit variants *hurt* relative to baseline, and softmax was not worse than binary. The premise collapsed on both ends.
2. **Query-KD:** prediction-space Hungarian matching should beat naive index alignment (covered in [part two](/posts/2026/08/rtdetr-kd-queries/)). → **Refuted.** Index truncation won by +0.0071 mAP, n=3.
3. **Stage-adaptive feature-KD:** an "inverse curriculum" schedule should beat a standard cosine schedule. → **Refuted, and instructively so.** A 2×2 λ-swap showed the gain tracked the *magnitude* of the loss weight, not the schedule direction. The "inverse curriculum wins" story was a λ artifact.

That third one is the trap I'm most glad I caught. Two configs differed in *both* schedule and λ; the schedule got the credit that belonged to λ. Swapping λ across the pair separated the variables and killed the claim. If you only ever run your clever config at its clever λ, you will attribute wins to the wrong knob.

## What actually held up

Not everything was negative:

- **The best config beat baseline by 74%** (0.0676 vs 0.0388 val2017 mAP), tight across three seeds.
- **KD reduced run-to-run variance** — baseline ±0.0028, the KD configs an order of magnitude tighter.

But the honest caveat has to travel with that headline: the 0.0676 came from a **λ-swap control run**, not from the calibrated λ, and the other methods were never tried at that high λ. So the correct claim is *"best configuration found,"* not *"stage-adaptive is the best method."* Every other method might close the gap at matched λ — I didn't run it, so I don't get to say.

## Deployment: fp16 was free

On the same 4 GB card, I swept precision on the best checkpoint:

| Precision | mAP | FPS | Latency | Mem |
|---|---|---|---|---|
| fp32 | 0.0672 | 95.6 | 10.5 ms | 111 MB |
| fp16 | 0.0672 | 160.8 | 6.2 ms | 64 MB |

Same mAP, 1.7× throughput, half the memory. fp16 is a free lunch for this model. Getting there took two fixes: positional embeddings were hardcoded to float32 (silently breaking fp16), and the eval script could only run under autocast. Both are the kind of bug you only find when you actually run inference in the target precision instead of assuming it works.

## The meta-lessons

- **Controls are the deliverable.** Three refuted claims are not a failed project; they are three things the field can now stop guessing about in this regime. The credibility *is* the negative result.
- **λ magnitude is a confound that impersonates a method.** Calibrate it, then swap it, before you believe any "method X wins" narrative in KD.
- **Budget shapes conclusions.** 30K subset, 512px, a 0.142-mAP teacher, one GPU. Everything here is a statement about *this regime*, and I say so every time.

This is the same discipline behind my [CIFAR-v2 paper](#): the honest, nuanced version of a result is the one worth publishing, even — especially — when it's the version where you were wrong.

Full code, results table, and limitations: [rt-detr-kd repo](#).