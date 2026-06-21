# 🌞 What is HeliQuant? — The Canonical Reference

> The definitive, in-depth explanation of HeliQuant — for the pitch, the judges, onboarding, and docs. The deepest single document; the GitHub org profile (`README.md`) is the concise version, and `FINDINGS.md` is the evidence log.
>
> **Honesty rule:** every number here is real and traceable to a run/probe. We publish what doesn't work too. *Nothing is invented.*

---

## 1. The one-liner

**HeliQuant — "the all-seeing quant" — is an autonomous, multi-source intelligence trading _firm_ on Mantle.**

Seven AI specialist desks, each reading a different real data source, debate bull-vs-bear. A Portfolio Manager synthesises one disciplined decision. The decision becomes a gated, risk-sized trade ticket, and its hash is anchored on-chain on Mantle. It trades the Mantle ecosystem, sizes up only on an edge that survives cost-aware out-of-sample testing, and abstains — honestly — when no edge applies. (Its one validated edge — OI-Contrarian on MNT — has since **decayed on fresh data and been demoted by the firm's own self-learning loop**; the validated registry is now empty, so the firm abstains by default. That self-retirement, not the edge, is the point.)

> **The name:** *Helios* (the Greek sun god who sees everything from the sky) + *Quant* (quantitative-trading discipline). The brand is the sun — the one source of light/intelligence — with the desks orbiting it.

---

## 2. The thesis — why HeliQuant is different

Most "AI trading" projects are one of two things: a single LLM guessing from a prompt, or a backtest-overfit bot showing a dashboard of imaginary alphas. HeliQuant rejects both.

1. **It's a firm, not a bot.** Seven real-source desks + a PM that synthesises — multi-agent *debate*, not one model's hunch.
2. **Edges are earned — and retired — on evidence.** Exactly one signal (OI-Contrarian on MNT) cleared cost-aware, out-of-sample testing — then **decayed on fresh data**, and our own self-learning loop **demoted it** (the validated registry is now empty). We name the number, keep its honest history, and refuse to quote a stale edge as if it were live. *Most projects would still be quoting the +28.9%.*
3. **Disciplined execution.** Every decision is a structured ticket behind a hard risk/reward gate, with dynamic risk-sizing and a drawdown circuit-breaker.
4. **Mantle-native verifiability.** Decisions are anchored on-chain; identity/reputation/validation contracts are deployed on Mantle.
5. **Brutal honesty is the product.** We rejected our own +96% backtest, retracted earlier headline numbers, and treat ABSTAIN as a feature. *The discipline that rejects a 96% backtest is worth more than the backtest.*

---

## 3. The firm — seven real-source desks → one PM

Each desk reads its **own real source** and posts a stance (bullish / bearish / neutral / avoid / n-a) with a confidence and a one-line key point. No desk sees the others' raw data — diversity is the point.

| # | Desk | Real source | What it reads | Role |
|---|---|---|---|---|
| 1 | **Regime / Technical** | XGBoost ML regime classifier (**82.6% OOS**) | trend/range/volatility regime + strength | the capital-allocator; sets dynamic R:R |
| 2 | **Macro (Allora)** | Allora decentralised-AI network | BTC/ETH 8h price prediction, consumed **on-chain** via AlloraConsumer | macro direction |
| 3 | **On-chain / Risk** | Mantlescan (Etherscan v2) + GoPlus | whale flow, watchlist health, token safety | risk + a hard **safety veto** |
| 4 | **Research** | Fear & Greed + public market APIs | global market context | macro sentiment context |
| 5 | **Smart-Money Flow** | dynamic whale/contract flow + Nansen | cohort netflow (rich on majors, sparse on Mantle) | follow real conviction, not single wallets |
| 6 | **Smart-Social** | Elfa | narrative / mindshare from **smart accounts** (not retail noise) | narrative momentum |
| 7 | **OI-Contrarian** | perp Open-Interest extremes | OI 24h-change quintiles | once the one OOS-validated edge — **since decayed on fresh data + demoted** (registry now empty); kept as a desk, no longer sizes up |

> In a typical run, ~5 desks post a directional read; the **OI-Contrarian desk stays "context"** unless its live signal fires — which is why the firm can hold even with desks active.

---

## 4. The decision loop (7 stages)

```
RECALL → PLAN → DEBATE → PM DECISION → TICKET → EXECUTE → LEARN ↻ (back to RECALL)
```

1. **RECALL** *(memory)* — the steward loads prior decisions + the live watchlist from Supabase `decisions_hq`, and re-frames the question: *is there a validated edge to act on now — or is discipline the trade?*
2. **PLAN** *(7 desks)* — every desk reads its real source in parallel and posts a stance + confidence + key point.
3. **DEBATE** *(bull · bear)* — a structured bull-vs-bear debate: opening cases, strongest points, honest caveats, risks, then rebuttals.
4. **PM DECISION** *(verdict)* — the Portfolio Manager synthesises one decision: **ENTER · {LONG|SHORT}** with confidence, or **ABSTAIN**, with plain-English reasoning + desk consensus.
5. **TICKET** *(R:R ≥ 2:1 gate)* — on ENTER a structured ticket is built and gated; on ABSTAIN no ticket is written (flat · 0 size).
6. **EXECUTE** *(record)* — the decision row is written off-chain to `decisions_hq` (always, even on abstain); on ENTER the decision hash is anchored on-chain.
7. **LEARN** *(loop)* — the outcome (TP / SL / timeout — or "abstained, capital preserved") is written back to memory; each run sharpens the next.

---

## 5. Execution discipline — a trade is a hypothesis with an invalidation

Every ENTER decision becomes a **structured ticket** (`firm/trade_ticket.py`):

- **Entry zone** — a range, not a single price.
- **Structural stop** — beyond a real swing level, not an arbitrary %.
- **Separate invalidation** — the price at which the thesis is simply wrong.
- **Take-profit ladder** — TP1 / TP2 …
- **Hard R:R ≥ 2:1 gate** — setups that don't pay are rejected, *even for a validated edge.*

**Dynamic sizing — aggression earned by data:**

- **SAFE** is the default posture (target R:R ≈ 1:2). Not a tiny-fixed-bet toy.
- **AGGRESSIVE** = fractional-Kelly, capped at **≤3% risk** and **≤5× leverage**, with a **20% drawdown circuit-breaker** and a minimum edge-sample of 20 — unlocked **only** when an OOS-validated edge's live signal fires *and* agrees with the decision.
- **The LLM judges direction; deterministic code computes the numbers; the gate enforces safety.** Big positions are a consequence of measured edge, never a wish.

---

## 6. The edge we validated — then RETIRED — OI-Contrarian on MNT

- **What:** fade perp **Open-Interest 24h-change extremes** — top quintile → SHORT, bottom quintile → LONG. OI build-up tends to **mean-revert**; positioning contrarian to the crowd is the edge.
- **MNT numbers on its original ~1-yr window (cost-aware, out-of-sample):** **58.8% win · 1.30 payoff · 34 trades · +28.9% OOS** (fee-only 11 bps) / **+25.1%** net of realistic fee+spread+slippage (~20 bps). *True on that window.*
- **Aggregate behaviour (across majors, binary/extremes):** **+47–64% aggregate OOS while the market fell ~60%** — an anti-correlated, **hedge-like** return stream (also of that bear-market window).
- **THEN IT DECAYED.** On **fresh full-history data** (13,000 hourly bars to 2026-06-07) the edge worked strongly (+66% to +76%) through ~April 2026, then the recent regime flipped it to **−7.2% OOS**. Our self-learning loop **demoted it on the evidence — `data/validated_edges.json` is now EMPTY.** There is currently **no validated predictive edge.**
- **Why this is the strength, not the weakness:** most "AI traders" would still be quoting the +28.9%. We caught our own flagship decaying on fresh data and retired it. **The edge was never the product; the discipline that retires a decayed edge is.**
- **(Context unchanged:)** per-asset, MNT was the *only* asset to clear the bar in the first place — BTC's edge is eaten by fees; ETH/SOL flip to momentum and lose — so with MNT demoted the registry is honestly empty rather than padded with a forced trade.

### What we tested and REJECTED (honesty)

- **Trend-following & mean-reversion** — fail rigorous walk-forward OOS; early single-window returns were artifacts.
- **Funding-capture** — too small, flips sign; fees eat the carry → a naive version **lost 30–39%.**
- **mETH/ETH convergence** — **+96% OOS / 95.6% win / 17.9 payoff** on paper → **−73 bps/trade** under realistic mETH slippage → a thin-liquidity artifact → **thrown out.**
- **Accuracy ≠ profit** — the regime classifier reads regime at 82–88% OOS, but trading it did not profit in sim; its job is allocation, not prediction.

> The full evidence log — **15 data-backed findings** — lives in [`FINDINGS.md`](./FINDINGS.md).

---

## 7. The trading universe — two tiers, one discipline

- **Mantle ecosystem (traded):** **MNT** *(once-validated edge, since decayed + demoted — see §6)* · mETH · cmETH · fBTC · USDe *(monitored)*.
- **Majors (macro context only):** BTC · ETH · SOL.

MNT once carried the only validated edge; it has since decayed on fresh data and been demoted, so **no asset currently carries a validated edge.** Every asset runs the same cost-aware, OOS-gated pipeline — and, lacking a live edge, the disciplined answer is **ABSTAIN.**

---

## 8. Verifiable on Mantle

**The decision ledger, on-chain.** Each decision's hash (SHA-256) is anchored in a live Mantle transaction under a **broadcast-on-ENTER** policy: real entries go on-chain; abstentions stay gas-frugal. On-chain is the tamper-proof **audit trail** — proof a decision existed at a time — **not** fund custody; portfolio state lives off-chain.

- **First decision anchored:** tx `0xa052ee03dca16bbac3b69285e062d6b18349c9809b99e5b186d4f2e91de69b53` · block **39,402,623** · MNT · ABSTAIN · recordHash `0x792b7941…684660`.

**Deployed contracts (Mantle Sepolia, chain 5003):**

| Contract | Address | Role |
|---|---|---|
| IdentityRegistry | `0x0fAE6342195fdc0007B94Fb3293bF56463C55ff3` | ERC-8004 identity |
| ReputationRegistry | `0x5A18F8D33D551666233701025754274dCA9B2929` | ERC-8004 reputation |
| ValidationRegistry | `0x8e55E41dc9a93E30aaf580DBA0B3Ee6B34e14a1B` | ERC-8004 validation |
| AlloraConsumer | `0x7A072465AC232709C114C5DAa842a9b7010D1d4f` | reads Allora predictions on-chain |
| TradingVault | `0x3BbD1f5e8733e901A8FdFf5cFA7E18e575896424` | the firm's on-chain vault |
| JobManager | `0x10421Eb1A230F484eEdB64642505d073e791823c` | job / task coordination |

**HeliQuant ships the first Allora prediction infrastructure ever deployed on Mantle Network** (AlloraConsumer + the off-chain relayer), extending the decentralised-AI inference layer to the ecosystem. First Allora-on-Mantle submission: tx `0x0d7c09…c469`.

---

## 9. Memory & the autonomous loop

- **Memory** (`firm/memory_store.py`): decisions persist to Supabase **`decisions_hq`** (Postgres + pgvector), with an automatic SQLite fallback and an Obsidian-vault markdown mirror. The RECALL↔LEARN loop means the firm **compounds context over time** — it is *not* a claim of self-modifying models.
- **Autonomous loop** (`scripts/52_autonomous_loop.py`): PLAN → EXEC → LEARN on an interval; `--replay` runs a no-LLM backtest, `--once` a single live pass.

---

## 10. Tech stack

| Layer | Tech |
|---|---|
| Intelligence | Allora · Nansen · Elfa · GoPlus · CoinGecko · Mantlescan (Etherscan v2) |
| ML | XGBoost regime classifier · TimeSeriesSplit CV · 39 features |
| Agents | 7-desk org + PM · BYO multi-provider LLM (OpenAI-compatible) · deterministic tools compute, agents decide |
| Memory | Supabase (Postgres + pgvector) — `decisions_hq` |
| Contracts | Foundry · Solidity 0.8.27 · OpenZeppelin v5 · ERC-8004 · EIP-712 |
| Frontend | Next.js 16 · Tailwind v4 · RainbowKit · Wagmi v2 · viem |
| Relayer | Python · web3.py · Allora SDK |

**Bring-your-own credentials** (data-API keys only — never private/service-role keys): Groq/LLM · Mantlescan · Nansen · Elfa · Allora. Keys are encrypted at rest (per-user RLS) in Supabase; the on-chain executor uses a testnet-only key, server-side.

---

## 11. Honest disclosures (the brand)

- The headline isn't a return — it's **rigor.** No all-weather alpha exists here, and we say so.
- Regime accuracy (82.6% OOS) is real but did **not** become simulated profit → allocation, not prediction.
- OI-contrarian validated on its original MNT window (+28.9% OOS, bear-amplified) but **decayed to −7.2% OOS on fresh full-history data** → our self-learning loop **demoted it; the validated registry is now empty.** We retired our own flagship rather than quote a stale number — that self-retirement is the differentiator.
- We **retracted our own earlier headline numbers** when they failed full-history walk-forward, and **rejected a +96% backtest** (thin-liquidity artifact).
- The firm **abstains** when no edge applies — sessions often fire 0 trades. That's correct, disciplined behaviour.

---

## 12. Mantle deployment

Trades **Mantle-ecosystem assets** · **ERC-8004** agent identity · deployed + verified on **Mantle Sepolia** · backtesting as verifiability.

---

*Related docs: [`README.md`](./README.md) (concise org profile) · [`FINDINGS.md`](./FINDINGS.md) (15 findings + evidence) · landing-page content specs in `Resources/Frontend/HeliQuant-FE/LandingPage/`.*
