---
title: Pipeline Repair & Bug Resolution
week: 2
date: 2026-03-28
summary: Codebase Audit & Bug Resolution
---

## What I worked on
- Moved from high-level observation to a full line-by-line audit of the codebase across all four generations (Gen 0-Gen 3)
- Resolved all 7 bugs in order of severity before running any new training:
    1. **Tensor shape transposition** :     	
        - `prepare_data.py` was saving tensors as (time × channels) but Conv1d expects (channels×time). 
        - Fixed by transposing before saving and removing the fragile shape-heuristic workaround from `data.py`.
        - All `.pt` files regenerated.
    

    2. **Embedding collapse (loss stuck at 0.5)** : 
        - Gen 3 training flatlined at loss=0.5, meaning the model produced random embeddings unable to separate any subjects.
        - Resolved by adding learning rate warmup for the Transformer and fixing corrupted inputs caused by the transpose bug.


    3. **Evaluation script importing dead Gen 0 code** :
        - `evaluate_metrics.py` was importing `SiameseFusion` from Gen 0, making it incompatible with Gen 3's
        - `HybridGaitTransformer`. Rewrote the evaluation script to correctly interface with Gen 3.


    4. **Checkpoint signatures not saved** : 
        - Gen 3 checkpoints were missing `model_signature` and `training_signature` keys, causing resume-from-checkpoint to always fail. Fixed by adding both keys at save time.


    5. **NaN values from Butterworth filter** : 
        - The high-pass filter was introducing NaN values on near-zero or signal-dropout segments. Added pre-filtering validation


    6. **Gen 2 cache bug** : 
        - `generate_cache` in Gen 2 had no transpose workaround, causing all files to be skipped as too short. Fixed by aligning cache logic with the corrected tensor orientation.


    7. **Insufficient batch diversity** : 
        - `batch_size=64` with 8 samples per class yielded only 8 subjects per batch, insufficient for stable BatchHard triplet mining.

## What I learned
- A single tensor shape mismatch can silently corrupt an entire training run without throwing
  an error the model trains, loss is logged, but nothing meaningful is being learned.
- Embedding collapse (loss = 0.5) is not always a model architecture problem; it can be
  entirely caused by bad data flowing in upstream.
- Fixing bugs in the right order matters : the tensor shape fix was a prerequisite for
  diagnosing whether the embedding collapse was a data issue or a model issue.

## Challenges
- Several bugs were silent, they didn't crash the pipeline, they just produced wrong results.
  This made them harder to catch than explicit errors.
- Regenerating all `.pt` files after the tensor fix was time-consuming but necessary to ensure
  no corrupted cached data persisted.

## Next week
- Verify Gen 0 and Gen 1 baselines reproduce correctly on the cleaned pipeline.
- Begin Gen 3 training runs and confirm loss no longer collapses.