---
title: "DETRFlow — RT-DETR Engineering Pipeline"
excerpt: "An end-to-end RT-DETR engineering project spanning evaluation, training infrastructure, inference, API serving, deployment, and benchmarking."
collection: portfolio
classes: portfolio-project
author_profile: false
share: false
---

## Overview

**DETRFlow** is an end-to-end object detection system built around RT-DETR, designed to cover the engineering path from model evaluation and training infrastructure to inference, serving, deployment, and performance measurement.

Rather than treating a pretrained detector as a black-box demo, the project builds the surrounding ML system explicitly: COCO evaluation, configurable fine-tuning, mixed-precision training, inference utilities, a REST API, containerized serving, an interactive demo, and latency/throughput benchmarking.

## System Design

The repository separates the workflow into reusable components rather than concentrating it in a single notebook. The training and evaluation scripts operate on COCO-format data; the inference layer wraps RT-DETR behind a stable predictor interface; FastAPI exposes the model as a service; and a Gradio application provides an interactive deployment path.

The same pipeline can therefore be exercised at several levels: from Python inference, to reproducible COCO evaluation, to HTTP serving and a public-facing demo.

## Implementation

A key part of the project is implementing the detector-side training mechanics rather than delegating all loss construction to an upstream library.

**Hungarian matching** is implemented using a cost that combines classification probability, L1 box distance, and generalized IoU before solving the bipartite assignment with `linear_sum_assignment`. **SetCriterion** then computes the matched classification, L1, and GIoU losses, including explicit no-object weighting.

The training path adds bf16 mixed precision, gradient accumulation, cosine learning-rate scheduling, checkpointing, and resume support around the RT-DETR model. These pieces make the repository useful both as a transparent implementation reference and as a base for controlled detector experiments.

## Evaluation & Benchmarking

The pretrained RT-DETR-R50 checkpoint is evaluated on the **COCO 2017 validation split (5,000 images)** using the standard COCO detection metrics. The reproduced baseline reaches **47.9 AP**, with AP50 of 64.2 and AP75 of 52.0.

| Model | Dataset | AP | AP50 | AP75 |
| --- | --- | ---: | ---: | ---: |
| RT-DETR-R50 pretrained | COCO val2017 | **47.9** | 64.2 | 52.0 |

This is a **pretrained-baseline evaluation**, not a claim that DETRFlow improves the underlying RT-DETR model. The repository also contains a configurable fine-tuning path, but no completed fine-tuned result is presented here.

Benchmark tooling measures warm-start inference latency, p50/p95/p99 latency, throughput, and peak GPU memory, with FP16 support. Because these measurements depend strongly on hardware and runtime configuration, the repository retains the detailed benchmark output and commands rather than treating one machine-specific FPS number as a general model result.

## Deployment

The inference layer is exposed through a **FastAPI** service with health and prediction endpoints. The service accepts uploaded images, returns structured detections with labels, confidence scores, and bounding boxes, and can be started through Docker Compose.

A separate **Gradio** interface provides interactive image upload and confidence-threshold control and is deployed publicly through Hugging Face Spaces. This keeps the demo layer separate from the API and core inference code while exercising the same detector pipeline.

## Engineering Decisions

Several choices are deliberately aimed at keeping the system inspectable and reusable:

- training, evaluation, inference, API, and demo code are separated into distinct modules;
- Hungarian matching and detection losses are explicit rather than hidden behind the serving abstraction;
- evaluation uses the standard COCO API so the reproduced baseline can be checked against familiar metrics;
- mixed precision and gradient accumulation are configurable rather than hard-coded to one GPU setup;
- benchmark scripts report latency percentiles in addition to average FPS, making runtime behavior easier to inspect.

## Relationship to RT-DETR KD

DETRFlow provides the engineering foundation for the separate **RT-DETR knowledge-distillation study**: model loading, evaluation, detector losses, matching, and constrained-hardware experimentation all build on the same underlying RT-DETR workflow.

The distillation work is presented separately because its evidence is experimental rather than systems-oriented. See the [RT-DETR Knowledge Distillation technical report](/rt-detr-kd/) for the controlled KD experiments and results.

## Tech Stack

`PyTorch` · `RT-DETR` · `Hugging Face Transformers` · `FastAPI` · `Gradio` · `COCO API` · `Docker`

## Links

- **Repository:** [github.com/umutonuryasar/detrflow](https://github.com/umutonuryasar/detrflow)
- **Live demo:** [Hugging Face Spaces](https://huggingface.co/spaces/umutonuryasar/detrflow)
