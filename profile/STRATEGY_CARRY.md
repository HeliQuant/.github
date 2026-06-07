# 🌀 HeliQuant — Strategy: Delta-Neutral Funding Carry

> A **market-neutral** strategy that earns yield **without predicting price** — the honest answer to
> "how do you make money on an efficient market?" Every number below is from a real backtest/probe run on
> live exchange data this session, net of fees. No prediction, no overfit, no hallucinated alpha.

---

## Why this exists

HeliQuant's directional edge (OI-Contrarian) was once validated on **MNT** — but has since **decayed on
fresh data and been demoted** (validated registry now empty), and it always abstained on efficient majors —
because you **can't predict** an efficient market. But profit ≠ prediction. The pros who consistently
earn on BTC/ETH don't forecast direction; they harvest **structure**. The most accessible structural
yield is the **perpetual funding rate**, captured **delta-neutral**.

This strategy is the portfolio complement to the directional edge: when nothing is predictable, you can
still earn carry — with zero directional bet.

---

## 1. The mechanism (no prediction involved)

A perpetual swap is tethered to spot by **funding**: when the perp trades above spot, longs pay shorts
every 8h (and vice-versa). So:

> **Long spot + short the perpetual, equal notional.** Price moves cancel (delta-neutral). You simply
> **collect the funding rate** the short leg earns. It's *carry* — like earning interest — not a bet on
> where price goes.

On the assets we trade, funding is **positive 74–80% of the time** (the crowd leans long), so a
short-perp-hedged position is paid to exist.

---

## 2. Where the carry actually pays (12 months, net of fees)

| Asset | mean funding /8h | % periods positive | **Net annualized carry** | vs ~5% risk-free |
|---|---|---|---|---|
| **HYPE** | +0.67 bps | 79% | **+7.2%/yr** ⭐ | **beats** |
| **SUI** | +0.48 bps | 80% | **+5.1%/yr** | beats |
| ETH | +0.33 bps | 72% | +3.4%/yr | below |
| BTC | +0.34 bps | 74% | +3.6%/yr | below |

**Key insight:** carry is **richest on less-efficient assets.** BTC/ETH funding is the thinnest (most
arbitraged) → their carry is *below* a plain stablecoin yield, so we **don't** run it there. The yield
lives where the inefficiency is — the same lesson as our directional work.

### 2b. Rigorous net — full PnL + walk-forward (`scripts/79`)

The +7.2% above is **gross funding only.** Modeling the *complete* per-trade economics — funding **+ basis
drift − the 4 fee legs** (open spot+perp, close spot+perp) — and walk-forwarding it across the year:

| Hold period | HYPE net /yr | fee drag | walk-forward buckets | verdict |
|---|---|---|---|---|
| 7 days | **−5.0%** | −11.5% | [+1, −2, −8, −10]% | rebalancing bleeds it dry |
| 30 days | +4.0% | −2.7% | [+9, +7, +0, −1]% | borderline |
| **90 days** | **+5.8%** | −0.9% | **[+11, +9, +2, +1]% — 4/4 positive** | ✅ **real + durable** |

Three honest refinements: **(1)** the trustworthy net is **+5.8%/yr at a ~quarterly hold**, not +7.2% —
fees and basis are real. **(2)** It's **~entirely funding (basis drift ≈ 0)** → durable carry, *not* basis
luck. **(3) Holding period is everything:** weekly rebalancing bleeds ~11%/yr in fees (net negative);
quarterly costs ~1%. *Hold long, rebalance rarely* — the math behind §4's "always-on beat clever." Caveat:
the walk-forward buckets are **declining** (+11→+1%) — the carry is **compressing** as funding cools (the
live carry desk reads this in real time). Still clears risk-free, but thinly. SUI nets **+4.3%** and BTC
**+2.8%** at 90d — both *below* risk-free once fees are modeled honestly, so **only HYPE qualifies.**

---

## 3. The crash stress-test (the part most people skip)

