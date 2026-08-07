---
title: "I Argued Softmax KL Was Wrong for Detection Distillation. Then I Ran the Control."
date: 2026-08-06
permalink: /posts/2026/08/rt-detr-kd-sigmoid-softmax/
description: "Argued sigmoid-trained RT-DETR logits need binary KL, not softmax KL, for distillation. Ran the softmax control anyway — it won. A case study in what a control run catches that a clean derivation doesn't."
tags:
  - knowledge distillation
  - RT-DETR
  - machine learning
---

{% include rt-detr-series.html part=1 live=3 %}

Argued sigmoid-trained RT-DETR logits need binary KL, not softmax KL, for distillation. Ran the softmax control anyway — it won. A case study in what a control run catches that a clean derivation doesn't.

---

# I Argued Softmax KL Was Wrong for Detection Distillation. Then I Ran the Control.

There is a specific kind of argument that feels like it cannot be wrong. It
starts from something true about how a model was trained, follows a short chain
of reasoning, and arrives at a conclusion that seems forced rather than chosen.
You do not test conclusions like that. You implement them.

I made one of those arguments while building a knowledge-distillation study for
RT-DETR. I was fairly sure of it. I also, almost as an afterthought, kept the
thing I was arguing against as a control run.

The control won. Sort of. This is the more interesting outcome, so here is the
whole thing.

## The argument

Classic knowledge distillation, in the Hinton formulation, matches the
student's class distribution to the teacher's:

$$\mathcal{L} = T^2 \cdot \mathrm{KL}\big(\mathrm{softmax}(t/T) \,\|\, \mathrm{softmax}(s/T)\big)$$

That softmax is doing real work. It says the classes *compete*: probability mass
is conserved, and pushing one class up pushes the others down. This is exactly
right for an image classifier trained with cross-entropy, where the model
learned a categorical distribution over mutually exclusive labels. The "dark
knowledge" people talk about — the teacher's relative confidence across the
classes it *didn't* pick — lives in that shared normalization.

RT-DETR does not train its classifier that way. Like most of the DETR family it
uses **sigmoid focal loss**: each class logit is an independent binary score,
"is this object a dog?" asked eighty times in parallel. Nothing sums to one.
Nothing competes.

So applying softmax KL to those logits imposes a structure the model was never
trained under. You take eighty independent binary scores, force them into a
simplex they never lived in, and match distributions in that invented space. It
is not obviously catastrophic — the ordering information survives — but it is
distorting a signal for no reason.

The fix follows immediately: match the distributions in the family the logits
actually belong to. Per-class binary KL between temperature-scaled sigmoid
probabilities:

$$\mathcal{L} = T^2 \cdot \frac{1}{|BQC|}\sum_{b,q,c} \mathrm{KL}_{\text{bin}}\big(\sigma(t_{bqc}/T) \,\|\, \sigma(s_{bqc}/T)\big)$$

Implemented stably as binary-cross-entropy-with-logits minus the teacher's
entropy — the entropy term carries no student gradient but makes the quantity a
true KL, which is worth having for logging.

I made this the default. I kept softmax as `--logit-mode softmax`, an ablation
row, mostly for completeness.

## The control

Here is the part I want to dwell on, because it is the only reason this post
exists.

Every claim in the study got a control run — not a comparison against a
baseline, but a run designed to isolate the specific mechanism I was claiming
credit for. If I said matching matters, I ran the version without matching. If
I said the curriculum direction matters, I ran the reversed curriculum. And if I
said the sigmoid-matched formulation is the right one, I ran the softmax
formulation under otherwise identical conditions.

The point of a control is not to be fair to the alternative. It is to make the
claim falsifiable at all. "My method beats no-KD" is compatible with almost any
story about *why*. "My method beats the version of my method with the
mechanism removed" is not.

Both logit runs used the same student, same teacher, same seed, same schedule,
and per-method λ values calibrated by the same rule — each KD term scaled so it
starts at the same magnitude as the detection loss.

## What happened

| Configuration | λ | mAP@[.5:.95] |
|---|---|---|
| Baseline (no KD) | — | 0.0419 |
| Logit-KD, binary KL *(the "correct" one)* | 24.23 | 0.0348 |
| Logit-KD, softmax KL *(the control)* | 5.317 | 0.0374 |

*(This baseline is a single seed — 0.0419 — not the 3-seed 0.0388 ± 0.0028
baseline reported elsewhere in this series; the two numbers aren't directly
comparable.)*

Two things, in order of importance.

**Logit distillation hurt.** Not "helped less than hoped" — both variants landed
below the no-distillation baseline. Whatever the teacher's class scores were
transferring, the student was better off without it.

**The predicted advantage did not appear.** The formulation I argued was
principled scored *lower* than the one I argued was distorting the signal.

Now the caveat that stops this from being a clean reversal: these are single-seed
runs. The gap between them, 0.0026, is smaller than the seed-to-seed spread I
measured on the baseline configuration (±0.0028 across three seeds). So I cannot
claim softmax beat binary. What I can claim is narrower and, honestly, worse for
my argument: **the effect I predicted was large enough to matter did not show up
at all.** I expected to see the principled formulation separate from the
distorting one. It did not separate in either direction.

## What I can't conclude

The argument might still be right. Several things could be hiding it:

**The teacher is weak.** It scores 0.142 mAP. Distilling class distributions
from a mediocre classifier transfers its errors along with its knowledge, and
with λ calibrated to put the KD term on par with the detection loss, that noise
competes directly against ground truth. A formulation argument may simply be
invisible underneath a teacher-quality problem.

**λ was never swept for this pair.** The calibration rule equalizes the KD
magnitude *at initialization*. Elsewhere in the same study I learned the hard
way that equal-at-init is not equal-over-training: a curriculum result I thought
was about schedule direction turned out to be entirely about λ, and only a
crossed 2×2 revealed it. I never ran that 2×2 for binary-versus-softmax. The
two λ values here differ by 4.5×.

**The regime is small.** A 15.9M student on a 30K-image subset lands at ~0.04
mAP. Differences that would be visible in a stronger regime can sit inside the
noise floor here.

Any of those would rescue the argument. None of them is evidence *for* it. That
distinction is the whole game: "my result is consistent with my hypothesis being
true under conditions I did not test" is not support, it is an alibi.

## The part worth keeping

The reasoning in the first section is still, as far as I can tell, correct about
what the loss functions do. Sigmoid-trained logits really do not live on a
simplex. Softmax KL really does impose structure that is not there.

What I got wrong was the step from *this is a distortion* to *removing it will
measurably help*. Those are different claims, and only the second one is about
the world. Loss-function arguments are seductive precisely because the first
kind feels like it entails the second. It does not. A distortion can be real and
inert.

The cheapest thing I did in this entire study was keep the alternative I had
already dismissed as an ablation row. It cost one extra training run. Without
it, I would have shipped a default, a clean derivation for why it was correct,
and no idea that the mechanism I was claiming did nothing — while the whole
technique quietly made the model worse.

Run the control. Especially when you are sure.

---

*Part of a series on distilling RT-DETR on a 4 GB GPU. Code, full ablation
tables and limitations: [github.com/umutonuryasar/rt-detr-kd](https://github.com/umutonuryasar/rt-detr-kd).
Next: what happened when I argued that decoder queries need proper matching —
and ran that control too.*
