# 🔭 HeliQuant — Findings & Insights

> Honest, data-backed discoveries from building an autonomous trading-intelligence org on Mantle.
> **Every finding traces to a real run/probe.** We publish what DOESN'T work too — honesty is the product.
> (Landing-page ready — each item = headline + plain-English insight + evidence.)

---

## 🐋 On Smart Money & Whales — the Mantle reality

**1. Mantle's smart money is genuinely sparse — and we confirmed it across 4 premium sources.**
Etherscan, Cielo, CoinAlyze, *and* Nansen (a premium smart-money platform) all converge on the same truth: only **~1–2 individual smart-money traders** are active on Mantle's core assets (mETH has just 2 real conviction wallets; Nansen sees 1 trader on MNT). It's not a data-access gap — *the activity isn't there yet.* So HeliQuant doesn't fake "whale-following" on Mantle; it tracks what's actually real.

**2. The dynamic Whale-vs-Contract insight.**
*Who holds a token* decides the signal. EOA-rich assets (BTC/ETH) → track individual whales & perp positioning. Contract-heavy assets (Mantle LSTs, where bridges/staking/LPs dominate) → track *contract flows*. HeliQuant measures holder composition on-chain (`eth_getCode`) and **auto-selects the mode per asset** — one engine, two brains.

**3. You can't track whales one wallet at a time anymore — aggregate or lose.**
~80% of transactions now route through private mempools; ghost-deposits and wallet-splitting are standard manipulation. Single-wallet alerts are gameable by design. HeliQuant uses **aggregate cohort flow** (manipulation-robust), not single-address alerts.

**4. mETH staking is the real Mantle conviction signal.**
While individual-whale data is thin, **~$118M of ETH is locked in mETH staking** — a hard-to-fake, on-chain conviction metric. That's a signal worth tracking; a single "$50k transfer to Binance" alert isn't.

---

## 📊 On Alpha & Strategies — what's real, what isn't

**5. Accuracy ≠ Profit.**
Our ML regime classifier reads market regime at **82–88% out-of-sample accuracy** — yet trading those predictions did *not* produce profit in simulation. The lesson most projects hide: classification accuracy is not cost-adjusted, money-weighted profit. So the regime engine's true job is **capital allocation & risk**, not price prediction.

**6. No magic price-TA alpha — and we say so out loud.**
Trend-following and mean-reversion both **fail rigorous walk-forward out-of-sample**. Eye-catching single-window returns we saw early on were artifacts that died under full-history testing. We reject overfit — every strategy must survive unseen data.

**7. The one real edge: OI-Contrarian (hedge-like).**
Open-interest build-up tends to mean-revert. Positioning *contrarian* to it produced **+47–64% aggregate out-of-sample while the market fell ~60%** — an anti-correlated, hedge-like return stream. Honest caveat: amplified by a bear environment and inconsistent fold-to-fold, so we deploy it as a **risk-controlled hedge**, not an all-weather guarantee.

**8. Funding-capture doesn't work on these assets.**
Funding rates here are too small and flip sign too often — trading fees eat the carry (a naive version lost 30–39%). Tested, measured, rejected.

**11. The one edge, validated and deployable — on MNT specifically.**
Drilling the OI-contrarian signal per asset, only **MNT** clears the cost-aware out-of-sample bar: **58.8% win rate, 1.30 payoff, 34 trades, +28.9% OOS**. (BTC's edge is eaten by fees; ETH/SOL flip to momentum and lose.) It is the *single* entry in our `validated_edges.json` registry — and the only signal allowed to size up. One real edge, honestly scoped — not a dashboard of imaginary ones.

---

## ⚙️ On Execution & Risk — how it actually trades

**12. A trade is a hypothesis with an invalidation — gated, not guessed.**
Every decision becomes a structured ticket: an entry zone, a **structural stop** (beyond a real swing level, not an arbitrary %), a separate invalidation, and a take-profit ladder. A hard **risk/reward ≥ 2:1 gate** rejects setups that don't pay — so even a *validated* edge **abstains** when the structure is poor. The LLM judges direction; deterministic code computes the numbers; the gate enforces safety.

**13. Aggression earned by data — not timid, not reckless.**
Most "AI traders" are either tiny-fixed-bet toys or max-leverage-on-vibes gamblers. HeliQuant is dynamic: **SAFE by default**, sizing up to **AGGRESSIVE** (fractional-Kelly, capped at 3% risk / 5× leverage, with a 20% drawdown circuit-breaker) **only** when an OOS-validated edge's live signal fires *and* agrees with the decision. Big positions are a *consequence of measured edge*, never a wish.

**14. Verifiable on Mantle — the decision ledger, on-chain.**
Each decision's hash is anchored in a live Mantle transaction (e.g. `0xa052ee03…`, block 39,402,623 — auditable on Mantlescan), under a **broadcast-on-ENTER** policy: real trades go on-chain, abstentions stay gas-frugal. On-chain is the tamper-proof *audit trail*; portfolio state lives off-chain — proof without theatrics.

---

## 🔬 On Method — the differentiator

**9. Honesty-by-design.**
Deterministic tools compute the numbers; LLM agents debate and decide; the system **rejects any strategy that doesn't survive out-of-sample**. We never publish a return we can't reproduce. *(We even retracted our own earlier headline numbers when they failed full-history testing.)*

**10. A firm of AI agents — recorded on Mantle.**
**Seven** real-source specialist desks — Regime ML · Macro (Allora) · On-chain Capital-Flow · Smart-Money (Nansen) · Smart-Social (Elfa) · Research · **OI-Contrarian (the validated edge)** — debate bull-vs-bear, then a Portfolio Manager synthesizes a disciplined decision whose hash is **anchored in a live Mantle transaction**. ERC-8004 identity registries + a TradingVault are deployed and verified on Mantle Sepolia. When no validated edge applies, it *abstains* — and that's a feature.

**15. We rejected our own +96% backtest.**
A market-neutral mETH/ETH convergence strategy looked spectacular — **+96% out-of-sample, 95.6% win rate, 17.9 payoff** — but it must trade thin mETH on both legs. Under realistic slippage it turns to **−73 bps per trade**: a thin-liquidity artifact, not alpha. We threw it out. *The discipline that rejects a 96% backtest is worth more than the backtest.*

---

*HeliQuant's edge isn't a magic indicator — it's rigor, multi-source integration, Mantle-native verifiability, and brutal honesty. Verified by real runs; auditable on-chain.*
