# 🟩 Decoding the Green Squares — Key Findings

## GitHub Bot & Suspicious Profile Detection Analysis

---

## Executive Summary

| Metric | Value |
|---|---|
| Total Profiles Analyzed | 101,807 |
| Genuine Profiles | 67,074 (65.88%) |
| Suspicious Profiles | 34,733 (34.12%) |
| Profiles with All 4 Red Flags | 9,124 |
| Red Flag Catch Rate | 100% |
| Top Country by Profile Count | United States (12,583) |
| Top Language by Avg Stars | TypeScript (344.2 avg stars) |

---

## Finding 1 — 1 in 3 GitHub Profiles Shows Bot-Like Behavior

**34.12%** of all profiles in the dataset were flagged as suspicious based on a custom multi-factor suspicion scoring system.

> Out of 101,807 profiles, **34,733 were flagged suspicious** and **67,074 were genuine**.

**What this means:** GitHub's green square activity graph does not always reflect real human contribution — a significant portion of profiles show engineered or automated patterns.

---

## Finding 2 — README Score is the Sharpest Signal

| Profile Type | Avg README Score |
|---|---|
| Genuine | 6.3 / 10 |
| Suspicious | 2.3 / 10 |

A **4-point gap** in README quality between genuine and suspicious profiles makes this one of the clearest individual signals. Bots and fake accounts invest in activity metrics (stars, commits) but neglect content quality.

---

## Finding 3 — Stars Are a Trap: High Stars ≠ Real Credibility

- **2,668 profiles** (2.6% of dataset) had **1,000+ stars but fewer than 10 forks** — a classic fake-popularity fingerprint.
- Of these high-star/low-fork profiles, **99.6% were independently flagged as suspicious**.
- Among the **Top 10 most-starred profiles**, **10 out of 10** were flagged suspicious.

> **Takeaway:** Raw star count is a vanity metric and a poor proxy for genuine credibility. Stars can be gamed; forks are much harder to fake.

---

## Finding 4 — Suspicious Hours Reveal the Bot Pattern

| Profile Type | % Committing in Suspicious Hours (11 PM – 4 AM) |
|---|---|
| Genuine | ~0.004% |
| Suspicious | 66.04% |

This is a **~1,036x difference** — making off-hours commit activity one of the most powerful discrimination signals in the entire dataset.

---

## Finding 5 — Mass-Follow Behavior Flags 27.7% of All Profiles

- **28,240 profiles** (27.7% of dataset) had a `follow_ratio < 0.05` — meaning they follow far more accounts than follow them back.
- Of these, **97% were already flagged as suspicious** by the scoring system.

| Profile Type | Avg Follow Ratio |
|---|---|
| Genuine | 13.43 |
| Suspicious | 2.36 |

Suspicious accounts follow aggressively to gain visibility, but rarely earn genuine followers back — a ratio nearly **5.7x lower** than genuine accounts.

---

## Finding 6 — High Commit Frequency is Paradoxically Suspicious

Profiles were bucketed into Low / Medium / High commit frequency groups:

| Commit Frequency Group | Suspicious Rate |
|---|---|
| Low | ~17% |
| Medium | ~34% |
| High | ~96% |

**High-frequency committers are nearly 6x more likely to be classified as suspicious** than low-frequency ones. Bots automate commit activity to appear productive, resulting in unnaturally high frequency.

---

## Finding 7 — Suspicion Tier Breakdown (Confirmed Bot Tier is Telling)

| Suspicion Tier | Score Range | Avg Stars | Avg README Score |
|---|---|---|---|
| Clean | 0 – 0.25 | 135.51 | 6.59 / 10 |
| Borderline | 0.25 – 0.50 | — | — |
| Likely Bot | 0.50 – 0.75 | — | — |
| Confirmed Bot | 0.75 – 1.00 | **781.80** | **1.81 / 10** |

**Confirmed Bot profiles average 781 stars with a README score of just 1.81** — almost a complete inversion of quality indicators vs. Clean profiles. High stars, low substance.

---

## Finding 8 — Account Age Does NOT Predict Suspicious Behavior

| Account Age | Suspicious Rate |
|---|---|
| < 1 year | ~33.9% |
| 1–2 years | ~34.2% |
| 2–4 years | ~34.5% |
| 4–6 years | ~34.7% |
| 6–8 years | ~34.1% |
| 8+ years | ~34.3% |

The suspicious rate is nearly **flat across all age groups (33.9% – 34.7%)**. Bot accounts are not just new — they exist at every profile age bracket, meaning account age is not a reliable trust signal.

---

## Finding 9 — Language Communities Vary in Suspicious Concentration

- Among languages with **500+ profiles**, **Rust** had the highest suspicious rate at **34.8%**.
- **Swift** profiles had the highest average suspicion score (**0.379**) and one of the lowest README scores (**4.90/10**).
- **TypeScript** developers had the highest average star count (**344.2**) — though star inflation is a known suspicious pattern.

---

## Finding 10 — The Scoring System Achieves 100% Catch Rate on Extreme Cases

**9,124 profiles** simultaneously triggered all four major red flags:
- Stars > 500
- Follow ratio < 0.05
- README score < 3
- Commits during suspicious hours (11 PM – 4 AM)

**100% of these profiles were already flagged by the suspicion scoring system**, validating that the multi-signal weighted scoring logic correctly surfaces the most extreme cases without any manual review.

---

## Suspicion Score — How It Was Built

The custom `suspicion_score` was engineered from 7 behavioral signals:

| Signal | Weight |
|---|---|
| star_fork_ratio > 50 | +0.25 |
| follow_ratio < 0.05 | +0.20 |
| readme_score < 3 | +0.20 |
| Commits in suspicious hours (11 PM – 4 AM) | +0.15 |
| Round-number commit streaks (100/200/300/365/500) | +0.10 |
| No bio | +0.05 |
| No profile picture | +0.05 |

Profiles scoring **≥ 0.50** were classified as **suspicious (`is_suspicious = 1`)**.

---

## Priority Signals — What Actually Predicts a Fake Profile

| Signal | Impact |
|---|---|
| 🔴 Commits during 11 PM – 4 AM | ~1,036x more common in suspicious profiles |
| 🔴 Follow ratio < 0.05 | 97% of mass-followers are suspicious |
| 🔴 README score < 3 | 4-point gap vs. genuine profiles |
| 🟡 Stars > 1000 with forks < 10 | 99.6% flagged as suspicious |
| 🟡 High commit frequency | ~6x more suspicious than low-frequency |
| 🟢 Account age | No meaningful impact — unreliable signal |

---

*Dataset: `github_illusion_dirty.csv` → Cleaned → `github_illusion_clean.csv` → Analysed → `github_illusion_analysis.csv`*  
*Tools: Python · Pandas · Seaborn · Matplotlib*
