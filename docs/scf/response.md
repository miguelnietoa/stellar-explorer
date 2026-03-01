## Research Complete

I've created three comprehensive architecture documents in docs/scf/:

1. docs/scf/architecture.md (1,078 lines) — Main Architecture Document

  The master document covering:
  - RFP requirements mapping — every requirement traced to a specific architecture component
  - System architecture overview — full component diagram with responsibilities
  - Data ingestion pipeline — Go-based pipeline using Stellar Ingest SDK + RPC + Data Lake
  - Database architecture — PostgreSQL + TimescaleDB (2 TB compressed for full history)
  - Search architecture — Typesense for fuzzy search + deterministic routing for 80%+ of exact-match queries
  - API layer — REST + OpenAPI + SSE streaming via Redis pub/sub
  - Frontend architecture — builds on existing Next.js 16 stack; details the Contract Studio (Read/Write Contract, Etherscan-style)
  - Soroban-specific features — transaction humanization engine, SEP-41/SEP-50 display, swap heuristics, contract invocation display
  - Protocol standards — CAP-67 unified events, CAP-46 smart contracts, asset resolution pipeline
  - Performance engineering — <400ms latency budget breakdown, caching strategy, load testing plan
  - Deployment & infrastructure — Kubernetes topology, ~$2,850/month estimated cost
  - Development roadmap — 10-week plan split into two tranches
  - Competitive differentiation — feature comparison against StellarExpert, Steexp, StellarChain

  2. docs/scf/database-schema.md (733 lines) — Complete Database Schema

  Full SQL definitions for 17 tables including:
  - TimescaleDB hypertables for time-series data (ledgers, transactions, operations, effects, contract_events, token_events, trades)
  - State tables for current data (accounts, trustlines, contracts, contract_storage, assets, offers)
  - Supporting tables (known_accounts, liquidity_pools, network_stats, ingestion_state)
  - Index strategy mapped to every primary access pattern
  - Compression policies (~90% storage reduction)
  - Data volume estimates and retention policies

  3. docs/scf/data-ingestion.md (465 lines) — Data Ingestion Deep Dive

  Detailed guide on:
  - All 5 data sources (RPC, Data Lake, Horizon SSE, Hubble BigQuery, Ingest SDK) with capabilities and limitations
  - Pipeline component design — Source Manager → XDR Parser → Transformer → Writer → State Manager
  - Per-ledger processing flow — step-by-step breakdown of how each ledger is ingested
  - Live mode vs Backfill mode (8 parallel workers, ~2 hours for full Soroban history)
  - Contract spec processing — WASM custom section parsing, SEP-41/50 classification
  - Soroban event decoding — ScVal type handling for topics and values
  - Backfill options comparison — Data Lake (recommended) vs Hubble vs Captive Core
  - Monitoring, alerting, and disaster recovery

  Key Architectural Decisions

  ┌────────────────────┬───────────────────────────────────┬─────────────────────────────────────────────────────────────────┐
  │      Decision      │              Choice               │                               Why                               │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Ingestion language │ Go                                │ Native Stellar Ingest SDK, no serialization overhead            │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Primary DB         │ PostgreSQL + TimescaleDB          │ Proven (Horizon, Blockscout), ACID, automatic time-partitioning │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Historical data    │ RPC Data Lake (S3)                │ Free, ~2 hours backfill, full control                           │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Search             │ Typesense + deterministic routing │ Sub-50ms fuzzy search; 80%+ of queries skip search engine       │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Cache              │ Redis                             │ Hot data, rate limiting, SSE pub/sub                            │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Frontend           │ Keep existing Next.js 16          │ Already built, just extend with new pages                       │
  ├────────────────────┼───────────────────────────────────┼─────────────────────────────────────────────────────────────────┤
  │ Killer feature     │ Contract Studio (Read/Write)      │ No Stellar explorer has this; Etherscan's most powerful feature │
  └────────────────────┴───────────────────────────────────┴─────────────────────────────────────────────────────────────────┘
