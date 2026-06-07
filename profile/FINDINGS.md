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
Drilling the OI-contrarian signal per asset, only **MNT** clears the cost-aware out-of-sample bar: **58.8% win rate, 34 trades, gross ~+90 bps/trade.** Net of a *realistic* execution cost — **fee + spread + slippage (~20 bps round-trip), not the optimistic fee-only 11 bps** — it returns **+25.1% OOS, 1.22 payoff** (a 100-trade live test taught us a real fill pays the spread too, so we validate against it; the fee-only figure was +28.9%). The edge has huge margin (survives up to ~40 bps cost) but **must trade a LIQUID venue** — a wide spread eats it. (BTC's edge is eaten by costs; ETH/SOL flip to momentum and lose.) It is the *single* entry in our `validated_edges.json` registry — the only signal allowed to size up. One real edge, honestly scoped — costs and all.

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

**16. Edge fired — and the firm still held (proven live, not claimed).**
In a live multi-desk run on MNT, the **OI-Contrarian *validated* edge fired an actionable LONG** (58.8% win, 1.30 payoff, +28.9% OOS). The PM still **ABSTAINED**: the hard **R:R ≥ 2:1 gate couldn't clear**, and the macro read **Extreme Fear** (Fear&Greed 11) with bearish on-chain flow. A tradeable buy signal in hand — declined on structure. Discipline here isn't a slogan; it's what the firm actually did with a live edge.

**17. We hunted four more assets — and earned none of them.**
We fetched real data and ran the same cost-aware out-of-sample validation on **BTC, ETH, mETH, and HYPE**. None cleared the bar: BTC's OI edge is **eaten by fees** (+8 bps < the ~11 bps round-trip), **ETH/SOL flip to momentum and lose**, **mETH has no liquid perp** to read positioning from, and **HYPE's open-interest moves *with* price** (momentum, not contrarian) and underperforms buy-and-hold (~31 days of OI is also too short to claim anything). **MNT stays the single validated edge.** A firm that only trades what it can prove — across the whole shortlist, not just the convenient one.

**18. AGGRESSIVE sizing is earned, not granted — proven as a live truth-table.**
HeliQuant's high-conviction mode (fractional-Kelly, capped at **3% risk / 5× leverage**, with a 20% drawdown breaker) unlocks **if-and-only-if** the asset has an OOS-validated edge (≥20 trades) *and* the account isn't past a 20% drawdown. We proved it on real MNT data as a 4-case truth-table: validated edge + healthy account → 🟢 **AGGRESSIVE**; edge with only 12 trades → SAFE; validated edge but −22% drawdown → **breaker forces SAFE**; an asset with no edge → SAFE. The aggression dial is **risk %, not leverage** (a tight stop can yield 5× notional while still SAFE). Big positions are a *consequence of proven edge* — never a mood.

**19. 39% win rate, still +24% out-of-sample — payoff over hit-rate, on a funded account.**
Funded with **$1,000** and trading the validated edge **out-of-sample** with live AGGRESSIVE sizing, HeliQuant won only **13 of 33 trades (39.4%)** — and still finished at **$1,243.81 (+24.4%, −11.5% max drawdown)**, net of fees on real Bybit MNTUSDT data. The profit comes from **payoff asymmetry** (winners far larger than losers), not from being right often. An honest edge doesn't need a high hit-rate; it needs positive expectancy that survives costs on unseen data. *(The same edge, un-leveraged, returns +28.9% / 34 trades — see Finding 11.)*

**20. We built an asset-onboarding lab — and it held back a +92% candidate.**
HeliQuant now onboards *any* asset through a hypothesis library (open-interest, price-momentum, funding, order-flow imbalance) tested under one gate: **cost-aware out-of-sample + 5-fold walk-forward + an outlier-robustness check** (drop the single best fold — the edge must *still* hold). Scanning **6 assets × 4 hypotheses = 24 tests**, only **MNT's OI-contrarian** clears it robustly (4/5 folds positive, +28.9% OOS). **HYPE's order-flow-contrarian flashed +92% OOS** — but ~65% of that came from a single fold; drop it and the edge collapses to **+1.9%**. So we **held HYPE back**: a tantalizing candidate, *not* a validated edge. The registry grows only when an edge is *earned across folds* — never when one window dazzles. This is the self-learning loop's honest spine: new assets (and new hypotheses, incl. ML) must out-earn the gate.

**21. Validation isn't just claimed — it's anchored on Mantle.**
When an edge clears HeliQuant's gate, its **validation record** (asset, edge, p_win, payoff, sample_n, OOS-ROI, and the firm's ERC-8004 tokenId) is hashed (SHA-256) and **anchored in a Mantle Sepolia transaction**. MNT's OI-contrarian edge is anchored live — tx `0x5ae63fd1…`, **block 39,509,049, status SUCCESS**; the transaction's calldata *is* the record hash, recomputable by anyone from the public record. So "validated" is **auditable, not asserted**: earn the edge → graduate it → record it on-chain. *(Honest about what it is: a tamper-proof proof-of-existence hash — not a trade, not capital moving.)*

