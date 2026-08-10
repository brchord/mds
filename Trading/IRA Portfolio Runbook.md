# IRA Portfolio — Operational Runbook

**Status:** Working specification / design baseline  
**Purpose:** Preserve the IRA architecture and operating rules independently from the taxable $7M portfolio.

> **Important:** This document records the architecture developed in our analysis. Verify current broker rules, option permissions, tax treatment, and contract specifications before implementation.

---

## 1. Portfolio objective

The IRA is designed separately from the taxable $7M portfolio.

Its objectives are:

1. Long-term growth over an approximately **24-year horizon**.
2. Take advantage of the IRA's lack of current annual taxation on trading activity.
3. Use defined-risk leverage rather than margin borrowing.
4. Maintain a large liquid SGOV reserve.
5. Target **99% CVaR no worse than approximately -35%**.
6. Minimize operational complexity.
7. Use long-dated options as an equity substitute rather than owning an unbounded leveraged position.

The IRA **does not have margin or portfolio margin**.

Therefore:

> **Short puts are not part of the IRA architecture.**

---

## 2. Strategic architecture

### Baseline structure

- **SPX LEAPs**.
- Remaining IRA capital in **SGOV**.

The long option provides leveraged equity exposure while the SGOV reserve provides liquidity and limits the portfolio's downside relative to outright leveraged stock ownership.

---

## 3. Instrument selection

### SPX vs XSP vs SPY

The research compared:

**SPX**

- Excellent liquidity.
- Institutional market.
- Tight normalized spreads.
- Large contract size.

**XSP**

- Excellent sizing granularity.
- Poor long-dated liquidity.

**SPY**

- Excellent sizing granularity.
- Very liquid options.
- However, observed long-dated SPY options had materially wider normalized spreads and substantially higher IV than comparable SPX options.

Final preference:

$$
\boxed{\text{SPX}}
$$

---

## 4. LEAP maturity

### Preferred maturity

Approximately **858 DTE**.

Longer maturities were considered.

### 1,228 DTE

Rejected as the default because:

- displayed spread approximately doubled (~40 → ~80 SPX points);
- additional duration is not fully utilized because the position is intentionally rolled when it approaches ~365 DTE;
- greater rho sensitivity adds another source of duration/rate exposure;
- the incremental complexity and execution cost did not appear justified.

### 1,592 / 1,956 DTE

Rejected because displayed spreads around ~300 points were considered economically excessive.

### Rule

> Choose the longest economically efficient maturity, not necessarily the longest available maturity.

Current baseline: **~858 DTE**.

---

## 5. Delta constraint

The design intentionally excludes options below approximately:

$$
\boxed{\Delta=0.75}
$$

Reason:

Lower-delta calls place an increasingly large proportion of capital into extrinsic value.

At the observed 857-DTE chain:

| Delta | Mid premium | Intrinsic | Extrinsic | Extrinsic share |
|---:|---:|---:|---:|---:|
| 0.75 | $1,688.75 | $657.64 | $1,031.11 | ~61% |
| 0.80 | $1,965.00 | $1,057.64 | $907.36 | ~46% |
| 0.85 | $2,328.40 | $1,557.64 | $770.76 | ~33% |
| 0.90 | $2,708.70 | $2,057.64 | $651.06 | ~24% |

Therefore the practical candidate set is:

- 0.75Δ
- 0.80Δ
- 0.85Δ
- 0.90Δ

Do not let an optimizer select a lower delta merely because it produces a better superficial CVaR.

---

## 6. Current preferred candidate

Preliminary conclusion:

$$
\boxed{\textbf{SPX ~858-DTE LEAPs at ~0.80Δ}}
$$

with the remaining IRA balance in SGOV.

At the observed chain:

- SPX spot: 7,757.64
- Strike: 6,700
- Bid: $1,944.20
- Ask: $1,985.80
- Mid: $1,965.00
- IV: 22.31%
- OI: 533

---

## 7. Entry mechanics

### At inception

1. Determine current IRA NAV.
2. Determine the approved LEAP delta.
3. Purchase SPX LEAPs.
4. Prefer limit execution.
5. Do not cross an abnormally wide market merely to establish the position.
6. Leave the remaining cash in SGOV.

### Important

The LEAP is **not** intended to become shares.

It is a tradable exposure instrument.

---

## 8. Expiration / exercise rule

The IRA cannot comfortably absorb a large outright SPX-equivalent equity position.

Therefore:

> **Never rely on exercise/expiration as part of the strategy.**

SPX is cash-settled and European-style, which makes this operationally cleaner than SPY.

The position should be closed/rolled well before expiration.

---

## 9. Roll policy

### Target roll point

When the existing LEAP reaches approximately:

$$
\boxed{365\text{ DTE}}
$$

the position is rolled.

