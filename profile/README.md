<div align="center">

# 🌞 HeliQuant

**The all-seeing quant.**

Autonomous multi-source intelligence trading **firm** on Mantle Network.

</div>

---

## What is HeliQuant?

**HeliQuant** — from *Helios* (the Greek sun god who sees everything from the sky) + *Quant* (quantitative discipline) — is an **autonomous trading firm of seven AI desks.** Each desk reads a different **real** data source and posts a stance; they run a **bull-vs-bear debate**; a **Portfolio Manager** synthesises one disciplined decision; that decision becomes a **gated, risk-sized trade ticket**; and its hash is **anchored on-chain on Mantle.**

It trades the **Mantle ecosystem**, sizes up **only** on an edge that survives cost-aware out-of-sample testing, and **abstains — honestly — when no edge applies.** Its one validated edge (OI-Contrarian on MNT) has since **decayed on fresh data and been demoted** — the registry is now empty, so the firm abstains by default. *That self-retirement is the point, not a setback.* Built for the **Mantle Turing Test Hackathon 2026**, Track 1 (AI Trading & Strategy).

### The firm — seven real-source desks → one PM

| Desk | Real source | Role |
|---|---|---|
| **Regime / Technical** | ML regime classifier (**82.6% OOS**) | trend strength + dynamic R:R — the capital-allocator |
| **Macro (Allora)** | Allora decentralised-AI | BTC/ETH 8h price prediction, consumed on-chain on Mantle |
| **On-chain / Risk** | Mantlescan + GoPlus | whale flow + watchlist health + token-safety **veto** |
| **Research** | Fear & Greed + public APIs | external macro context |
| **Smart-Money Flow** | dynamic whale/contract flow + **Nansen** | smart-money netflow (rich on majors, sparse on Mantle) |
| **Smart-Social** | **Elfa** | narrative / mindshare from smart accounts, not retail |
| **OI-Contrarian** | perp Open-Interest extremes | once the one OOS-validated edge — **since decayed on fresh data + demoted** (registry now empty); kept as a desk, no longer sizes up |

### The decision loop

```
RECALL → PLAN → DEBATE → PM DECISION → TICKET → EXECUTE → LEARN ↻
memory   7 desks  bull/bear   verdict     gate     record    loop
```

A trade is a **hypothesis with an invalidation**, never a guess: every ENTER ticket has an entry zone, a **structural stop**, a separate invalidation, and a TP-ladder, behind a hard **risk/reward ≥ 2:1 gate**. **SAFE** by default; **AGGRESSIVE** (fractional-Kelly, **≤3% risk / ≤5× leverage**, with a **20% drawdown circuit-breaker**) unlocks **only** when a validated edge's live signal fires *and* agrees — and with the registry currently empty (our MNT edge decayed and was demoted), the firm stays in disciplined SAFE/abstain mode by design. The LLM judges direction; deterministic code computes the numbers; the gate enforces safety.

### What's validated — and what isn't (honesty first)

| Claim | Status |
|---|---|
| **Regime classifier** | **82.6% out-of-sample accuracy** — verified |
| **OI-Contrarian on MNT** | once the one edge surviving cost-aware OOS: **58.8% win · 1.30 payoff · 34 trades · +28.9% OOS** (true on its ~1-yr window) — but **DECAYED to −7.2% OOS on fresh full-history data** (strong +66–76% through Apr 2026, then the regime killed it). Our self-learning loop **demoted it; `validated_edges.json` is now empty.** Most projects would still quote the +28.9% — we retired it instead |
| Trend-following / mean-reversion | ❌ **none** — fail full-history walk-forward; early single-window returns were artifacts (**retracted**) |
| Funding-capture | ❌ tested → lost 30–39% (fees eat the carry) → rejected |
| mETH/ETH convergence | ❌ looked spectacular (**+96% OOS, 95.6% win**) → **−73 bps/trade** under realistic slippage → a thin-liquidity artifact → **we rejected our own backtest** |
| Accuracy → profit | accuracy is *not* money-weighted profit; the regime engine's job is **capital allocation**, not prediction |

We publish what *doesn't* work too. The firm **abstains** when no validated edge applies — a feature, not a failure.

### Verifiable on Mantle

- **Decision ledger on-chain:** each decision's hash is anchored in a live Mantle transaction under a **broadcast-on-ENTER** policy (abstentions stay gas-frugal). First decision anchored: [tx `0xa052ee03…de69b53`](https://sepolia.mantlescan.xyz/tx/0xa052ee03dca16bbac3b69285e062d6b18349c9809b99e5b186d4f2e91de69b53) · block 39,402,623.
- **First Allora prediction infrastructure ever deployed on Mantle** — extending the decentralised AI inference layer to the ecosystem.

