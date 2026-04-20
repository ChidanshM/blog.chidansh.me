---
title: First Gen 3 Training Run
week: 4
date: 2026-04-12
summary: Gen 3 trains without collapse, but EER needs improvement
---

## What I worked on
- Ran the first full Gen 3 training pass on the cohort.
    - Loss no longer stuck at 0.5,  embedding collapse confirmed resolved.
    - Training converged, loss curves showed consistent decrease across epochs.
- Evaluated the trained model using EER, results are above the Gen 0 and Gen 1 
  baseline indicating the architecture is learning but not yet competitive.
- Began hyperparameter tuning to close the gap:
    - Adjusted triplet loss margin
    - Experimented with learning rate warmup duration
    - Tested different embedding dimensions

## What I learned
- A training run that converges isnt the same as a training run that performs well,
  also means the loss going down is necessary but not sufficient. EER is the real signal.
- The Transformer needs more careful tuning than the CNN baselines did small changes
  in margin and learning rate have a larger effect on final EER than expected.
- Gen 3's longer window size (1000 samples vs Gen 0's 200) captures more gait
  context but also makes the model slower to converge.

## Challenges
- EER is not yet competitive with the Gen 0 or the Gen 1 the architecture is working
  but the configuration is not optimal yet.
- Hyperparameter tuning without a clear search strategy is slow, where each run takes
  significant time, making trial-and-error expensive.

## Next week
- Continue tuning Gen 3 to close the EER gap with Gen 0 anf the Gen 1.
- Run on the next cohort once the results are stable.
- Begin planning the sensor pruning study based on current Gen 3 performance.