Correct sequence:

1. Sell the existing LEAP at its then-current market price.
2. Purchase a **new ~858-DTE LEAP**, not a 365-DTE option.
3. Re-establish the desired delta exposure.
4. Keep the residual capital in SGOV.

Conceptually:

$$
858\rightarrow365\rightarrow858\rightarrow365\rightarrow858
$$

The 365-DTE market is the **exit valuation**, not the replacement maturity.

---

## 10. Roll exceptions

The 365-DTE point is a target, not a blind command.

Consider delaying or modifying the roll if:

- volatility is exceptionally high;
- option liquidity is temporarily impaired;
- the replacement LEAP is unusually expensive;
- a major market dislocation makes immediate re-entry unattractive.

However, any deviation should be explicit and documented.

### Hard operational cutoff

Do not allow a LEAP to drift toward expiration accidentally.

Establish an operational review well before expiration (e.g. 60–90 DTE) so the position cannot inadvertently become an exercise/expiration problem.

---

## 11. Tail-risk constraint

Primary risk metric:

$$
\boxed{\text{99\% CVaR}}
$$

Target:

$$
\boxed{\text{99\% CVaR}\ge -35\%}
$$

This is deliberately less conservative than the taxable margin portfolio's -25% target because the IRA has a longer horizon and a different purpose.

It is nevertheless a hard design constraint.

---

## 12. Preliminary CVaR results

Using the observed 857-DTE and ~374-DTE market data as calibration inputs, the preliminary lifecycle simulation produced approximately:

| LEAP Δ | Approx. initial delta exposure | 99% CVaR |
|---:|---:|---:|
| 0.75 | ~0.97× | ~-24% |
| **0.80** | **~1.03×** | **~-29%** |
| 0.85 | ~1.10× | ~-34% |
| 0.90 | ~1.16× | ~-39% |

Interpretation:

- 0.75: comfortably inside budget, but high extrinsic burden.
- **0.80: best preliminary balance.**
- 0.85: technically inside the budget but with very little margin.
- 0.90: fails the -35% requirement.

These figures are model outputs, not guarantees.

---

## 13. Expected-return framework

For each delta, evaluate:

$$
\text{Return}
=
\text{LEAP P\&L}
+
\text{SGOV return}
-
\text{entry/exit/roll friction}
$$

Because the account is an IRA, current-year taxable-event drag is not a design constraint.

Do not optimize for nominal return alone.

Use:

$$
\boxed{\text{Expected return subject to 99\% CVaR}\ge-35\%}
$$

as the optimization objective.

---

## 14. Preliminary reward/risk conclusion

Observed preliminary annualized expected returns were approximately:

| Delta | Approx. annualized expected return |
|---:|---:|
| 0.75 | ~6.2% |
| **0.80** | **~6.35%** |
| 0.85 | ~6.55% |
| 0.90 | ~6.8% |

The additional expected return from increasing delta is modest relative to the deterioration in tail risk.

Therefore:

$$
\boxed{\textbf{0.80Δ is currently the preferred risk/reward point.}}
$$

Do not increase to 0.85 merely because it has higher expected return; it consumes almost the entire CVaR budget for relatively little incremental return.

---

## 15. Volatility / crash sensitivity

The long-call structure has an important property:

During a severe equity decline:

- delta falls;
- the option loses intrinsic value;
- but an increase in implied volatility can partially offset the loss through vega.

Therefore the final stress test should explicitly model:

1. SPX price shock.
2. Volatility expansion.
3. Correlation between price and volatility.
4. Remaining time value.
5. Option liquidity / execution cost.

Do not assume constant IV in the final production risk model.

---

## 16. Monitoring

### Weekly

- SPX price.
- SPX volatility.
- LEAP delta.
- LEAP market liquidity.
- SGOV balance.
- Distance to 365 DTE.

### Monthly

- Recalculate portfolio CVaR.
- Recalculate delta-equivalent exposure.
- Review implied volatility.
- Review LEAP liquidity.
- Review SGOV yield.
- Track actual option drag.

### At ~365 DTE

Execute or explicitly defer the roll.

### At ~90 DTE

Hard operational review to ensure the position cannot accidentally approach expiration.

---

## 18. Core philosophy

The IRA is deliberately simpler than the taxable portfolio.

The architecture is:

> **Long-dated defined-risk equity exposure + large SGOV reserve + no short-volatility underwriting + explicit CVaR ceiling.**

The purpose of the LEAP is to create efficient long-term equity exposure while preserving a large liquid reserve.

The purpose of SGOV is not to generate exciting returns. It is the stabilizing and liquidity component that makes the defined-risk leverage possible.

The strategy should therefore optimize for:

$$
\boxed{\text{Long-term compound growth subject to survivable tail risk}}
$$

rather than maximum beta.