---

## Repositories

| Repo | What |
|---|---|
| **[contracts](https://github.com/HeliQuant/contracts)** | Solidity — ERC-8004 (identity/reputation/validation) + AlloraConsumer + TradingVault + JobManager, deployed + verified on Mantle Sepolia |
| **[agents](https://github.com/HeliQuant/agents)** | Python — the 7-desk autonomous firm + ML regime classifier + multi-source intelligence + backtests |
| **[relayer](https://github.com/HeliQuant/relayer)** | Off-chain Allora → Mantle on-chain prediction bridge (first Allora infra on Mantle) |
| **[frontend](https://github.com/HeliQuant/frontend)** | Next.js dApp — the immersive "Observatory" landing + live decision ledger |
| **[docs](https://github.com/HeliQuant/docs)** | Architecture, findings, deployment records |

---

## Live on Mantle Sepolia (chain 5003)

All six core contracts deployed + verified on Mantlescan.

| Contract | Address |
|---|---|
| IdentityRegistry (ERC-8004) | [`0x0fAE6342…C55ff3`](https://sepolia.mantlescan.xyz/address/0x0fAE6342195fdc0007B94Fb3293bF56463C55ff3) |
| ReputationRegistry (ERC-8004) | [`0x5A18F8D3…B2929`](https://sepolia.mantlescan.xyz/address/0x5A18F8D33D551666233701025754274dCA9B2929) |
| ValidationRegistry (ERC-8004) | [`0x8e55E41d…4a1B`](https://sepolia.mantlescan.xyz/address/0x8e55E41dc9a93E30aaf580DBA0B3Ee6B34e14a1B) |
| AlloraConsumer | [`0x7A072465…1d4f`](https://sepolia.mantlescan.xyz/address/0x7A072465AC232709C114C5DAa842a9b7010D1d4f) |
| TradingVault | [`0x3BbD1f5e…6424`](https://sepolia.mantlescan.xyz/address/0x3BbD1f5e8733e901A8FdFf5cFA7E18e575896424) |
| JobManager | [`0x10421Eb1…823c`](https://sepolia.mantlescan.xyz/address/0x10421Eb1A230F484eEdB64642505d073e791823c) |

- **First Allora-on-Mantle submission:** [tx `0x0d7c09…c469`](https://sepolia.mantlescan.xyz/tx/0x0d7c09c945f74595a484b16f185db5c78d175eb286596a881bc78868a6c745b1)
- **First decision anchored:** [tx `0xa052ee03…de69b53`](https://sepolia.mantlescan.xyz/tx/0xa052ee03dca16bbac3b69285e062d6b18349c9809b99e5b186d4f2e91de69b53) (MNT · ABSTAIN · block 39,402,623)

---

## Tech stack

| Layer | Tech |
|---|---|
| Intelligence | [Allora](https://allora.network) · [Nansen](https://nansen.ai) · [Elfa](https://elfa.ai) · [GoPlus](https://gopluslabs.io) · CoinGecko · Mantlescan (Etherscan v2) |
| ML | XGBoost regime classifier · TimeSeriesSplit CV · 39 features |
| Agents | 7-desk org + PM · BYO multi-provider LLM (OpenAI-compatible) · deterministic tools compute, agents decide |
| Memory | Supabase (Postgres + pgvector) — the decision ledger `decisions_hq` |
| Smart contracts | Foundry · Solidity 0.8.27 · OpenZeppelin v5 · ERC-8004 · EIP-712 |
| Frontend | Next.js 16 · Tailwind v4 · RainbowKit · Wagmi v2 · viem |
| Relayer | Python · web3.py · Allora SDK |

---

## Honest disclosures

- The headline isn't a return — it's **rigor.** No all-weather alpha exists here, and we say so out loud.
- The **regime classifier (82.6% OOS) is real but did *not* become simulated trading profit** → the engine allocates capital + sizes risk; it does not predict price.
- The **OI-contrarian** edge validated strongly on its original MNT window (+28.9% OOS, bear-amplified) but **decayed to −7.2% OOS on fresh full-history data** → our self-learning loop **demoted it; the validated registry is now empty.** We retired our own flagship rather than keep quoting a stale number — the discipline that retires a decayed edge **is** the product.
- We **retracted our own earlier headline numbers** when they failed full-history testing, and we **rejected a +96% backtest** that was a thin-liquidity artifact.
- Sessions frequently fire **0 trades** (sideways MNT / no validated edge) — a correct, disciplined **ABSTAIN**, not a bug.
