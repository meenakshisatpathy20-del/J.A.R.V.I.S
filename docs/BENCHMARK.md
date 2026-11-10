# Blind benchmark: J.A.R.V.I.S. vs simple baselines

Generated 10 Nov 2026, 06:22 on the Docker stack by `scripts/benchmark.py`: 15 blind runs (5 per fault), interleaved F1 → F2 → F3. Chaos injects the fault and hides the truth; J.A.R.V.I.S. diagnoses from telemetry only; its signed answer is committed before the reveal. Each baseline is scored on the same incident J.A.R.V.I.S. saw.

## Top-1 accuracy

| Method | F1 DB exhaustion | F2 payment slowdown | F3 auth down | Overall |
|---|---|---|---|---|
| **J.A.R.V.I.S. (evidence score)** | 5/5 | 5/5 | 5/5 | **15/15 (100%)** |
| Random broken service (expected) | 1.7/5 | 2.5/5 | 2.5/5 | 6.7/15 (44%) |
| Most error events | 0/5 | 3/5 | 0/5 | 3/15 (20%) |
| First to break | 2/5 | 0/5 | 0/5 | 2/15 (13%) |
| Most depended-on (always the DB) | 5/5 | 0/5 | 0/5 | 5/15 (33%) |

## Time to diagnose (fault injected → root cause named)

- F1 DB exhaustion: median **4.4 s** (range 1.7–5.1 s)
- F2 payment slowdown: median **6.8 s** (range 6.7–7.2 s)
- F3 auth down: median **4.0 s** (range 3.7–4.7 s)
- All runs: median **4.7 s**

## Every run

| # | Fault | Hidden truth | J.A.R.V.I.S. | Confidence | Time | Random | Most errors | First to break | Most depended-on |
|---|---|---|---|---|---|---|---|---|---|
| 1 | F1 DB exhaustion | user-db | user-db ✓ | High | 4.3 s | 1 in 3 | gateway ✗ | auth-service ✗ | user-db ✓ |
| 2 | F2 payment slowdown | payment-service | payment-service ✓ | High | 7.2 s | 1 in 2 | payment-service ✓ | gateway ✗ | user-db ✗ |
| 3 | F3 auth down | auth-service | auth-service ✓ | High | 3.8 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 4 | F1 DB exhaustion | user-db | user-db ✓ | High | 5.1 s | 1 in 3 | auth-service ✗ | user-db ✓ | user-db ✓ |
| 5 | F2 payment slowdown | payment-service | payment-service ✓ | High | 6.8 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 6 | F3 auth down | auth-service | auth-service ✓ | High | 3.7 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 7 | F1 DB exhaustion | user-db | user-db ✓ | High | 1.7 s | 1 in 3 | auth-service ✗ | user-db ✓ | user-db ✓ |
| 8 | F2 payment slowdown | payment-service | payment-service ✓ | High | 6.7 s | 1 in 2 | payment-service ✓ | gateway ✗ | user-db ✗ |
| 9 | F3 auth down | auth-service | auth-service ✓ | High | 4.7 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 10 | F1 DB exhaustion | user-db | user-db ✓ | High | 4.8 s | 1 in 3 | gateway ✗ | auth-service ✗ | user-db ✓ |
| 11 | F2 payment slowdown | payment-service | payment-service ✓ | High | 6.7 s | 1 in 2 | payment-service ✓ | gateway ✗ | user-db ✗ |
| 12 | F3 auth down | auth-service | auth-service ✓ | High | 4.0 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 13 | F1 DB exhaustion | user-db | user-db ✓ | High | 4.4 s | 1 in 3 | gateway ✗ | auth-service ✗ | user-db ✓ |
| 14 | F2 payment slowdown | payment-service | payment-service ✓ | High | 6.8 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |
| 15 | F3 auth down | auth-service | auth-service ✓ | High | 4.1 s | 1 in 2 | gateway ✗ | gateway ✗ | user-db ✗ |

*Honest notes:* a small sample (15 runs) on one machine with synthetic traffic. The random baseline is scored by its expected value (picking 1 of the k broken services), not a single seeded draw, which would overstate luck. Thresholds and weights were set before these runs and were not tuned on them.
