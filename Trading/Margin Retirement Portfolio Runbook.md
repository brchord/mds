# Retirement Portfolio — Operational Runbook

**Status:** Finalized working specification  
**Purpose:** Preserve the final portfolio architecture and give an unambiguous operating procedure for the equity and short-option overlays.

> **Important:** This is the strategy specification developed in our analysis. Verify current broker, tax, option, and regulatory mechanics before implementation.

---

## 1. Portfolio objective

Build a taxable brokerage portfolio designed to:

1. Preserve capital and maintain substantial liquidity.
2. Target approximately **-25% 99% CVaR** as the primary tail-risk design constraint.
3. Generate option premium through a disciplined overlay.
4. Minimize unnecessary taxable events.
5. Keep the portfolio highly liquid and operationally simple.
6. Explicitly change operating behavior when market conditions indicate elevated tail risk.
7. Preserve the capital efficiency of the architecture even when short options move against us.

The portfolio is **not** designed to maximize option premium or available margin.

---

## 2. Strategic allocation

| Sleeve | Target |
|---|---:|
| **SPY equity** | **40%** |
| **SGOV** | **60%** |

### Equity instrument

**SPY** is the equity substrate because its options market is exceptionally deep and liquid.

The final architecture intentionally concentrates the equity sleeve in SPY so that the option overlay can be run on the same highly liquid underlying.

---

# 3. Equity option overlay

The SPY equity sleeve is divided operationally into:

- **70% strategic buy-and-hold SPY**
- **30% covered-call sleeve**

The covered-call overlay is therefore deliberately **partial**, preserving substantial upside participation.

---

# 4. Covered-call operating rules

## Entry

### Underlying

SPY only.

### Tenor

Target:

$$
\boxed{30\text{–}45\ DTE}
$$

The standard operating point between **30-45 DTE** when liquidity and the chain permit.

### Entry delta

Target:

$$
\boxed{15\text{–}20\Delta}
$$

Use approximately **15Δ** when premium is adequate; move toward **20Δ** only when the economics justify it.

Do not initiate calls above 20Δ under NORMAL conditions.

Delta is preferred over a fixed percentage-OTM rule because it adapts to volatility and the option surface.

---

# 5. Covered-call active roll thresholds

The previous runbook was too vague here. The following thresholds are the operational rules.

### GREEN: delta ≤ 20Δ

**No active defensive roll.**

The call is behaving as intended.

Manage only according to the normal lifecycle / DTE rules.

### YELLOW: 20Δ < delta < 30Δ

**Monitor.**

Do not automatically roll merely because delta has moved above the entry delta.

The position can remain open if:

- market conditions are NORMAL;
- expiration is not imminent;
- the covered-call sleeve remains within its intended allocation.

### ORANGE: delta ≥ 30Δ

**Active roll condition.**

The call is now sufficiently close to the money that assignment risk and upside truncation are materially different from the intended 15–20Δ campaign.

Action:

1. Buy back the existing call.
2. Sell a new SPY call at approximately **15–20Δ**.
3. Prefer a replacement with approximately 30–45 DTE.
4. The roll must be evaluated as a package, including realized loss on the old call and premium from the new call.
5. **Do not roll for a debit.**

If no acceptable credit roll exists, close the call and leave the shares uncovered rather than paying a debit merely to preserve the overlay.

### RED: delta ≥ 50Δ

**Immediate intervention.**

Do not wait for expiration.

Close the call.

Then reassess whether a replacement call is appropriate. If a replacement cannot be established at the target 15–20Δ for an acceptable credit, leave the shares uncovered.

This prevents a low-delta covered-call strategy from silently becoming an aggressive systematic sale of the underlying.

---

# 6. Covered-call DTE lifecycle

Delta is the primary active-management trigger. DTE determines the normal lifecycle.

### >21 DTE

Allow the position to develop normally unless it reaches the active delta thresholds.

### 14–21 DTE

Begin evaluating the next roll.

If delta is still ≤20Δ and the option has retained little value, there is no requirement to force a roll.

### ≤14 DTE

The position enters the **scheduled-roll window**.

Normally:

1. Buy back the existing call.
2. Sell a new 30–45 DTE call.
3. Target 15–20Δ.
4. Require a net credit.

### ≤7 DTE

Do not allow a routine covered call to drift toward expiration without an explicit decision.

If it remains in the money or near the money, manage it before expiration.

---

# 7. Covered-call assignment

Assignment is acceptable only when it does not unintentionally violate the strategic SPY allocation.

Because this sleeve represents only ~30% of the SPY position, assignment is not inherently catastrophic.

However:

> **Do not use assignment as the normal roll mechanism.**

If the call reaches the active-roll thresholds, manage the option before expiration.

If assignment nevertheless occurs:

