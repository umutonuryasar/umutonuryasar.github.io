---
title: "I Fixed the 'Correct' Way to Align Queries in DETR Distillation. It Lost."
date: 2026-08-06
permalink: /posts/2026/08/rt-detr-kd-queries/
description: "The principled fix for Query-KD in RT-DETR was prediction-space Hungarian matching. I implemented it, expected it to win, and it lost — reproducibly. Here's what the control taught me."
mathjax: true
use_math: true
tags:
  - knowledge-distillation
  - object-detection
  - rt-detr
  - negative-results
---

{% include rt-detr-series.html part=2 live=3 %}

The principled fix for Query-KD in RT-DETR was prediction-space Hungarian matching. I implemented it, expected it to win, and it lost — reproducibly. Here's what the control taught me.

---
Part two of a series on distilling RT-DETR on a single 4 GB GPU. This one is about a fix I was confident in — and the control run that proved me wrong.

## The bug I found

DETR-family detectors carry a fixed set of object queries through the decoder. In the Query-KD component I inherited, the student was supervised query-by-query against the teacher using **index alignment**: student query *i* was matched to teacher query *i*.

That is naive, and I could explain exactly why. Object queries have no canonical order. Query slot 17 in the teacher and slot 17 in the student are not the same "thing" — they emerge from independent training and land on whatever role the optimization happened to assign them. Aligning by index supervises the student against a semantically arbitrary target.

The principled fix is **prediction-space Hungarian matching**: build a cost matrix from what each query actually predicts (class + box), solve the assignment, and distill matched pairs. This is the same logic DETR uses for its own label assignment. I was sure it would help.

## The result

Three seeds (42/43/44), same split, same teacher, 30K COCO subset, val2017 mAP:

| Config | mAP |
|---|---|
| Baseline (no KD) | 0.0388 ± 0.0028 |
| Query-KD, index | **0.0448 ± 0.0010** |
| Query-KD, Hungarian | 0.0377 ± 0.0007 |

Index truncation beat Hungarian matching by **+0.0071 mAP** — outside the error bars on both sides. Worse: the "correct" method (0.0377) didn't even clear the baseline (0.0388), while the naive one gave a clean lift. The claim I set out to demonstrate was refuted with statistical support.

## Why the wrong thing won (hypotheses, not conclusions)

I don't get to declare a mechanism from one ablation, but a few explanations are consistent with the data and worth stating as hypotheses:

- **Matching noise compounds.** My teacher is not strong (0.142 mAP on this subset). A weak teacher produces a noisy cost matrix, so Hungarian re-assigns queries to *wrong* partners every step, injecting bad targets throughout training.
- **Moving targets hurt convergence under a tight step budget.** Hungarian assignment changes each iteration; the student chases a supervision signal that keeps reshuffling. Index alignment is arbitrary but *stable*, and a stable-if-imperfect target may converge better when you can't afford many steps.
- **The teacher's slots may carry usable structure.** If the teacher's query ordering encodes soft positional regularities, index alignment lets the student inherit them for free — and matching throws that away.

The honest read is that a theoretically-correct method is not automatically the empirically-better one, especially in a low-budget regime where the "principled" version's assumptions (a good teacher, enough steps to absorb a shifting target) don't hold.

## The takeaway

I almost shipped Hungarian matching on intuition alone. The only reason I didn't is that I ran the control and reported it. That is the entire lesson: the intuition was defensible, the fix was well-motivated, and it still lost. Under budget, run the boring baseline against your clever idea before you believe the clever idea.

**Caveat.** One teacher, one 30K subset, 512px, single architecture. This says index alignment wins *in this regime* — not that Hungarian matching is a bad idea in general. At scale, with a strong teacher and a real step budget, the ordering could easily flip. If you distill DETR queries and can afford the runs, test both. Don't assume the principled one wins for free.

Code and full results: [rt-detr-kd repo](https://github.com/umutonuryasar/rt-detr-kd).
