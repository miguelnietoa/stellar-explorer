# SCF Submission — Soroban-first Block Explorer

## One-liner

Soroban-first explorer with human-readable transactions, unified Classic and Soroban token tracking, and rich smart contract visibility.

---

## Problem

While Stellar block explorers have strong support for classic assets, the ecosystem currently lacks an explorer that displays Soroban events and transactions legibly. As Soroban enables DeFi, collectibles trading, and other emergent use cases, not having a way to clearly see what happened in a smart contract transaction dampens growth. Users engaging with new apps — swapping tokens on Soroswap, lending on Blend, providing liquidity on Aquarius — cannot easily verify what their transactions did. This limits innovation and erodes trust in what's happening on-chain.

Our explorer addresses this by displaying both Soroban and classic operations in human-readable, context-rich formats — showing not just "invoke_host_function" but "Swapped 100 USDC for 50 XLM at Soroswap."

---

## Audience

1. **Soroban developers** — Building and debugging smart contracts. Need to inspect contract invocations, verify emitted events, browse storage state, and test interactions without writing custom scripts.

2. **DeFi and dApp users** — Engaging with the growing Soroban ecosystem (Soroswap, Blend, Aquarius, FxDAO). Need to verify swaps, deposits, borrows, and other transactions in a format they can understand.

3. **Auditors and security researchers** — Reviewing on-chain activity. Need to trace contract call chains, inspect authorization trees, and review state changes to ensure contracts behave as expected.

4. **Ecosystem builders** — Wallets, dashboards, and analytics tools that need a reliable, indexed API to query Soroban data for their own products.

---

## Market

**Stellar network today:**
- 10.3M+ accounts, ~55M ledgers, ~21.5B lifetime operations
- ~7.9M daily operations, growing steadily with Soroban adoption
- 500K+ deployed smart contracts, ~5M daily contract events

**The gap:** The Stellar ecosystem has excellent block explorers for classic assets, but Soroban support remains limited across the board. As DeFi protocols on Stellar mature and attract more users, the need for a Soroban-first explorer becomes more urgent. Every developer deploying a contract, every user making a swap, and every auditor reviewing a protocol needs clear visibility into what Soroban transactions do.

**The opportunity:** Block explorers are essential public infrastructure. Stellar's smart contract platform has reached production maturity, with active protocols and growing daily usage. The ecosystem needs explorer infrastructure that matches this maturity — purpose-built for Soroban.

---

## Solution

We are building a block explorer that displays both classic asset and Soroban token transactions in a clean, human-readable format. The explorer is powered by a custom data pipeline that ingests, indexes, and serves the full history of Stellar on-chain activity with a focus on Soroban.

### Core capabilities

**Human-readable transactions** — Every transaction is displayed in plain language. Classic operations like payments and offers are described clearly, and Soroban invocations show the function called, the arguments passed, and the outcome — e.g., "Address C...AAA swapped 1 USDC to 1 XLM at Soroswap."

**CAP-67 unified event stream** — Full support for the unified event stream introduced in Protocol v23, displaying all token movements (classic and Soroban) in a single view: transfers, mints, burns, and clawbacks. Swap heuristics detect when events represent a swap based on the function name and the presence of corresponding swap events.

**SEP-41 and SEP-50 display** — Reference implementation for displaying SEP-41 fungible tokens (name, symbol, decimals, holders, transfer history) and SEP-50 NFTs (collection metadata, individual token rendering, ownership history).

**Soroban operations display** — Contract invocations show both the technical detail ("address C...AAA called `swap`") and the human-readable summary ("Address C...AAA swapped 1 USDC to 1 XLM at Soroswap"), with decoded arguments, authorization chains, and resource consumption.

**Smart contract function filtering** — Users can filter events and transactions by specific contract function calls directly in the UI (e.g., show only `swap` or `deposit` calls for a given contract).

**SEP-39, SAC, and Classic Asset support** — A unified asset resolution pipeline that bridges classic Stellar assets, Stellar Asset Contracts (SAC), and Soroban-native tokens into a coherent display.

**Complete Soroban history** — Full transaction history from 2024 onward, with support for previous protocol versions (v20+), powered by historical backfill from the public RPC Data Lake.

### Technical architecture

**Ingestion Pipeline (Go)** — Reads ledger data from Stellar RPC providers and the public Data Lake (S3), parses XDR using the Stellar Ingest SDK, extracts operations, effects, and events, and writes to our database. Processes CAP-67 unified token events via the Token Transfer Processor. Detects and classifies contracts (SEP-41, SEP-50) by parsing WASM contract specs.