1. Record the shares removed.
2. Rebalance the SPY sleeve back toward the 40% strategic allocation when appropriate.
3. Re-establish the covered-call campaign only after the underlying allocation has been restored.

---

# 8. SPX Short-put architecture

SGOV is the collateral engine for the short-put campaign.

## Instrument

**SPX options**

The design favors SPX because of its deep institutional liquidity, cash settlement, European exercise, and notional efficiency.

## Tenor

Target:

$$
\boxed{30-45\ DTE}
$$

## Entry delta

Target:

$$
\boxed{15\Delta}
$$

This is the normal underwriting point.

The campaign should not routinely initiate puts above approximately 20Δ.

---

# 9. Short-put sizing

The put campaign is sized against the SGOV collateral architecture.

The approved notional is determined by the portfolio's **99% CVaR budget**, not by maximum broker margin.

The key constraint is:

$$
\boxed{\text{Portfolio 99\% CVaR}\approx-25\%}
$$

which roughtly translates to:

$$
\boxed{\text{Short-put notional}\leq100\%\times\text{designated put capital}}
$$

Do not use broker margin capacity as the target.

The short-put notional should therefore be recalculated if:

- SPY allocation changes;
- SGOV allocation changes;
- put delta changes;
- market volatility changes materially;
- the regime changes.

---

# 10. Short-put active roll thresholds

### GREEN: delta ≤ 20Δ

**No active defensive roll.**

The put is behaving close to its intended entry profile.

Continue normal management.

### YELLOW: 20Δ < delta < 30Δ

**Monitor.**

Do not automatically roll solely because the delta has moved above 20Δ.

Check:

- SPX drawdown;
- VIX;
- VIX futures structure;
- DTE;
- liquidity;
- total portfolio CVaR.

### ORANGE: delta ≥ 30Δ

**Active roll condition.**

The original 15Δ thesis has materially changed.

Action:

1. Buy back the existing short put.
2. Sell a new SPX put at the **lowest possible delta**.
3. Prefer approximately **60 DTE**.
4. Reduce notional if necessary.
5. **Require a net credit.**

If the market does not permit a sensible credit roll, **close the short put and do not replace it**.

The strategy is explicitly designed to reduce volatility exposure.

### RED: delta ≥ 50Δ

**Immediate risk-reduction condition.**

Close the short put.

Do not wait for expiration.

Do not roll merely to avoid realizing the loss.

Do not increase notional to "average down."

Only re-enter short-put exposure after the portfolio returns to NORMAL conditions and the new trade satisfies the 15Δ / CVaR rules.

---

# 11. Short-put DTE lifecycle

### >21 DTE

Normal monitoring.

The delta thresholds remain the primary active-management triggers.

### 14–21 DTE

Begin evaluating the next 41-DTE cycle.

If the existing put is still ≤20Δ and conditions are NORMAL, there is no requirement to roll immediately.

### ≤14 DTE

Enter the scheduled-roll window.

Normal action:

1. Buy back the existing put.
2. Sell a new approximately 41-DTE SPX put.
3. Target approximately 15Δ.
4. Require a net credit.
5. Confirm total notional remains inside the approved CVaR budget.

### ≤7 DTE

Do not allow the position to drift toward expiration without an explicit decision.

A short put approaching expiration while materially above the target delta should normally be closed rather than carried merely because expiration is close.

---

# 12. Profit-taking versus rolling

The campaign is **not** governed by a mechanical "take profit at 50%" rule.

The primary management variables are:

1. Delta.
2. DTE.
3. Market regime.
4. Portfolio CVaR.
5. Execution economics.

If an option has decayed substantially and can be closed cheaply, closing it early is acceptable and may be preferable to carrying unnecessary tail exposure.

Do not reopen a new short option simply because the old one was profitable.

A new trade must independently satisfy the current portfolio rules.

---

# 13. Credit-only rule

This applies to **all short-option rolls**.

$$
\boxed{\textbf{Never roll a short option for a debit.}}
$$

For a covered call:

> Buy old call + sell new call = net credit required.

For a short put:

> Buy old put + sell new put = net credit required.

If the desired defensive roll cannot be executed for a credit:

**close the old option and remain unhedged / uncovered as appropriate.**

Do not pay a debit merely to maintain the appearance of continuity.

---

# 13. Market-regime state machine

## NORMAL

Permitted:

- Open 15–20Δ SPY covered calls on the designated 30% sleeve.
- Open ~15Δ 41-DTE SPX puts.
- Roll normally.
- Maintain approved notional.
- Use SGOV as collateral.

## ELEVATED RISK

Signals include:

- VIX rate-of-change crossing the validated defensive threshold.
- VIX futures entering backwardation.
- Confirmation from SPX drawdown and/or option liquidity deterioration.

Actions:

