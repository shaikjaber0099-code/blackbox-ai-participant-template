# round-2 — Investigate

**Team:** BB-010
**Queries used:** 98 / budget

## What we concluded

The Round 2 investigation focused on identifying which parameters produced the most significant changes in the hidden GK-04 approval score.

Across 98 controlled queries, two parameters stood out as the primary drivers of score variation:

1. **`post_length` — strongest observed score increaser**
2. **`surface` — strongest observed score decreaser**

### Primary Result 1 — `post_length` increased the score

`post_length` showed the strongest positive numerical relationship observed in the tested range.

| Post Length | Score | Decision |
|---:|---:|---|
| 26.5 | 0.9411 | APPROVE |
| 50 | 0.9695 | APPROVE |
| 85.5 | 0.9865 | APPROVE |
| 100 | 0.9897 | APPROVE |

Increasing `post_length` from **26.5 to 100** increased the observed score by approximately **+0.0486**.

The increase was not perfectly linear. The largest practical improvement occurred between **26.5 and 85.5**, after which the score began approaching a high-score region near 1.0.

Between `post_length = 50` and `100`, the observed score increased from **0.9695 to 0.9897**, giving an approximate local change of **+0.0004 score per unit**.

Therefore, within the tested range, **higher post length was the strongest observed numerical score increaser**.

![GK-04 Round 2 parameter impact analysis](./plots/parameter-impact-analysis.png)

### Primary Result 2 — `surface` caused the largest score decrease

`surface` produced the largest overall score variation observed during the investigation.

The numerical parameters were kept constant while only the categorical `surface` value was changed:

| Surface | Score | Decision |
|---|---:|---|
| A | 0.9872 | APPROVE |
| B | 0.9876 | APPROVE |
| C | 0.0560 | DECLINE |
| D | 0.9900 | APPROVE |

The difference between the best and worst surface was:

**0.9900 − 0.0560 = 0.9340**

Changing from **Surface D to Surface C** therefore produced an observed score drop of approximately **0.9340**, which was substantially larger than the numerical score changes produced by the other tested parameters.

This is a **categorical effect**, so a per-unit slope is not meaningful.

Surface D produced the **highest observed score (0.9900)**, while Surface C produced the **lowest observed score (0.0560)**.

This makes `surface` the **strongest overall score-decreasing factor observed in the tested query space**.

## How we got there

The investigation was performed through controlled parameter changes rather than relying only on random observations.

### 1. Establishing a high-score reference

A high-scoring configuration was identified around:

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

This configuration produced a score of approximately **0.9889**.

### 2. Testing numerical parameter sensitivity

Individual parameters were then varied while keeping the remaining inputs as constant as possible.

This showed that:

- Increasing `post_length` increased the score substantially.
- Increasing `recent_strikes` decreased the score.
- Increasing `reputation` decreased the score.
- Increasing `months_active` decreased the score.
- Increasing `reach` produced a smaller negative effect.
- Increasing `account_age_days` produced a small positive effect.

### 3. Testing categorical `surface`

The same numerical configuration was tested with surfaces A, B, C and D.

The results were:

- A → **0.9872**
- B → **0.9876**
- C → **0.0560**
- D → **0.9900**

This experiment isolated `surface` as the largest observed source of score variation.

### 4. Testing parameters with little visible effect

Additional controlled tests were performed on:

- `linked_accounts`
- `mentions`
- `report_ratio`

Within the tested high-score region, these parameters produced scores that were effectively unchanged at the displayed precision.

## What we ruled out

### `linked_accounts` as a major score driver

Values including 0, 10 and 20 linked accounts produced approximately the same high score in the controlled high-score region.

Therefore, we found **no measurable major effect** from `linked_accounts` in this region.

### `mentions` as a major score driver

Testing mentions around 0, 3 and 6 produced approximately **0.9900** under the controlled configuration.

Therefore, mentions were ruled out as a major contributor to the observed score variation in this region.

### `report_ratio` as a major score driver

Testing report-ratio values including 0, 0.5 and 1 produced approximately the same score in the controlled high-score region.

Therefore, `report_ratio` was not identified as a major score driver in the tested region.

### Surface-independent explanations for the largest score change

The large difference between **0.9900 and 0.0560** occurred when the numerical parameters were held constant and `surface` was changed.

This strongly indicates that the largest observed score change was associated with the categorical `surface` variable rather than a simultaneous numerical parameter change.

## What we are still unsure about

The relationships identified here are **empirical observations from the tested query space**, not guaranteed global model coefficients.

We are still unsure about:

- Whether `post_length` continues to increase the score outside the tested range.
- Whether the positive effect of `post_length` eventually saturates or reverses.
- Whether `surface` interacts with other parameters outside the tested configurations.
- Whether the apparently neutral parameters (`linked_accounts`, `mentions`, and `report_ratio`) become important in other regions of the input space.
- Whether combinations of parameters create nonlinear or interaction effects that were not isolated by our experiments.
- Whether the observed high score of **0.9900** represents the actual maximum score or only the highest score found within our tested queries.

### Final interpretation

The Round 2 investigation therefore identifies two dominant observations:

> **`post_length` was the strongest observed score increaser, while `surface` produced the largest observed score decrease.**

The other parameters explain smaller variations or showed no measurable effect within the tested region. The highest observed configuration achieved **0.9900 (APPROVE)**, while the lowest tested surface configuration produced **0.0560 (DECLINE)**.