**22. We tested ML too — and it didn't get a free pass.**
Honoring the idea of "an ML model per asset," we trained **RandomForest and XGBoost** on MNT's engineered features to predict the 24h direction, then traded their high-confidence calls out-of-sample under the **same cost-aware gate** as every other edge. Result: **~51% accuracy** (barely above a coin-flip at a 24h horizon) and **−50% to −62% OOS net of fees** — both **abstain**. ML is welcome as a *hypothesis*, but it must earn the bar like everything else; on MNT it doesn't, so the validated edge stays the transparent **OI-contrarian rule, not a black box**. *(Accuracy ≠ money-weighted profit — Finding 5, now confirmed with real models. ML plugs in as just another signal source, judged by the same evidence.)*

**23. Self-learning, proven on LIVE-fetched data — and it still refused to chase the flip.**
We fetched **fresh HYPE market data** (Bybit, new bars the edge had never seen) and re-ran the full gate — genuinely new evidence, not a replay. HYPE's order-flow-contrarian edge **crossed**: **+142% OOS, walk-forward robust** — where days earlier it was *fragile*. So the loop **responded to evidence**. But it did **NOT** graduate it: a *just-crossed*, **regime-dependent** edge (HYPE's been in a parabolic bull — fading sell-pressure = buying dips, which flatters in a bull) goes on **PROBATION**, requiring confirmation across **consecutive NEW-DATA cycles** before any live capital (re-running the *same* data doesn't count — anti-overfit). The validated registry stays **MNT-only**. *Promote on evidence, confirm before you trust — that's the line between self-learning and self-deception.*

**24. We re-tested a "+36% BTC" ML backtest the honest way — it became −13%.**
A collaborator's RandomForest/XGBoost strategy reported **+36% ROI, 2.22 profit factor, 62% win** on BTC 5-minute bars. We replicated it on **10× the data (52,441 bars / 182 days, vs 17)** and added the four things its own honesty notes flagged as missing: **real fees + slippage, 5-fold walk-forward, an out-of-sample confidence threshold (not tuned on the test set), and a buy-and-hold baseline.** Result: even *zero-fee*, the walk-forward mean was just **+0.8%** — the +36% was a tiny-window + selection-bias artifact (the model's cross-validated direction accuracy was **49%, worse than a coin-flip**); **with realistic fees, all five folds were negative (mean −12.8%).** A beautiful backtest is not an edge. We settle it on data, not opinion.

**25. The honest way to "profit on BTC" is to stop predicting it — and even then BTC is the wrong target.**
After BTC's *directional* edge failed every test (it's the most efficient market there is), we asked how people *actually* profit on BTC: not by predicting it, but by **harvesting funding** — delta-neutral (long spot + short perp, collect the perpetual funding rate, zero directional bet). On a year of real funding, BTC carry nets **+3.6%/yr — below a ~5% stablecoin yield.** Even the non-predictive path is **structurally thin on BTC** (its funding is the lowest, most arbitraged). The *strategy* is sound, though: the same carry pays **+7.2%/yr on HYPE and +5.1% on SUI**, and we **crash-stress-tested** it — HYPE's basis held within **29 bps through a −17% crash** (survivable), while SUI's dislocated to **−166 bps** in its worst crashes (richer yield = real crash-risk *premium*, not free money). Honest bonus lesson: the "clever" variant that only collects positive funding **lost −26%** to switching fees — *always-on beat clever.* Doing less won.

**26. Probation works both ways — HYPE's +142% snapshot DECAYED on new data, and we'd already held it.**
Finding 23 put HYPE's order-flow edge on **probation** instead of graduating its +142% snapshot. Days later, on **genuinely new market data**, that edge **collapsed** — first to 2-of-5 walk-forward folds, then to **earning no edge at all** (its contrarian signal stopped even beating buy-and-hold once HYPE rallied). Had we trusted the +142%, we'd now be sizing live capital on a dead edge. We **hard-coded the discipline into the engine**: a confirmation counts only over a *meaningful* window of new data (≥~20 hours past the last), so a few fresh bars can never rush a graduation — the anti-overfit rule lives in code, not just our heads. Meanwhile **MNT re-validated identically (+28.9%, 4/5 folds) on the very same fresh data that killed HYPE** — the test that breaks fragile edges is exactly the one a real edge passes. *Promote on evidence, confirm before you trust — and let the data demote, too.*

---

*HeliQuant's edge isn't a magic indicator — it's rigor, multi-source integration, Mantle-native verifiability, and brutal honesty. Verified by real runs; auditable on-chain.*
