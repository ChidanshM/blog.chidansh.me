---
title: Sensor Pruning Study Begins
week: 5
date: 2026-04-19
summary: Gen 3 tuning stabilised, first pruning runs underway
---

## What I worked on
- Continued hyperparameter tuning from W4 and reached a stable Gen 3 config:
  - EER has improved from initial runs but remains above the Gen 0's baseline.
  - Treating this as the working baseline for the sensor pruning study going forward.
- Designed the sensor pruning study structure:
  - Single-sensor removal, as it says removing 1 sensor at a time and measure the EER impact.
  - Anatomical grouping: upper body only, lower body only, extremities only.
- Ran an initial single-sensor removal experiments:
  - Some sensors show minimal EER impact when removed.
  - While others cause a sharp EER increase, carry identity-relevant information.

## What I learned
- Not all sensors contribute equally, early results suggest a small subset of sensors
  is responsible for most of the identity-relevant signal.
- EER degradation is not linear with sensor removal, removing certain sensors 
  has disproportionately large impact compared to others.

## Challenges
- Gen 3 EER is not yet at or below the Gen 0's baseline, which makes interpreting pruning
  results harder, it is difficult to define "acceptable degradation" relative to a 
  baseline that is itself not fully optimised.
- Running one experiment per sensor configuration is time-consuming, and results
  so far are incomplete.

## Next week
- Complete single-sensor removal experiments across all sensors.
- Begin anatomical group experiments.
- Extract Transformer attention weights and compare against pruning results.