**Database + Search** — PostgreSQL with TimescaleDB for time-series partitioning and compression. Typesense for sub-50ms fuzzy search across assets, contracts, and known accounts. Redis for caching and real-time event distribution.

**API Layer** — REST API with OpenAPI documentation serving ledgers, transactions, accounts, contracts, assets, and search. Cursor-based pagination, tiered caching, and rate limiting.

**Frontend (Next.js + React 19)** — A modern, responsive explorer built on our existing open-source codebase with 9-language internationalization, multi-network support (public, testnet, futurenet), and dark/light mode.

---

## Why Now

1. **CAP-67 (Protocol v23)** — The unified event stream makes it possible for the first time to display all token movements — classic and Soroban — through a single data pipeline. This is the foundation for a truly unified explorer experience, and it didn't exist before.

2. **Soroban DeFi has reached production maturity** — Soroswap, Blend, Aquarius, and FxDAO are live with real users and real volume. The need for legible Soroban transaction display is no longer theoretical — it's a daily pain point for a growing user base.

3. **RPC Data Lake availability** — SDF published the full historical ledger archive on public S3 (~3.8TB), enabling complete indexing without running Captive Core. This dramatically reduces the infrastructure barrier for building a full-featured explorer.

4. **Ecosystem direction toward RPC and custom indexers** — SDF is steering the ecosystem away from Horizon toward RPC and purpose-built data pipelines. Building an explorer with its own index aligns with this direction and future-proofs the tool.

5. **The ecosystem is asking for this** — SCF delegates identified the gap in Soroban explorer support and published this RFP. The timing is right to deliver.

---

## Go-to-Market

1. **Stellar developer docs** — Get listed on the official [Block Explorers](https://developers.stellar.org/docs/tools/developer-tools/block-explorers) page as a recommended Soroban-first explorer.

2. **Stellar Ambassador program** — The team are active Stellar Ambassadors. We will present the explorer in community calls, regional meetups, and Ambassador-led events to drive awareness and adoption. This gives us a direct channel to introduce the explorer to new projects building on Stellar, positioning it as the go-to reference for transaction transparency and on-chain verification.

3. **Open source community** — Enable community contributions, forks, and integrations, expanding reach organically.


---

## Traction & Evidence

- **Live open-source explorer:** [Stellar Explorer](https://stellar-explorer.acachete.xyz/) — built with Next.js 16, React 19, multi-network support (public/testnet/futurenet), 9-language i18n, dark/light mode, SSE streaming, and a dual Horizon + RPC client layer.

- **Early usage (Vercel Web Analytics, Jan 30 – Mar 1, 2026):** 94 visitors, 549 page views, 54% bounce rate.


---

## SCF Track Choice

**RFP Track** — responding to the **"Soroban-first Block Explorer"** RFP (Q1 2026).

This track is the right fit because the RFP directly describes the problem we've been working to solve. Our submission addresses each requirement:

| RFP Requirement | How We Address It |
|-----------------|-------------------|
| SEP-41 and SEP-50 transaction handling | Contract spec parsing detects token standards, displays metadata, and formats amounts using on-chain decimals |
| Human-readable transactions | Humanization engine for all 27 classic operations + Soroban invocations with DeFi pattern matching |
| CAP-67 event handling + unified stream | Token Transfer Processor integration, unified token event index, swap heuristics based on function name and event presence |
| Soroban operations display | Decoded invocations showing function name, typed arguments, authorization chain, and resource consumption |
| Filtering by smart contract fn call | Indexed contract events with compound filter UI for function names, event types, and time ranges |
| SEP-39, SAC, and Classic Asset support | Unified asset resolution pipeline bridging classic assets, SAC, and Soroban-native tokens |
| Complete Soroban history (2024+, v20+) | Historical backfill via the public RPC Data Lake, with protocol-versioned XDR parsing |
| Open source | Apache-2.0 license |
| <400ms API responses under parallel load | Multi-layer caching, read replicas, connection pooling, verified with k6 load testing |

---

## Funds — What the Grant Unlocks

The grant enables building the full data infrastructure that makes a Soroban-first explorer possible: a custom indexer that processes the entire Stellar ledger history, a caching layer, a public API, and the humanization engine that turns raw operations into readable transactions. The grant covers the engineering time to build it to production quality, the infrastructure costs during development, and the time to achieve and verify the <400ms performance target under parallel load.
