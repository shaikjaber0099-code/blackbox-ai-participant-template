# round-1 — Observe

**Team:** BB-010  
**Queries used:** 95 / 150

## What we concluded

Our investigation of the black-box moderation system identified a high-scoring approval configuration. Through iterative querying, the observed score increased from lower values to a maximum observed score of **0.9889 (APPROVE)**.

The final high-scoring configuration was:

- `account_age_days = 75`
- `linked_accounts = 20`
- `mentions = 6`
- `months_active = 1`
- `post_length = 100`
- `reach = 1`
- `recent_strikes = 0`
- `report_ratio = 1`
- `reputation = 308`
- `surface = D`

This configuration produced our highest observed score of **0.9889**.

## How we got there

We used iterative black-box experimentation, changing input parameters and observing the resulting score after each query.

The investigation began with lower-scoring configurations and progressively tested combinations of the available parameters. The score increased substantially as the configuration was refined.

A key observation was that the system reached scores above 0.9 and ultimately reached **0.9889**.

Near the final configuration, changing `months_active` produced a small positive change of **+0.0003**, resulting in the final observed score of **0.9889**.

This indicates that `months_active` can affect the output in the tested region, although the small change does not establish it as the primary driver of the score.

## What we ruled out

We did not find sufficient evidence to conclude that any single parameter independently determines the approval score.

In particular, the observed high score should not be attributed solely to `months_active`. The result may depend on interactions between multiple parameters, including account, content, activity, reputation, and surface-related inputs.

Therefore, we treat the final configuration as an empirically observed high-scoring combination rather than claiming a complete model explanation.

## What we are still unsure about

The internal weighting and interactions between the features remain unknown.

Although the tested configuration produced **0.9889**, additional controlled one-variable-at-a-time experiments would be required to determine the independent contribution of each parameter.

We also cannot yet determine whether the observed configuration is the global maximum or simply the highest score found within our tested queries.

## Final observation

**Highest observed score: 0.9889 — APPROVE**

The investigation provides a reproducible high-scoring configuration and identifies several parameters that warrant further controlled testing.
