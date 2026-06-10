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

**11. We validated an edge on MNT — then our own self-learning RETIRED it. That's the product.**
Drilling the OI-contrarian signal per asset, **MNT** once cleared the cost-aware out-of-sample bar: **58.8% win rate, 34 trades, gross ~+90 bps/trade.** Net of a *realistic* execution cost — **fee + spread + slippage (~20 bps round-trip), not the optimistic fee-only 11 bps** — it returned **+25.1% OOS, 1.22 payoff** (a 100-trade live test taught us a real fill pays the spread too, so we validate against it; the fee-only figure was +28.9%). But we kept testing it. On **fresh full-history data** (13,000 hourly bars to 2026-06-07) the edge had **decayed**: strong (+66% to +76%) through April 2026, then the recent regime flipped it to **−7.2% OOS**. Our self-learning loop **demoted it on the evidence** — `validated_edges.json` is now **empty**. Most "AI traders" would still be quoting the +28.9%. We would rather hold an empty registry than trade a dead edge on a stale number. The edge was never the product — *the discipline that retires a decayed flagship is.* (BTC's edge is eaten by costs; ETH/SOL flip to momentum and lose — so the registry stays honestly empty rather than forcing a trade.)

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
**Seven** real-source specialist desks — Regime ML · Macro (Allora) · On-chain Capital-Flow · Smart-Money (Nansen) · Smart-Social (Elfa) · Research · **OI-Contrarian (once the validated edge — since decayed and demoted, see #11)** — debate bull-vs-bear, then a Portfolio Manager synthesizes a disciplined decision whose hash is **anchored in a live Mantle transaction**. ERC-8004 identity registries + a TradingVault are deployed and verified on Mantle Sepolia. With the registry now empty, it *abstains* by default — and that's a feature.

**15. We rejected our own +96% backtest.**
A market-neutral mETH/ETH convergence strategy looked spectacular — **+96% out-of-sample, 95.6% win rate, 17.9 payoff** — but it must trade thin mETH on both legs. Under realistic slippage it turns to **−73 bps per trade**: a thin-liquidity artifact, not alpha. We threw it out. *The discipline that rejects a 96% backtest is worth more than the backtest.*

**16. Edge fired — and the firm still held (proven live, not claimed).**
In a live multi-desk run on MNT, the **OI-Contrarian edge** (then-validated, +28.9% OOS — since decayed and demoted, see #11) fired an actionable LONG. The PM still **ABSTAINED**: the hard **R:R ≥ 2:1 gate couldn't clear**, and the macro read **Extreme Fear** (Fear&Greed 11) with bearish on-chain flow. A tradeable buy signal in hand — declined on structure. Discipline here isn't a slogan; it's what the firm actually did with a live edge — and the same discipline later retired that very edge when it stopped working.

**17. We hunted four more assets — and earned none of them.**
We fetched real data and ran the same cost-aware out-of-sample validation on **BTC, ETH, mETH, and HYPE**. None cleared the bar: BTC's OI edge is **eaten by fees** (+8 bps < the ~11 bps round-trip), **ETH/SOL flip to momentum and lose**, **mETH has no liquid perp** to read positioning from, and **HYPE's open-interest moves *with* price** (momentum, not contrarian) and underperforms buy-and-hold (~31 days of OI is also too short to claim anything). **MNT was the single edge to clear the bar** — and it has since decayed on fresh data and been demoted (see #11). A firm that only trades what it can prove — and *retires what stops proving out.*

**18. AGGRESSIVE sizing is earned, not granted — proven as a live truth-table.**
HeliQuant's high-conviction mode (fractional-Kelly, capped at **3% risk / 5× leverage**, with a 20% drawdown breaker) unlocks **if-and-only-if** the asset has an OOS-validated edge (≥20 trades) *and* the account isn't past a 20% drawdown. We proved it on real MNT data as a 4-case truth-table: validated edge + healthy account → 🟢 **AGGRESSIVE**; edge with only 12 trades → SAFE; validated edge but −22% drawdown → **breaker forces SAFE**; an asset with no edge → SAFE. The aggression dial is **risk %, not leverage** (a tight stop can yield 5× notional while still SAFE). Big positions are a *consequence of proven edge* — never a mood.

**19. 39% win rate, still +24% out-of-sample — payoff over hit-rate, on a funded account.**
Funded with **$1,000** and trading the validated edge **out-of-sample** with live AGGRESSIVE sizing, HeliQuant won only **13 of 33 trades (39.4%)** — and still finished at **$1,243.81 (+24.4%, −11.5% max drawdown)**, net of fees on real Bybit MNTUSDT data. The profit comes from **payoff asymmetry** (winners far larger than losers), not from being right often. An honest edge doesn't need a high hit-rate; it needs positive expectancy that survives costs on unseen data. *(This is the MNT edge as validated on its original window — +28.9% / 34 trades; it has since decayed and been demoted, see Finding 11. An honest record of that window, not a forward claim.)*

**20. We built an asset-onboarding lab — and it held back a +92% candidate.**
HeliQuant now onboards *any* asset through a hypothesis library (open-interest, price-momentum, funding, order-flow imbalance) tested under one gate: **cost-aware out-of-sample + 5-fold walk-forward + an outlier-robustness check** (drop the single best fold — the edge must *still* hold). Scanning **6 assets × 4 hypotheses = 24 tests**, only **MNT's OI-contrarian** cleared it robustly at the time (4/5 folds positive, +28.9% OOS — since decayed on fresh data and demoted, #11). **HYPE's order-flow-contrarian flashed +92% OOS** — but ~65% of that came from a single fold; drop it and the edge collapses to **+1.9%**. So we **held HYPE back**: a tantalizing candidate, *not* a validated edge. The registry grows only when an edge is *earned across folds* — never when one window dazzles. This is the self-learning loop's honest spine: new assets (and new hypotheses, incl. ML) must out-earn the gate.

**21. Validation isn't just claimed — it's anchored on Mantle.**
When an edge clears HeliQuant's gate, its **validation record** (asset, edge, p_win, payoff, sample_n, OOS-ROI, and the firm's ERC-8004 tokenId) is hashed (SHA-256) and **anchored in a Mantle Sepolia transaction**. MNT's OI-contrarian edge is anchored live — tx `0x5ae63fd1…`, **block 39,509,049, status SUCCESS**; the transaction's calldata *is* the record hash, recomputable by anyone from the public record. So "validated" is **auditable, not asserted**: earn the edge → graduate it → record it on-chain. *(Honest about what it is: a tamper-proof proof-of-existence hash — not a trade, not capital moving. And this edge has since decayed on fresh data and been demoted — the anchor is an immutable snapshot of what validated at block-time, not a claim it still works; see #11.)*

**22. We tested ML too — and it didn't get a free pass.**
Honoring the idea of "an ML model per asset," we trained **RandomForest and XGBoost** on MNT's engineered features to predict the 24h direction, then traded their high-confidence calls out-of-sample under the **same cost-aware gate** as every other edge. Result: **~51% accuracy** (barely above a coin-flip at a 24h horizon) and **−50% to −62% OOS net of fees** — both **abstain**. ML is welcome as a *hypothesis*, but it must earn the bar like everything else; on MNT it doesn't, so when MNT's OI-contrarian edge was validated it stayed a transparent **rule, not a black box** (that edge has since decayed and been demoted — see #11; ML never earned a spot in the first place). *(Accuracy ≠ money-weighted profit — Finding 5, now confirmed with real models. ML plugs in as just another signal source, judged by the same evidence.)*

**23. Self-learning, proven on LIVE-fetched data — and it still refused to chase the flip.**
We fetched **fresh HYPE market data** (Bybit, new bars the edge had never seen) and re-ran the full gate — genuinely new evidence, not a replay. HYPE's order-flow-contrarian edge **crossed**: **+142% OOS, walk-forward robust** — where days earlier it was *fragile*. So the loop **responded to evidence**. But it did **NOT** graduate it: a *just-crossed*, **regime-dependent** edge (HYPE's been in a parabolic bull — fading sell-pressure = buying dips, which flatters in a bull) goes on **PROBATION**, requiring confirmation across **consecutive NEW-DATA cycles** before any live capital (re-running the *same* data doesn't count — anti-overfit). The validated registry stays **MNT-only**. *Promote on evidence, confirm before you trust — that's the line between self-learning and self-deception.*

**24. We re-tested a "+36% BTC" ML backtest the honest way — it became −13%.**
A collaborator's RandomForest/XGBoost strategy reported **+36% ROI, 2.22 profit factor, 62% win** on BTC 5-minute bars. We replicated it on **10× the data (52,441 bars / 182 days, vs 17)** and added the four things its own honesty notes flagged as missing: **real fees + slippage, 5-fold walk-forward, an out-of-sample confidence threshold (not tuned on the test set), and a buy-and-hold baseline.** Result: even *zero-fee*, the walk-forward mean was just **+0.8%** — the +36% was a tiny-window + selection-bias artifact (the model's cross-validated direction accuracy was **49%, worse than a coin-flip**); **with realistic fees, all five folds were negative (mean −12.8%).** A beautiful backtest is not an edge. We settle it on data, not opinion.

**25. The honest way to "profit on BTC" is to stop predicting it — and even then BTC is the wrong target.**
After BTC's *directional* edge failed every test (it's the most efficient market there is), we asked how people *actually* profit on BTC: not by predicting it, but by **harvesting funding** — delta-neutral (long spot + short perp, collect the perpetual funding rate, zero directional bet). On a year of real funding, BTC carry nets **+3.6%/yr — below a ~5% stablecoin yield.** Even the non-predictive path is **structurally thin on BTC** (its funding is the lowest, most arbitraged). The *strategy* is sound, though: the same carry pays **+7.2%/yr on HYPE and +5.1% on SUI**, and we **crash-stress-tested** it — HYPE's basis held within **29 bps through a −17% crash** (survivable), while SUI's dislocated to **−166 bps** in its worst crashes (richer yield = real crash-risk *premium*, not free money). Honest bonus lesson: the "clever" variant that only collects positive funding **lost −26%** to switching fees — *always-on beat clever.* Doing less won.

**26. Probation works both ways — HYPE's +142% snapshot DECAYED on new data, and we'd already held it.**
Finding 23 put HYPE's order-flow edge on **probation** instead of graduating its +142% snapshot. Days later, on **genuinely new market data**, that edge **collapsed** — first to 2-of-5 walk-forward folds, then to **earning no edge at all** (its contrarian signal stopped even beating buy-and-hold once HYPE rallied). Had we trusted the +142%, we'd now be sizing live capital on a dead edge. We **hard-coded the discipline into the engine**: a confirmation counts only over a *meaningful* window of new data (≥~20 hours past the last), so a few fresh bars can never rush a graduation — the anti-overfit rule lives in code, not just our heads. Meanwhile **MNT re-validated identically (+28.9%, 4/5 folds) on the fresh data that killed HYPE *at that snapshot*** — the test that breaks fragile edges is exactly the one a real edge passes. *(Honest update: on still-newer full-history data MNT's edge then decayed too — +66–76% through April 2026, then −7.2% OOS — and the same loop demoted it, registry now empty; see #11. The discipline that demoted HYPE later demoted our own flagship.)* *Promote on evidence, confirm before you trust — and let the data demote, too.*

**27. We built the engine to be brave — then proved bravery without an edge is just faster ruin, and gated it.**
Asked to "trade more aggressively," we first tested *how* aggression pays. On real candles, out-of-sample, path-aware (intrabar high/low, ~20 bps round-trip cost), we compared exit policies — a 4-hour cap vs **letting winners run on a trailing stop** — and conviction sizing. The result is a law, not a knob: **let-winners-run and size-up are *multipliers*, not edges.** On **MNT** (where a signal exists) the trailing stop turned **−8.8% into +15.4%**; on **BTC and HYPE** (no edge) the *same* lever deepened losses (**−50% → −57%**, **−65% → −65%**), and applied blindly across a 6-asset basket it **wiped out (−98% pooled).** So we wired aggression into the live engine **gated on a registry-validated edge**: no-edge assets keep tight stops and base size (unchanged); an edge asset gets the trailing stop and, when the regime backs it, **2× size**. Then we went hunting for an edge to justify firing it — and the gate did its job (next finding). *Aggression is earned per-asset by evidence, or it doesn't happen at all.*

**28. We found a +64% edge this turn — and our own robustness gate rejected it, live.**
Re-running the asset-onboarding lab right now, MNT's OI-contrarian signal flashed exactly what a careless team would ship: **+64.0% out-of-sample, 59 trades, 57.6% win, payoff 1.43, p-value 0.025** — a clean "passed" on the headline test. We didn't ship it. The **stability gate** re-tests the edge across shrinking data windows; MNT's held at the 0.8 and 1.0 cutoffs but **failed at 0.9 (`robust_at = [true, false, true]` → `stable: false`)** — the +64% lives in *part* of the history and vanishes in another. Window-dependent, not durable. The lab **refused to write it to the validated registry**, which therefore **stays empty `{}`** — so the brave engine from Finding 27 is **armed but holding fire** (0 edge positions, 0 size-ups, live). The other five assets failed outright (BTC p-value 0.27; ETH/SOL/SUI negative OOS). Competitors print the +64% on a landing page; **we held it in our hands, stress-tested it, watched it break, and said no** — and we can show you the rejection. *The discipline is the product; the receipts are public.*

---

*HeliQuant's edge isn't a magic indicator — it's rigor, multi-source integration, Mantle-native verifiability, and brutal honesty. Verified by real runs; auditable on-chain.*
