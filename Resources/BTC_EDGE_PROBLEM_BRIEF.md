# 🟠 HeliQuant — "Why can't we trade BTC?" — Research Brief

> A self-contained problem statement for external research agents. Goal: find an **honest, cost-aware,
> out-of-sample-validated** edge on BTC, or confirm there isn't one. **No hallucinated alpha** — any
> proposed edge must survive realistic fees + walk-forward, not just a single backtest.

---

## 1. CONTEXT — what HeliQuant is + what we're trying

HeliQuant is an autonomous multi-agent trading firm (7 desks → PM) on Mantle. Its ONE validated edge is
**OI-Contrarian**: fade perp open-interest 24h-change extremes (top quintile of OI-change → SHORT,
bottom quintile → LONG, hold 24h). On **MNT** this clears a strict gate: **58.8% win, 1.30 payoff,
+28.9% OOS, ~+79 bps/trade** net of fees. We want the SAME edge to work on **BTC**.

**The gate every edge must pass** (this is the whole point — anti-overfit, cost-aware):
1. Cost-aware OOS: net return = `pos·move − 2·taker_fee`; ROI > 0 AND > buy&hold AND avg_bps > round-trip fee.
2. Walk-forward: 5 anchored folds, majority positive AND **still positive after dropping the single best fold** (no one-fold mirages).
3. Sample ≥ 20 non-overlapping trades.
Fee assumption: Bybit perp **taker ≈ 0.055%/side → ~11 bps round-trip**.

---

## 2. ROOT CAUSE — why BTC fails

**BTC's OI-contrarian edge is REAL but TOO SMALL to beat fees.** On real Bybit BTCUSDT perp data:

| Asset | dir | OOS ROI | trades | avg bps/trade | round-trip fee | verdict |
|-------|-----|---------|--------|---------------|----------------|---------|
| **MNT** | contrarian | +28.9% | 34 | **+79.4 bps** | 11 bps | ✅ huge margin |
| **BTC** | contrarian | +4.59% | 77 | **+8.3 bps** | 11 bps | ❌ **fee-eaten** (8.3 < 11) |

**The mechanism:** BTC is the most efficient, most-arbitraged, most-liquid crypto market. Mispricings are
tiny and corrected fast → the per-trade contrarian edge is only ~+8 bps, which is **smaller than the ~11
bps round-trip taker fee**. The signal's DIRECTION is correct (positive, contrarian, beats buy&hold which
was −17%), but **costs consume the edge**. On MNT the same signal earns ~10× more per trade (+79 bps)
because MNT is far less efficient.

---

## 3. WHAT WE'VE TRIED (the "resolve" attempts) — and results

We ran a **bounded variant search** (propose → validate → dispose): sweep threshold, horizon, fee model;
each variant must clear the fee bar AND survive walk-forward.

| Variant (BTC oi_contrarian) | OOS ROI | avg bps | clears fee? | walk-forward | result |
|------|---------|---------|-------------|--------------|--------|
| quintile (20/80) / 24h / taker | +4.6% | +8.3 | ❌ (<11) | — | fee-eaten (baseline) |
| **decile (10/90) / 24h / taker** | **+5.9%** | **+15.0** | ✅ (>11) | folds [11.4, −6.7, 0.5, 16.2, −7.0], **ex-best −0.47%** | ❌ **grid-search artifact** (gains in 1–2 folds; drop best → negative) → HELD |
| quintile / 24h / **maker** (~4 bps) | +12.1% | +17.3 | ✅ | not robust-confirmed | only with maker fills (NOT guaranteed) |
| 12h horizon (both thresholds) | negative | negative | ❌ | — | worse (more noise, more fees) |
| 48h horizon | negative | negative | ❌ | — | worse (fewer, lower-quality) |
| sources: funding_fade +2.33% (+5.8bps), price_meanrev/flow → negative | — | — | ❌ | — | none clear |

