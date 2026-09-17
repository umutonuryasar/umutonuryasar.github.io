---
title: "MicroCLIP — Testing SigLIP at Small Scale"
excerpt: "A from-scratch CLIP-style system for testing whether SigLIP's reported small-batch advantage over softmax persists in a controlled, single-GPU regime."
collection: portfolio
author_profile: false
---

## Overview

**MicroCLIP** is a from-scratch CLIP-style vision-language system built around one controlled question: whether the SigLIP sigmoid objective retains its reported small-batch advantage over softmax InfoNCE at small scale.

The system pairs a **ResNet-18 image encoder** with a **4-layer Transformer text encoder implemented from scratch**, a 16K BPE tokenizer trained on COCO captions, and a shared normalized embedding space. The full experiment was designed to fit within a single-GPU budget while remaining cheap enough to repeat across losses, batch sizes, and random seeds.

## Research Question

> Does the SigLIP sigmoid objective retain its reported small-batch advantage over softmax InfoNCE at small scale?

Rather than treating the implementation itself as the result, MicroCLIP uses it as an experimental harness for comparing the two objectives under the same architecture, data, training recipe, and evaluation pipeline.

## Experimental Setup

The main comparison trains on **COCO Captions** for 30 epochs and evaluates image–text retrieval on the COCO 5K validation split. Sigmoid and softmax objectives are compared at batch sizes **128, 256, and 512**. Batch sizes 128 and 512 use **three seeds (42/43/44)**; batch 256 is a single-seed intermediate point and is treated as indicative rather than conclusive.

Training uses a Colab A100 with bf16 autocast. Model initialization, data order, caption sampling, and augmentation are deterministic functions of the seed, epoch, and sample index.

## Results

The main result is a **small-scale non-replication of the expected SigLIP advantage**. Across the tested batch range, softmax InfoNCE matches or outperforms the sigmoid objective on COCO retrieval.

The clearest signal is image-to-text Recall@10:

| Loss | Batch | Seeds | I→T R@10 |
| --- | ---: | ---: | ---: |
| Sigmoid | 512 | 3 | 45.6 ± 0.1 |
| Softmax | 512 | 3 | **48.2 ± 0.2** |
| Sigmoid | 128 | 3 | 44.3 ± 2.0 |
| Softmax | 128 | 3 | **47.7 ± 0.3** |

Softmax leads by roughly **2.5 points at batch 512** and **3.4 points at batch 128**. The gap therefore widens rather than shrinks as batch size drops within this regime. Sigmoid is also substantially less stable at batch 128, with higher seed-to-seed variance across the retrieval metrics.

Batch size itself shows no strong effect between 128 and 512 for either objective. The batch-256 runs are single-seed, so they are not used to make an optimal-batch claim.

## Interpretation

These results are **not a refutation of SigLIP**. They show that its reported small-batch advantage does not reproduce in this particular small-data, small-model regime.

The tested batches (128–512) are all small by CLIP standards, and the experiment never reaches the data or batch scales at which SigLIP's advantage was originally demonstrated. The useful conclusion is therefore narrower: at this scale, switching from softmax to sigmoid does not compensate for the constraints of small-batch contrastive training, and in these runs it performs worse while becoming less stable at the smallest tested batch.

## Secondary Experiments

Single-seed ablations probe several recipe and architecture choices. A cosine learning-rate schedule outperforms a constant schedule in the tested setup; an untuned SGD configuration collapses to near-chance retrieval; initialization changes are comparatively small; and a ViT-Tiny image encoder underperforms ResNet-18 when trained from scratch on this data.

These ablations are **diagnostic rather than confirmatory** because they use one seed. Zero-shot CIFAR-10/100 evaluation is also near chance and high-variance at this scale, so it is reported in the repository for completeness but not used to support a project-level conclusion.

## Reproducibility

The main loss comparison uses three deterministic seeds, and model selection is based on the lowest validation-loss checkpoint rather than the highest retrieval score. The repository tracks the raw per-run and seed-grouped evaluation outputs used to generate the result tables, together with the training/evaluation harness and experiment configurations.

The canonical checkpoint is a representative softmax batch-512 run rather than the highest-scoring single run, avoiding selection on the final retrieval metric. Training curves and run metadata are available in Weights & Biases.

## Tech Stack

`PyTorch` · `ResNet-18` · `Transformer` · `Contrastive Learning` · `COCO Captions` · `Weights & Biases`

## Links

- **Repository:** [github.com/umutonuryasar/microclip](https://github.com/umutonuryasar/microclip)
- **Experiment tracking:** [Weights & Biases](https://wandb.ai/umutonuryasar-independent/microclip)
- **Raw results:** [results/](https://github.com/umutonuryasar/microclip/tree/main/results)
