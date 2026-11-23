---
title: "Student Capacity Moderates Knowledge Distillation Effectiveness"
excerpt: "Five-seed, leakage-free study of knowledge distillation across four ResNet teacher-student pairs on CIFAR-10. KD gains concentrate on higher-capacity students; v2 openly corrects v1. arXiv preprint."
collection: portfolio
---

A systematic study of knowledge distillation (KD) effectiveness across four ResNet teacher-student pairs on CIFAR-10, comparing Logit-KD and Feature-KD under a leakage-free protocol: hyperparameters and checkpoints selected on a held-out validation split, final configurations re-run with five seeds, and the test set used only for reporting. This page reflects **v2** of the preprint.

## Key Findings

**Student capacity is where the gains concentrate.** The only statistically significant KD gains belong to ResNet-34 students under Feature-KD (+0.19 and +0.21 pp, Welch *p* < 0.05). No gain for a ResNet-18 student is distinguishable from zero. Doubling teacher parameters at matched teacher accuracy (R50 vs. R101) leaves the R34 gain unchanged. Neither nominal result survives correction across all eight comparisons, so this is directionally consistent evidence, not proof.

**Feature-KD matches or outperforms Logit-KD in all four pairs.** Top-1 teacher-student agreement is flat across pairs, so the accuracy gains do not come from closer imitation.

**Architecture dominates KD.** Correcting the ResNet stem for 32×32 inputs is worth +5.5 to +7.2 pp, over 25× the largest KD gain.

**v2 corrects v1.** A controlled re-run showed the gradient-clipping bug blamed in v1 had no measurable effect; v1's larger gains came from test-set model selection.

## Results

| Teacher | Student | Logit-KD Δ | Feature-KD Δ | Best |
|---------|---------|------------|--------------|------|
| R34 (95.30%) | R18 (94.86%) | −0.08 pp | +0.13 pp | Feature |
| R50 (95.36%) | R18 (94.86%) | +0.06 pp | +0.10 pp | Feature |
| R50 (95.36%) | R34 (95.04%) | +0.08 pp | +0.19 pp* | Feature |
| R101 (95.37%) | R34 (95.04%) | +0.06 pp | +0.21 pp* | Feature |

Gains relative to the corresponding student baseline, mean over seeds {0, 1, 2, 3, 4}. * *p* < 0.05 (Welch) vs. baseline; no comparison survives Bonferroni correction across the eight tests.

## Key topics

leakage-free evaluation · feature alignment · model compression · ResNet · ablation study · reproducibility

## Tech stack

Python · PyTorch · CIFAR-10 · ResNet-18/34/50 · NVIDIA A100

## Links

[GitHub Repository](https://github.com/umutonuryasar/kd-capacity-gap) · [arXiv Preprint](https://arxiv.org/abs/2605.31191) · [HuggingFace Space](https://huggingface.co/spaces/umutonuryasar/kd-capacity-gap)