**Key result:** stricter threshold (decile) made the per-trade edge bigger (+15 bps, clears fee on a
single split) — BUT it **fails walk-forward** (the gain concentrates in 1–2 folds; ex-best-fold mean is
**−0.47%**). So it's a multiple-testing / grid-search artifact, not a robust edge. **We did NOT register
it.** Maker fees would clear it, but maker fills aren't guaranteed at the faded extreme (needs live
fill-rate proof).

**Deep-research round 2 (scripts/70) — NEW hypotheses, all under the same gate:**

| Hypothesis | OOS ROI | avg bps | clears fee? | walk-forward robust? |
|------------|---------|---------|-------------|----------------------|
| funding_change | −26.9% | −39 | ❌ | — |
| oi_acceleration | −3.1% | −0.8 | ❌ | — |
| confluence oi+funding | (too sparse, <20 trades) | — | — | — |
| confluence oi+flow | +0.5% | +3.1 | ❌ | — |
| **oi @ HIGH-vol regime** | **+7.4%** | **+15.0** | ✅ | ❌ (1-split only — artifact) |
| oi @ LOW-vol regime | (too sparse, <20 trades) | — | — | — |

**Verdict after 2 deep-research rounds: NO robust fee-clearing edge in oi/funding/flow/vol transforms of
the data we have.** The only variants that clear the fee on a single split (decile threshold, high-vol
regime) consistently **fail walk-forward** — they are grid-search artifacts. BTC's residual inefficiency
is not captured by these signals. **To trade BTC honestly we'd need RICHER data BTC is less efficient on:
order-flow / CVD, liquidation feeds, cross-exchange basis, on-chain exchange-netflows — which we do not
currently collect.** Until then: ABSTAIN on BTC (no forced trade, no faked testnet number).

---

## 4. CURRENT STATE

**BTC = NO validated edge → HeliQuant ABSTAINS on it.** Not anchored on-chain (we only record validated
edges). This is consistent across multiple data windows and matches prior thorough research. Honest
conclusion so far: *BTC's OI-contrarian edge is structurally too thin to trade net of taker fees, and the
fee-clearing variants are overfit artifacts.*

---

## 5. COMPETITOR / PEER SURVEY — does anyone solve BTC honestly?

Surveyed 7 trading-AI repos. **NONE has a validated, cost-aware BTC edge.** Two camps:

- **LLM multi-agent narrators (TradingAgents, TradingAgents-CN, NeuroBro):** produce confident BTC
  BUY/SELL narratives with **ZERO fee modeling and NO out-of-sample validation**. TradingAgents *supports*
  BTC-USD (fundamentals analyst auto-disabled for crypto) but openly disclaims it's "a research scaffold,
  not a strategy with replicable return." NeuroBro is a research API (one throwaway LLM line: "majors are
  range-bound; alpha is in narrative rotation"). **They'd "trade BTC" but never prove it survives costs.**
- **Serious framework (freqtrade):** the gold-standard OSS bot. Models fees rigorously (`max(maker,taker)`
  applied on entry+exit), has anti-overfit tooling (lookahead-analysis, hyperopt with capped precision),
  and **DEPRECATED + REMOVED its "edge" module** (2023.9→2025.6). Its hard-won wisdom = exactly our finding:
  *"markets with high fees are unprofitable for mechanical systems without strong alpha."* It supports
  limit/maker orders (~0.025–0.05%/side savings) but **does not model maker fill-risk** in backtests, and
  gives no BTC-specific edge — just better tooling.
- **Not trading systems:** atlas-gic (equities only, −5.91% backtest, no fee model), Thiny-AI (thin ETH
  example, slippage cap only), openclaw (personal-assistant framework; `btc-analyzer` skill = EMA20 on 15m,
  no edge/fees).

**Takeaway:** HeliQuant is MORE rigorous than the peers. The LLM agents ignore the fee problem entirely;
freqtrade independently confirms it. Our "abstain on BTC" is the honest, defensible position — the others
would either hallucinate a BTC trade or quietly lose to fees.

---

## 6. OPEN RESEARCH QUESTIONS (for other agents to help with)

