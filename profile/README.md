<div align="center">

# 🌞 HeliQuant

**The all-seeing quant.**

Autonomous multi-source intelligence trading firm on Mantle Network.

</div>

---

## What is HeliQuant?

**HeliQuant** — from *Helios* (the Greek sun god who sees everything from the sky) and *Quant* (quantitative trading discipline) — aggregates four independent signal sources and executes with quant-grade discipline. Built for the **Mantle Turing Test Hackathon 2026**, Track 1 (AI Trading & Strategy).

### Three-layer decision pipeline

```
LAYER 1 — REGIME             ML regime classifier (forward 4h) + lifecycle manager
LAYER 2 — INTELLIGENCE       Allora • Whale flow • Sentiment • Rugpull veto
LAYER 3 — RECONCILIATION     Strategy + source-vote must agree; adaptive sizing
```

### What's validated — and what isn't (honesty first)

| Claim | Status |
|---|---|
| **Regime classifier** | **82.6% out-of-sample accuracy** (91.6% on confident calls) — verified |
| Trend-following alpha | ❌ **none** — fails full-history walk-forward; earlier single-window returns were artifacts (**retracted**) |
| OI-contrarian (positioning) | the **one** edge surviving OOS — hedge-like, **caveated** (bear-amplified, inconsistent fold-to-fold) |
| Accuracy → profit | accuracy is *not* money-weighted profit; the regime engine's job is **capital allocation**, not prediction |

We publish what *doesn't* work too. The org **abstains** when no validated edge applies — a feature, not a failure.

---

## Repositories

| Repo | What |
|---|---|
| **[contracts](https://github.com/HeliQuant/contracts)** | Solidity smart contracts (ERC-8004 + ERC-8183 + AlloraConsumer + TradingVault) deployed + verified on Mantle Sepolia |
| **[agents](https://github.com/HeliQuant/agents)** | Python ML regime classifier + multi-source intelligence engine + 3-layer signal agent |
| **[relayer](https://github.com/HeliQuant/relayer)** | Off-chain Allora → Mantle on-chain prediction bridge (first Allora infra on Mantle) |
| **[frontend](https://github.com/HeliQuant/frontend)** | Next.js dApp — landing + firm detail + ERC-8183 hire flow + live whale watchlist |
| **[docs](https://github.com/HeliQuant/docs)** | Architecture, progress tracker, deployment records |

---

## Live on Mantle Sepolia

All six core contracts deployed + verified on Mantlescan. **HeliQuant ships the first Allora prediction infrastructure ever deployed on Mantle Network** — extending the decentralised AI inference layer to the ecosystem.

| Contract | Address |
|---|---|
| IdentityRegistry | [`0x0fAE6342...C55ff3`](https://sepolia.mantlescan.xyz/address/0x0fAE6342195fdc0007B94Fb3293bF56463C55ff3) |
| ReputationRegistry | [`0x5A18F8D3...B2929`](https://sepolia.mantlescan.xyz/address/0x5A18F8D33D551666233701025754274dCA9B2929) |
| ValidationRegistry | [`0x8e55E41d...4a1B`](https://sepolia.mantlescan.xyz/address/0x8e55E41dc9a93E30aaf580DBA0B3Ee6B34e14a1B) |
| AlloraConsumer | [`0x7A072465...1d4f`](https://sepolia.mantlescan.xyz/address/0x7A072465AC232709C114C5DAa842a9b7010D1d4f) |
| TradingVault | [`0x3BbD1f5e...6424`](https://sepolia.mantlescan.xyz/address/0x3BbD1f5e8733e901A8FdFf5cFA7E18e575896424) |
| JobManager | [`0x10421Eb1...823c`](https://sepolia.mantlescan.xyz/address/0x10421Eb1A230F484eEdB64642505d073e791823c) |

**First Allora-on-Mantle submission**: [tx `0x0d7c09…c469`](https://sepolia.mantlescan.xyz/tx/0x0d7c09c945f74595a484b16f185db5c78d175eb286596a881bc78868a6c745b1)

---

## Tech stack

| Layer | Tech |
|---|---|
| Intelligence | [Allora Network](https://allora.network) · [CoinGecko](https://coingecko.com) · [GoPlus](https://gopluslabs.io) · [GeckoTerminal](https://geckoterminal.com) |
| ML | XGBoost (forward 4h regime classifier) · TimeSeriesSplit CV · 39 features |
| Smart contracts | Foundry + Solidity 0.8.27 · OpenZeppelin v5 · Solady · 23/23 tests passing |
| Standards | ERC-8004 · ERC-8183 · EIP-712 |
| Frontend | Next.js 16 · Tailwind v4 · RainbowKit · Wagmi v2 · viem |
| Relayer | Python · web3.py · Allora SDK |

---

## Honest disclosures

- The headline isn't a return — it's **rigor**. No all-weather alpha exists here, and we say so out loud.
- OI-contrarian's strong aggregate OOS was **amplified by a bear market** and is inconsistent fold-to-fold → deployed as a risk-controlled hedge, never a guarantee.
- Regime accuracy (82.6% OOS) is real but **did not** become simulated trading profit → the engine allocates capital + sizes risk; it does not predict price.
- Sessions frequently fire **0 trades** (e.g. sideways MNT / no validated edge) — a correct, disciplined ABSTAIN, not a bug.
