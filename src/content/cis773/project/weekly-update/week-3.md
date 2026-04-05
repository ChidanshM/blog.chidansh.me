---
title: Training Preparation & Architecture Review
week: 3
date: 2026-04-04
summary: Validating fixes and preparing for first Gen 3 training run
---

## What I worked on
- Verified the Gen 0 and the Gen 1, baselines do reproduce correctly on the new 
  cleaned pipeline.
- Resolved the remaining open issue from W2, insufficient batch diversity, 
  by increasing subjects per batch in the BalancedBatchSampler
- Reviewed and finalised the Gen 3 training configuration before the first run:
  - Loss: BatchHard triplet loss (margin=0.4)
  - Optimiser: learning rate warmup followed by cosine annealing
  - Batch: BalancedBatchSampler with corrected subject diversity

## What I learned
- Fixing bugs and validating fixes are two different things W2 resolved
  the root causes of embedding collapse, but those fixes are only proven once
  an actual training run completes without collapsing.
- Baseline reproduction on the cleaned pipeline is a necessary and important
  sanity check, so that if the Gen 0 and the Gen 1 no longer reproduce their 
  original EERs, something in the pipeline fix introduced a regression.

## Challenges
- Two open questions from W2 still cannot be answered without running training:
  1. Whether embedding collapse is fully resolved, which will only be confirmed 
     by watching the loss curve of the first Gen 3 run.
  2. Whether the corrected batch diversity is sufficient for stable BatchHard
     mining remains to be seen under real training conditions.

## Next week
- Run the first full Gen 3 training pass on G01.
- Monitor loss curves and embedding distributions to confirm collapse does not recur.
- If collapse persists, diagnose whether it is a data issue or a model configuration issue.