Honest leads worth testing under the SAME gate (cost-aware OOS + walk-forward, no overfit):
1. **Maker/limit execution empirics:** what's the realistic FILL RATE for a contrarian limit order at a
   BTC OI extreme? If maker fills hold up (~4 bps), the decile variant *might* become real. Need live
   fill-rate data, not an assumption.
2. **Different SIGNAL TYPES on BTC** (not just OI): perp **funding-basis / spot-perp basis**, **order-flow
   imbalance (CVD)**, **liquidation cascades**, **cross-exchange dislocations**, **on-chain** (exchange
   netflows, miner flows, long-term-holder supply). Does any produce a >15–20 bps/trade edge that survives
   walk-forward?
3. **Higher timeframe / regime-conditioned:** does the OI-contrarian edge survive on a 4h–1d cadence, or
   only in specific volatility/funding regimes (regime filter)?
4. **Lower-fee venue / fee tier:** does a maker-rebate venue or VIP fee tier flip the +8 bps edge positive
   robustly?
5. **Is BTC simply not the right asset for THIS edge?** (Our honest prior: yes — the edge lives in less
   efficient assets like MNT. BTC may require a fundamentally different, non-mean-reversion alpha.)

**Constraint for any answer:** it must clear cost-aware OOS + walk-forward + drop-best-fold, on real data,
net of realistic fees. A single good backtest is NOT a yes. *Promote on evidence, confirm before you trust.*

---

## 7. PARALLEL RESEARCH HANDOFF — concrete tasks for other agents

> HeliQuant's own agent is building richer-data collectors (starting with order-flow/CVD). In PARALLEL,
> please research the frontiers below. For each, find: (a) a **free/accessible data source/API**, (b) a
> **specific signal hypothesis**, (c) ideally a **reference** that it has historically had edge on BTC.

### ⛔ Already RULED OUT (do NOT re-test — all failed or are walk-forward artifacts):
- OI-change contrarian: quintile = fee-eaten (+8.3 bps < 11); decile = 1-split clears but **artifact**
- funding level, funding change, OI acceleration, order-flow from **long-short ACCOUNT ratio**, price momentum/mean-reversion
- vol-regime-conditioned OI (high-vol clears 1-split +15 bps but **walk-forward artifact**)
- maker fees would clear some, but **maker fill-rate is unproven** (needs live data)
- **order-flow / CVD** — REAL Binance `takerBuyBaseVolume` (genuine aggressive buy/sell, NOT account ratio):
  taker-frac level, CVD-imbalance, CVD-change, CVD-24h-cumsum → **ALL negative OOS, worse than buy&hold**
  (tested 2026-06-04, `scripts/71`, 9000 bars). BTC order-flow is efficiently priced at the 1h/24h horizon.
- **cross-exchange basis** (Hyperliquid vs Binance perp): avg |basis| **1.56 bps** (heavily arbed); market-
  neutral pairs (fade basis) = **−6% OOS, −20.6 bps/trade** vs ~22 bps 2-leg fee (tested 2026-06-05, `scripts/72`).
  Perp-perp basis is arbitraged away. *(Byproduct: Hyperliquid now onboarded as a DATA venue +
  permissionless EXECUTION venue — solves the Bybit order geo-block — useful regardless of this result.)*
- **VENUE-MIGRATION thesis (cheaper fees rescue the +8.3 bps edge)** — tested directly per the btc-research.md
  recommendation (route OI-contrarian to a low-fee perp DEX: Hyperliquid 7 bps RT, ApeX/Mantle 5 bps RT, vs Bybit
  11 bps). **REFUTED at the root (tested 2026-06-05):** re-ran BTC oi_contrarian at fee ∈ {0, 5, 7, 11} bps RT — it
  fails at EVERY level **including ZERO fee** (gross net_bps **−1.8**, OOS −3.05%). A cutoff-timeline (40→100%) shows
  the GROSS directional edge **oscillates around zero** (−36.5, −0.1, **+31.9**, −12.5, −15.2, +6.6, −1.8 bps) and the
  direction even **flips** contrarian↔momentum, with **ex-best-fold negative at nearly every window**. ∴ the brief's
  earlier "+8.3 bps" was a **single-window reading (≈the +31.9 @60% slice), NOT a stable edge**. Venue migration
  cannot rescue a phantom edge — lowering fees only makes a losing strategy lose less. *(Genuinely useful from the
  research regardless: **ApeX Omni = a Mantle-native perp DEX** — a real low-fee EXECUTION venue for our VALIDATED
  edges, MNT/SUI, not BTC.)*