1. **Stop opening new SPX puts.**
2. Do not initiate new covered calls merely to harvest higher premiums.
3. Review every live short put.
4. Any put at ≥30Δ receives active management.
5. Any put at ≥50Δ is closed.
6. Covered calls at ≥30Δ are actively rolled if a credit roll is available.
7. Preserve SGOV liquidity.

## BLACK SWAN / CRISIS

When the evidence indicates a genuine systemic event:

1. Stop all new short-put underwriting.
2. Close live short puts aggressively.
3. Do not wait for expiration.
4. Do not roll into substantially higher-risk short-volatility exposure merely for a credit.
5. Do not increase put notional after a large selloff.
6. Preserve SGOV.
7. Do not immediately deploy SGOV into equities simply because prices have fallen.
8. Re-enter short puts only after volatility and liquidity normalize and the portfolio returns to NORMAL.

The discretionary human override is intentional.

---

# 14. VIX / VIX-futures monitoring

## Primary signal

**VIX futures backwardation** is the principal regime-warning signal.

## Secondary signal

**Rapid VIX expansion** is a supporting signal.

The two signals should be interpreted together rather than treating a single one-day VIX spike as an automatic crisis.

## Confirmation

Also monitor:

- SPX drawdown;
- option bid/ask widening;
- unusual put activity;
- open-interest structure;
- market liquidity.

---

# 15. Tail-risk control

Primary risk metric:

$$
\boxed{\text{99\% CVaR}}
$$

Target:

$$
\boxed{\text{approximately }-25\%}
$$

The CVaR budget determines the permissible SPX short-put notional.

Do not increase notional simply because historical premium has been attractive.

The risk model should be rerun whenever the strategy parameters or market regime change materially.

---

# 16. Liquidity hierarchy

The portfolio prioritizes:

1. **Option collateral**
2. **Spending reserve**
3. **Portfolio stabilizer**

The 60% SGOV allocation is part of the risk architecture.

It is not "cash sitting around waiting to be invested."

Do not consume the reserve merely to avoid realizing a short-option loss.

---

# 18. Monitoring schedule

## Daily / trading-day check

- VIX.
- VIX futures term structure.
- SPX movement.
- Live short-put deltas.
- Live covered-call deltas.
- Option liquidity.
- SGOV collateral utilization.

## Weekly

For every live short option record:

- underlying;
- strike;
- expiration;
- entry delta;
- current delta;
- premium received;
- current mark;
- DTE;
- roll eligibility;
- current regime.

Verify:

- short-put notional;
- covered-call sleeve size;
- SGOV collateral;
- CVaR budget.

## Monthly

- Recalculate portfolio CVaR.
- Review realized option premium.
- Review execution quality and slippage.
- Review SGOV reserve.
- Review the current regime classification (NORMAL / ELEVATED / CRISIS).

## Quarterly

- Re-run the full Monte Carlo / CVaR analysis.
- Revalidate the 15Δ put target.
- Revalidate the 15–20Δ call target.
- Revalidate the 30Δ active-roll threshold.
- Revalidate the 50Δ emergency threshold.
- Revalidate the short-put notional limit.
- Review regime thresholds.

---

# 19. Quick-reference decision table

| Condition | Covered Call | Short SPX Put |
|---|---|---|
| **Entry** | 15–20Δ, 30–45 DTE | ~15Δ, 30-45 DTE |
| **≤20Δ** | Normal | Normal |
| **20–30Δ** | Monitor | Monitor |
| **≥30Δ** | **Active roll to 15–20Δ** | **Active roll to ~15Δ** |
| **≥50Δ** | **Close immediately** | **Close immediately** |
| **≤14 DTE** | Scheduled roll window | Scheduled roll window |
| **≤7 DTE** | Explicit management required | Explicit management required |
| **Normal roll** | 30–45 DTE replacement | 30-45 DTE replacement |
| **Roll economics** | Credit only | Credit only |
| **Crisis** | Reduce/stop new calls as appropriate | **Close / stop new puts** |

---

# 20. Core philosophy

The final architecture is:

$$
\boxed{\textbf{40\% SPY + 60\% SGOV}}
$$

with:

- a **30% covered-call overlay on the SPY sleeve**;
- a **~15Δ 41-DTE SPX put-underwriting engine** collateralized by SGOV;
- explicit **30Δ active-roll thresholds**;
- explicit **50Δ emergency-close thresholds**;
- a strict **credit-only rolling rule**;
- a **~25% 99% CVaR target**;
- and a regime system that allows the portfolio to stop selling volatility when the evidence indicates a genuine tail event.

The central principle is:

> **The portfolio earns option premium only while the expected compensation remains adequate for the tail risk being assumed. Once an option migrates materially away from its intended delta, or the market enters a crisis regime, risk reduction takes precedence over premium collection.**