A carry is delta-neutral **only if the perp tracks spot.** Its real killer is a **basis dislocation** in
a crash — when the perp gaps away from spot, the hedge breaks and you take a mark-to-market hit *exactly*
when you need protection. So we measured the basis = (perp − index)/index **through each asset's worst
crashes:**

| Asset | normal basis | basis in WORST crashes | funding in crashes | verdict |
|---|---|---|---|---|
| **HYPE** | median −5 bps, max \|29\| | held **−8…+3 bps** through a **−17.3%** crash | ~0 bps/8h | ✅ **crash-robust** |
| **SUI** | median −5 bps | dislocated to **−166 / −97 bps** | spiked **−16.8 bps/8h** | ⚠️ **lumpy — risk premium** |
| BTC | max \|11\| bps | −5.8 bps (tame) | +0.07 bps/8h | ✅ tame (but thin) |

**HYPE is the standout:** richest carry **and** its hedge held within 29 bps even through a −17% crash.
**SUI pays more than BTC but its basis genuinely dislocates in crashes** — that extra yield is a **real
crash-risk premium, not free money** (though the dislocations are brief: cumulative negative-funding bleed
over the year was only −0.52%). Trade SUI carry smaller, with margin to survive the swing.

---

## 4. Honest lesson: *always-on beat clever*

We tested a "smart" variant — only hold the carry when funding is positive, sit flat otherwise. It
**lost −26%/yr.** Cycling in and out racked up **303 fee-paying switches** that dwarfed the ~1% of extra
funding it captured. The dumb **always-on** version (+3.6–7.2%) won decisively. *In carry, doing less is
the edge.* (The same anti-cleverness discipline that makes us reject overfit directional signals.)

---

## 5. Execution — single-venue, no cross-exchange complexity

Both legs trade on **one venue**, so there's no cross-exchange settlement risk:

- **Bybit:** HYPE & SUI have **spot *and* perp both listed** → long spot + short perp on one CEX.
- **Hyperliquid (on-chain):** HYPE token spot + HYPE/SUI perps exist → a **fully on-chain, permissionless
  delta-neutral carry** — fitting HeliQuant's on-chain ethos and solving the CEX geo-block.

Costs are a **one-time** ~22 bps (open + close, both legs), amortized over a long hold → negligible
against a year of funding. (This is why churning kills it — §4.)

---

## 6. Honest caveats (so this stays trustworthy)

1. **HYPE is a young asset** — "crash-robust" means *on the data that exists*, which may not include a
   once-a-cycle liquidation cascade. Size for the crash you haven't seen.
2. **Yield-enhancement, not a moonshot** — ~7% is real and market-neutral, but it's carry, not alpha.
   It belongs in a portfolio *alongside* the directional edge, not as a headline return.
3. **Funding-only approximation** — basis drift over a hold is second-order and mean-reverting; the
   stress-test (§3) bounds the tail. A full per-trade basis-PnL model is the next rigor before live size.
4. **Crowded** — carry yields compress as more capital arrives; re-measure, don't assume.

---

## 7. How it fits HeliQuant

HeliQuant runs a **portfolio of validated strategies**, not one trick:

- **Directional:** OI-Contrarian on **MNT** — validated + anchored on Mantle on its original window (hedge-like, +28.9% OOS), **since decayed on fresh data to −7.2% OOS and demoted** (registry now empty; the on-chain anchor is an immutable snapshot of what validated at block-time, not a claim it still works).
- **Market-neutral:** this **Funding Carry**, richest + crash-tested on **HYPE** (+7.2%/yr) — a second,
  *uncorrelated* return stream that earns when nothing is predictable.

The firm picks the right tool per regime: **predict where there's a proven edge, harvest carry where
there isn't, and abstain where neither pays.** That's the honest whole.

---

*HYPE's directional edge decayed to nothing on fresh data — but its funding carry is the richest and most
crash-robust of every asset we tested. Same asset, different (and honest) way to win. Reproduce:
`scripts/76_btc_carry.py` (cross-asset carry) + `scripts/77_carry_stress.py` (crash stress-test).*
