# round-4 — Reconstruct

Team: BB-010
Queries used: 98 / 170

## What we concluded

Our reconstruction indicates that GK-04 is not governed by a simple linear scoring rule. The system exhibits a highly asymmetric response to specific inputs, with post_length acting as a strong numerical driver and surface acting as a dominant categorical switch.
The most striking observation is the surface-dependent cliff: with the same numerical inputs, the score changes from 0.9900 on surface D to only 0.0560 on surface C. This is a 0.9340 score difference produced by changing a single categorical variable.
At the same time, controlled post_length experiments show a clear monotonic increase from 0.9411 → 0.9695 → 0.9865 → 0.9897 as length increases from 26.5 → 50 → 85.5 → 100.
Together, these observations strongly suggest a reconstruction consisting of continuous feature effects combined with a categorical surface-dependent mechanism, rather than a uniform additive model.

## How we got there

We used 98 queries as controlled experiments rather than treating the output as a generic prediction problem.
First, we isolated post_length. With the remaining inputs fixed, increasing the length from 26.5 to 100 produced a consistent score increase. This established a strong numerical relationship rather than random score fluctuation.
Next, we performed a surface comparison while keeping the numerical configuration constant:
- A: 0.9872
- B: 0.9876
- C: 0.0560
- D: 0.9900
This experiment exposed the largest behavioral discontinuity in the entire observed dataset. The C result is particularly important because the surrounding surfaces remain near 0.99.
We then probed secondary numerical features. recent_strikes, reputation, and reach produced measurable negative effects, while months_active produced a comparatively small decrease. account_age_days showed a small positive effect.
The combined evidence allowed us to reconstruct the shape of the decision surface, rather than merely reproducing individual outputs.

## What we ruled out

We ruled out the hypothesis that every numerical feature contributes substantially or equally to the final score.
Controlled tests showed effectively no measurable score movement for linked_accounts, mentions, and report_ratio within the tested region. For example, changing mentions between 0, 3, and 6 retained a score of approximately 0.9900.
We also ruled out a purely smooth numerical explanation. Such a model cannot explain the transition from 0.9900 to 0.0560 when only surface changes.
Most importantly, we did not treat the high scores around 0.99 as evidence that the system simply approves everything. The existence of the surface-C cliff and the lower-score numerical probes demonstrates that the system contains distinct regions of behavior.

## What we are still unsure about

The exact mechanism behind surface C remains unresolved. We have established the effect experimentally, but the available observations do not reveal whether C represents a hard rule, a hidden routing condition, an interaction with another feature, or another internal mechanism.
We also cannot claim that our reconstructed relationships hold outside the tested parameter ranges. The evidence supports the observed local behavior, not an exact reproduction of the complete hidden model.
Our current reconstruction therefore prioritizes behavioral fidelity over unsupported assumptions: we know where the system changes, how strongly it changes, and which variables appear irrelevant in the tested region, while explicitly leaving the unexplained mechanism as an open investigation target.
