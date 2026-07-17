---
title: "Student Capacity Moderates Knowledge Distillation Effectiveness"
excerpt: "Systematic study of knowledge distillation across ResNet teacher-student pairs on CIFAR-10. Identifies student capacity — not teacher-student accuracy gap — as the key moderating factor in KD effectiveness. Published as an arXiv preprint."
collection: portfolio
---

A systematic study of knowledge distillation (KD) effectiveness across three ResNet teacher-student capacity pairs on CIFAR-10, comparing Logit-KD and Feature-KD under fully reproducible conditions (3 seeds, mean ± std reported throughout).

## Key Findings

**Student capacity, not the teacher-student accuracy gap, is the primary moderating factor in KD effectiveness.** ResNet-34 students consistently benefit more from distillation than ResNet-18 students, even when gap magnitudes are comparable.

**Implementation correctness critically affects Feature-KD.** An unclipped projection-layer gradient suppresses Feature-KD performance and produces misleading comparisons with Logit-KD. After correction, Feature-KD matches or outperforms Logit-KD in two of three pairs.

## Results

| Teacher | Student | Logit-KD Δ | Feature-KD Δ | Best |
|---------|---------|------------|--------------|------|
| R34 (95.70%) | R18 (95.13%) | +0.00 pp | +0.18 pp | Feature |
| R50 (95.81%) | R18 (95.13%) | +0.21 pp | +0.08 pp | Logit |
| R50 (95.81%) | R34 (95.25%) | +0.21 pp | +0.30 pp | Feature |

All gains relative to the corresponding student baseline across seeds {0, 1, 2}.

## Key topics

temperature scaling · feature alignment · model compression · ResNet · ablation study · reproducibility

## Tech stack

Python · PyTorch · CIFAR-10 · ResNet-18/34/50 · NVIDIA A100

## Links

[GitHub Repository](https://github.com/umutonuryasar/kd-capacity-gap) · [arXiv Preprint](https://arxiv.org/abs/2605.31191) · [HuggingFace Space](https://huggingface.co/spaces/umutonuryasar/kd-capacity-gap)