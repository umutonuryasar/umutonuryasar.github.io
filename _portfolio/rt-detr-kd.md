---
title: "Distilling RT-DETR on a 4 GB GPU"
excerpt: "A controlled knowledge-distillation study on RT-DETR run end-to-end on a single RTX 3050. Three novel KD methods, each tested against a control run designed to isolate the mechanism it claimed credit for — all three refuted."
collection: portfolio
---

A knowledge-distillation study on RT-DETR, run end to end on a laptop RTX 3050 with 4 GB of VRAM. Five KD methods, three of them novel, each tested against a control run designed to isolate the mechanism it claimed credit for.

## Key Findings

**All three novel methods failed their controls.** Query matching via Hungarian assignment lost to arbitrary index pairing. The curriculum-direction hypothesis turned out to track λ magnitude, not schedule direction. Logit distillation hurt in both formulations tested.

**The best configuration was a λ-swap control, not a novel method** — Stage-Adaptive KD at cosine schedule, λ=22.51, beat baseline by +74%.

**λ mattered more than any method-design choice under test** — an uncomfortable finding, since λ is the parameter most KD papers report in a footnote.

## Results

| Configuration | mAP@[.5:.95] (n=3) | Δ baseline |
|---|---|---|
| Baseline (no KD) | 0.0388 ± 0.0028 | — |
| Query-KD, Hungarian matching *(novel)* | 0.0377 ± 0.0007 | −0.0011 |
| Query-KD, index truncation *(control)* | 0.0448 ± 0.0010 | +0.0060 |
| **Stage-Adaptive, cosine, λ=22.51** *(λ-swap control)* | **0.0676 ± 0.0005** | **+0.0288 (+74%)** |

Deployed at fp16 on the same 4 GB GPU: 160.8 ± 2.5 FPS, 6.22 ms latency, 64.0 MB peak VRAM — no measurable accuracy cost versus fp32.

## Key topics

knowledge distillation · object detection · RT-DETR · DETR-family transformers · negative results · ablation study · edge deployment

## Tech stack

Python · PyTorch · RT-DETR · COCO · NVIDIA RTX 3050 (4 GB)

## Links

[Full Tech Report](/rt-detr-kd/) · [GitHub Repository](https://github.com/umutonuryasar/rt-detr-kd)