- **LESS-EFFICIENT VENUE (Hyperliquid on-chain perp DEX)** — the hypothesis that the CEX-dead edge survives
  on a smaller, less-arbitraged DEX, incl. HL's **exclusive `premium` signal** (perp vs oracle, a direct
  crowd-positioning gauge CEXes don't expose). Tested funding_fade, premium_fade, funding_chg24, premium_chg24,
  premium_mom6 on **4,679 HL BTC 1h bars (195d)** (tested 2026-06-05, `scripts/74`). **ALL fail:** the only
  fee-clearer is `premium_mom6` (+5.7% OOS, +21.8 bps, clears 1-split) but it's a **walk-forward artifact**
  (3/5 folds, **ex-best −2.06%**) — the identical decile/high-vol trap. The DEX is efficient too at the 1h/24h
  horizon. *(HL retains only ~200d of 1h candles; funding history goes ~540d but candles cap the window.)*

- **SHORT-TIMEFRAME ML (RF/XGBoost on 5m, EMA/RSI/Stoch/ATR + triple-barrier SL/TP)** — a collaborator's
  UAS project (`Hasil_Test_BTCUSD.md`) reported BTC 5m RF **+36% ROI / PF 2.22 / 62% win**, but with NO fees,
  17 days of single-regime Hyperliquid data, test-set-tuned confidence, and **CV accuracy 49% (≈ random)**.
  Re-tested rigorously (`scripts/75`, 2026-06-07): **52,441 bars / 182d Binance 5m, 5-fold walk-forward,
  OOS-picked confidence, real taker+slippage cost, vs Buy&Hold.** Result: **zero-fee mean only +0.8% (3/5
  folds)** — the "+36%" was a tiny-sample/selection-bias artifact; the model has ~no directional skill
  (matching its own CV 49%). **WITH realistic fees: ALL 5 folds NEGATIVE, mean −12.8%** (per-trade −13 to
  −90 bps vs ~13 bps round-trip cost on the ATR-tight leveraged sizing). Same fee-wall as everything else.
  **Verdict: technical-ML on 5m BTC has no cost-surviving edge.** Honest ABSTAIN holds. *(The UAS doc's own
  §8/§9 already said "belum deploy-ready" — this just proved it on 10× the data.)*

### 🔭 FRONTIERS still open to research (BTC may be less efficient on these):
1. **Liquidation cascades:** forced liquidations overshoot → fade the cascade.
   - Sources: Coinglass liquidation API, Binance `/fapi/v1/forceOrders`, Bybit liquidation stream, Hyperliquid. (Historical retention is the hard part.)
   - Hypothesis: large liquidation spikes → short-term reversal?
2. **On-chain (BTC):** exchange netflows, miner flows, LTH supply, SOPR, MVRV.
   - Sources: CryptoQuant, Glassnode (paywall), Blockchain.com, mempool.space, Santiment.
   - Hypothesis: large exchange-inflow (sell pressure) precedes drops?

### ✅ VALIDATION PROTOCOL (so any finding is comparable — REQUIRED):
A signal "works" ONLY if, on **real BTC data**, **out-of-sample** (params from train only, no peeking):
```
net ROI > 0  AND  > buy&hold  AND  avg_bps/trade comfortably > 11 (round-trip taker fee)  AND  >= 20 trades
AND walk-forward robust: 5 anchored folds, majority positive AND STILL positive after dropping the single best fold
```
A single good backtest is NOT a yes (multiple-testing trap). **Deliverable:** signal definition + data source +
the backtest grid (ROI / avg_bps / trades / fold-by-fold). Honesty > a flashy number — a testnet/leverage
profit is NOT evidence.
