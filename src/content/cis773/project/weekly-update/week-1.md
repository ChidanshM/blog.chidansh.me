---
title: "Re-Onboarding & Semester Planning"
week: 1
date: 2026-03-26
summary: "Reestablishing the research questiong"
---

## What I worked on
- Reestablished the core research question: how many IMU sensors are necessary for reliable gait-based identity verification? The underlying hypothesis is that a minimal sensor subset exists, are the ones that preserves authentication performance while meaningfully reducing hardware burden. Answering this has direct implications for real-world deployment where a full 16-sensor body area network is impractical.

- Reviewed last semester's documented results and logs to understand what worked and what didn't before writing any new code:
    1. Gen 0 (6 independent CNNs, TripletLoss) -> 6.66% EER; best result, achieved with a simpler architecture.
    2. Gen 1 (shared-weight Siamese CNN, BatchHard triplet loss) -> 14.88% EER; architectural complexity hurt.

- Finalised the 3phase implementation plan as submitted in the project proposal.

## What I learned:
- Increased architectural complexity does not guarantee better performance:
	- Gen 1's shared-weight CNN performed significantly worse than Gen 0's independent CNNs
	- because it lost sensor-specific information by treating all sensors identically.
- This failure directly shaped the Gen 3 design decision, where per-sensor representations must be fully extracted by the CNN before the Transformer performs any cross-sensor fusion.
- Last semester established that gait authentication works; this semester's real contribution is answering which sensors are actually driving it and refinging the algorithm.

## Challenges:
- Re-reading last semester's codebase after a break made it clear that the existing Gen 3 architecture has known unresolved bugs that need to be addressed before any new work can begin.

## Next week: 
- Full codebase audit, identify and resolve all known pipeline bugs, and confirm Gen 0 and Gen 1 baselines reproduce correctly before touching any new model code.
