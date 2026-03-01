# RFP Track

#### ✅ Who This Track Is For:

* Devs and teams building tools, SDKs, APIs, explorers, or testing infra
* Experienced builders solving problems for other developers

#### 🚫 Who This Track Is Not For

* Builders without much prior experience in domain or the Stellar ecosystem

  → Better fit for [Instawards](https://stellar.gitbook.io/scf-handbook/scf-awards/instawards)
* Teams building apps, protocols, or integrations for end users\
  → Consider either the [Open Track ](https://stellar.gitbook.io/scf-handbook/scf-awards/build-award/open-track)or [Integration Track](https://stellar.gitbook.io/scf-handbook/scf-awards/build-award/integration-track), depending on your focus
* Teams proposing a tooling idea not aligned with an [active RFP](#current-open-rfps)\
  →  Wait for a future RFP that matches your concept

#### 📋 Requirements

* Your submission must address an [open RFP](#current-open-rfps) from the current quarter—read the RFP carefully and respond directly to its needs.
* You must clearly show:
  * Why you’re a good fit to solve this (provide examples of past dev-focused work if possible)
  * What makes your solution technically strong
  * Clear, testable milestones&#x20;
  * How your tool will be maintained post-launch

#### Current Open RFPs

RFPs are sourced from ideas submitted by the Stellar ecosystem, selected by Delegates through the [SCF Quarterly Process](https://stellar.gitbook.io/scf-handbook/scf-awards/build-award/quarterly-governance-process), and published here at the start of each quarter:

<details>

<summary><strong>Prices API</strong></summary>

### Prices API

*Added: Q1 2026*

#### **1. Scope of Work**

Develop a publicly accessible API and indexing service to provide normalized price data.

#### **2. Background & Context**

The Stellar ecosystem lacks a single, reliable, and standardized API to source aggregated, real-time, and historical price data for all Stellar assets (both classic and SEP-41 Soroban contract tokens). This fragmentation hinders the development of accurate DeFi protocols, portfolio tracking tools, and general-purpose applications, increasing integration complexity for builders.

#### **3. Requirements**

* Asset Coverage: Support for all current native Stellar assets and SEP-41 Soroban contract tokens.&#x20;
* Oracle Coverage: Support for oracles, including but not limited to prices streamed from Chainlink, Redstone, Band, Reflector, and others.
* Price Aggregation: Calculate a weighted average price across major on-chain and off-chain markets (e.g., Soroswap, Aquarius, SDEX, Blend, etc.).&#x20;
* Adjustable Volume Threshold: At least one version should allow prices to be weighted by trading volume (VWAP), requiring a configurable USD-denominated trading volume threshold for market inclusion to filter out illiquid pairs.
* Data Endpoints: Real-time price endpoints, and historical price endpoints for different intervals (e.g., 24h, 7d, 30d, 1yr) for charting price changes over time.&#x20;
* Base and quote volume in USD.
* OHLC: Historical price endpoints should offer OHLC aggregates to support candlestick charts and their use cases.
* Supported timeframes: 1 hour, 24 hours, 1 week, 1 month, 1 year, and All-Time (since asset inception)
* Suggested granularity: 1 min, 15 min, 1 hour, 4 hours, 1 day, 1 week, 1 month
* Granularities can be matched to timeframes and finer granularities (1 min to 4 hours) do not have to be stored forever. The granularities of 1 hour and above must remain available indefinitely for the All-Time view.
* Performance: Must be highly available, low-latency, and capable of handling high query volume. Teams must clearly explain what happens when prices become unavailable, sources start to differ, etc.
* Repo needs to be completely Open Source (Tranche I and II)

**Asset Metadata**

| Field            | Type           | Source      | Notes                          |
| ---------------- | -------------- | ----------- | ------------------------------ |
| Asset/Token Code | string         | On-chain    | Asset name                     |
| Current Price    | float (USD)    | Pricing API | Real-time or near-real-time    |
| Asset Type       | enum           | On-chain    | classic \| soroban             |
| Issuer Address   | string         | On-chain    | Issuer public key (G...)       |
| Contract Address | string         | On-chain    | Issuer contract address (C...) |
| Home Domain      | string \| null | On-chain    | Classic assets only; nullable  |

**Historical Price Chart**

| Timeframe       | Granularity (suggested) | Data Points | Price Type   |
| --------------- | ----------------------- | ----------- | ------------ |
| 1 hour          | 1 min                   | \~60        | TWAP or VWAP |
| 24 hours        | 15 min                  | \~96        | TWAP or VWAP |
| 1 week          | 1 hr                    | \~168       | TWAP or VWAP |
| 1 month         | 4 hr                    | \~180       | TWAP or VWAP |
| Since Inception | 1 day                   | Variable    | TWAP or VWAP |

#### **4. Evaluation Criteria**

* Technical design quality
* Relevant experience in building performant, low latency APIs
* Security: Resistant to relevant attack vectors
* Ecosystem alignment: Proactive in understanding and indexing prices for classic and Soroban assets
* Ability to deliver within 2 months
* Coherent integration plan

#### **5. Expected Deliverables**

* Ready to use API production deployment within \~10 weeks from first award payment.&#x20;
* API reference documentation
* Self-service onboarding experience
* Examples:
  * <https://stellar.expert>
  * <https://steexp.com/>&#x20;

</details>

<details>

<summary><strong>DeFi Positions API</strong></summary>

### DeFi Positions API

*Added: Q1 2026*

#### 1. Scope of Work

Develop an aggregation service and API to normalize and display a user's total DeFi exposure across multiple, supported Stellar protocols.

#### 2. Background & Context

As the DeFi sector of Stellar matures with protocols like Blend, Aquarius, and others, users and developers face significant complexity in tracking and displaying a user's full, aggregated position (deposits, borrowings, yield) across multiple protocols. This creates a poor user experience and limits the ability to build advanced analytics or personal finance dashboards.

#### 3. Requirements

* Protocol Support: Initial support for the top 3-5 DeFi protocols by TVL (e.g., Blend, Aquarius, Soroswap, FxDAO).&#x20;
* Protocol Semantics: Must handle protocol-specific logic, such as Blend's backstop pools or different collateral/interest models, ensuring accuracy for each position type. &#x20;
* Data Points per Position:&#x20;
* Required:
* Total current value
* Total deposit value
* Total borrowed value (if applicable)
* Total current return (if applicable)
* Health Value/factor (if applicable)
* Optional but preferred:
* Position value history
* User Input: Accepts a user's G-address or C-address to query all associated DeFi positions.
* Performance: Must be highly available, low-latency, and capable of handling high query volume. Teams must clearly explain what happens when position data become unavailable, sources start to differ, etc.

#### 4. Evaluation Criteria

* Technical design quality
* Coherent project plan
* Team Technical capability
* Relevant experience of building similar products in the Stellar ecosystem and others
* Security & audit history
* Ability to deliver within required timeline

#### 5. Expected Deliverables

* Ready to use API production deployment within \~10 weeks from first award payment.&#x20;
* API reference documentation
* Self-service onboarding experience
* Examples:
  * <https://stellar.broker/>
  * <https://docs.soroswap.finance/soroswap-api>

</details>

<details>

<summary><strong>C-Address Tooling &#x26; Onboarding</strong></summary>

### C-Address Tooling & Onboarding

*Added: Q1 2026*

#### 1. Scope of Work

Well-researched approach with reference/example implementation with onboarding flow on how to easily fund a C-address (Soroban Smart Account) without first using a traditional G-address.

#### 2. Background & Context

The shift to C-addresses (Soroban smart accounts) is critical for next-generation dApps on Stellar, but two major adoption blockers persist: 1) the inability to easily fund a C-address without first using a traditional G-address, and a2) a lack of core, modern mobile tooling around the [Smart Account standard by OpenZeppelin](https://docs.openzeppelin.com/stellar-contracts/accounts/smart-account). This technical friction limits the utility of C-addresses for end-users and exchanges.

#### 3. Requirements

* C-Address Onboarding/Funding Solution with G-to-C Seamless Bridge: A protocol or service that enables the direct funding of a C-address using G-addresses, CEX withdrawals, and  possibly off-ramp solutions (e.g., a proxy service routing from credit card to G-address to C-address). The goal is to make the G-address step transparent or unnecessary for the end-user.
* Viable C-Address Wallet: A reference implementation of a production level wallet to showcase the tooling and standard at parity with the [Freighter wallet](https://www.freighter.app/), including support showing all tokens held, history of transfers.&#x20;
* Onboarding Kit/UX Flow: Develop a standard, open-source "onboarding kit" that creates a standard user experience flow for wallet providers/Stellar wallets. This kit should encompass:
* A seamless flow for C-address creation, asset bridging, adding assets, and funding with XLM for fees.
* Easy integrations with existing bridges when selecting a Stellar wallet.
* An onboarding flow that supports funding a C-address from a standard G-address or CEX withdrawal.
* Wishlist Item: The onboarding flow should allow users to sign in with other ecosystem wallets (e.g., Metamask, Phantom, Rabby, etc.), derive a new address, and then proceed through the funding flow, potentially funding their account/adding assets based on policies like address activity.
* Associated tooling for building on and with C-address wallets as part of the reference implementation on the web and on mobile
* Approach and structure needs to be designed with input from potential users (such as Ecosystem wallets)
* The implementation should be fully Open Source

#### 4. Evaluation Criteria

* High technical capability and proven experience building on both Stellar operations and Soroban smart contracts
* Ecosystem alignment and demonstrated ability and willingness to connect with the existing ecosystem (wallets) as they’d be the potential users of this
* Coherent integration plan and timeline

#### 5. Expected Deliverables

* Smart contracts
* SDKs or libraries
* Documentation
* Test suite
* Audit fixes
* Production-ready version
* Example integrations

</details>

<details>

<summary><strong>Soroban-first Block Explorer</strong></summary>

### Soroban-first Block Explorer

*Added: Q1 2026*

#### 1. Scope of Work

A block explorer that displays both classic asset and Soroban token transactions in a clean, human-readable format.

#### 2. Background & Context

While all Stellar block explorers have good support for classic assets, Stellar block explorers do not have good support for Soroban assets. This dampens growth, because Soroban allows for DeFi, collectibles trading, and other emergent use cases. Not having a block explorer that displays Soroban events and transactions legibly makes it harder for you to engage with new apps, which limits innovation. For that reason, a block explorer that supports displaying Soroban and classic operations in human-readable and context-rich formats will enhance trust in what’s happening on-chain.&#x20;

#### 3. Requirements

* SEP-41 and SEP-50 Transaction handling: A reference implementation of how to display SEP-41 and SEP-50
* Human-Readable Transactions: Block explorers that show exactly what happened in a transaction don’t always show transactions in a human-readable fashion.
* CAP-67 event handling: Full support for CAP-67 events and expose the entire CAP-67 unified event stream. Should rely on heuristics for when to show these as a swap (e.g. based on the function name and the presence of a separate "swap" event).
* Support for Soroban operations (For example, showing both "address C...AAA called \`swap\`", and "Address C...AAA swapped 1 USDC to 1 XLM at Soroswap")
* Add Filtering feature by smart contract fn call (e.g. in the UI, being able to filter by fn\_call.
* Support for SEP-39, SAC, and other Classic Assets:&#x20;
* Support for complete Soroban transactions history (from 2024), support of previous protocol versions (protocol v20+)&#x20;
* Open Source

#### 4. Evaluation Criteria

* Technical capability and relevant experience: Ideally has successfully built and maintained similar block explorer infrastructure in other ecosystems.
* Relevant experience with maintenance of large databases
* Ecosystem alignment: Aligned with Stellar’s mission
* Ability to deliver within required timeline
* Coherent launch plan

#### 5. Expected Deliverables

* Production-ready frontend
* Documentation
* Audit & remediations
* Example integrations
* Load-testing (should provide stable <400ms API responses under parallel load)

</details>

<details>

<summary><strong>Soroban Specialized Reverse Engineering Tool</strong></summary>

### Soroban Specialized Reverse Engineering Tool

*Added: Q1 2026*

#### 1. Scope of Work

Develop a specialized reverse engineering tool that accepts a compiled Soroban Smart Contract (WASM file) and produces a highly accurate and human-readable WebAssembly Text (WAT) representation. The core of the work is to integrate deep knowledge of Soroban's internal structures and logic into the decompilation process to create a superior, more context-aware reconstruction of the contract's functionality.

#### 2. Background & Context

Soroban Smart Contracts are essentially WASM files that include proprietary Soroban-specific internal structures and conventions ("Soroban Magic"). While general WASM decompilers can produce C-like representations, they fail to fully initialize the Soroban internal knowledge, resulting in less accurate and difficult-to-understand WebAssembly Text (WAT) output. This lack of a specialized tool hinders in-depth analysis, security auditing, and comprehensive understanding of complex Soroban contracts, which is a significant blocker for security and development workflows.

#### 3. Requirements

* Decompilation: The tool must be capable of taking a compiled Soroban Contract (WASM file) as input.
* Soroban-Aware Reconstruction: It must incorporate and leverage Soroban internal knowledge during the decompilation process to produce a WAT representation that is significantly more accurate and human-readable than general WASM decompilers.
* Accuracy Target: Define a group of test vectors where the tool should reconstruct the code as close to the source as possible ( at least 90% accuracy in the reconstruction of the decompiled WASM contract through an algorithmic assessment of the AST of the decompiled source vs original source). Check that all custom types, structs, enums, exceptions, and all standard Soroban built-ins (xdr Types, env functions, helpers, standard SDK lib members) are reconstructed as well.
* Should produce RUST code as output
* Open Source

#### 4. Evaluation Criteria

* Technical capability and relevant experience: team should have background with WASM parsing, or vice versa, building WASM compilers for some programming language. Some hands-on experience with formal ANF/BNF programming language grammar specifications would be a plus as well.
* Ecosystem alignment
* Ability to deliver within required timeline
* Coherent integration plan

#### 5. Expected Deliverables

* Smart contracts
* SDKs or libraries
* Documentation
* Test suite
* Production-ready version

</details>

<details>

<summary><strong>Hummingbot integration (open source trading engine)</strong></summary>

### Hummingbot integration (open source trading engine)

*Added: Q1 2026*

#### 1. Scope of Work

Develop a fully integrated Stellar connector for the open-source algorithmic trading framework, Hummingbot. This work should enable users to easily deploy market-making, arbitrage, and other automated trading strategies on the Stellar network.

#### 2. Background & Context

The deprecation of previous community market-making tools, such as Kelp, has created a significant gap in the ecosystem's tooling. This lack of a robust, maintained, and easy-to-use solution for automated trading is resulting in illiquid markets and markets on the Stellar DEX that are extremely out of sync with centralized exchanges (CEXs) during periods of high volatility.

Hummingbot, a mature, open-source Python framework for automated trading, provides a proven solution. By integrating Stellar support into Hummingbot, we can achieve several key outcomes:

* Increased Liquidity: Make it easy and cheaper for new and existing companies, as well as community members who may not be skilled coders, to run professional market-making bots.
* Improved Price Accuracy: Increase the accuracy of prices on Stellar by enabling efficient arbitrage between Stellar and other exchanges.
* Lower Barrier to Entry: Leverage Hummingbot's dedicated maintenance team and existing user base, requiring the Stellar ecosystem to only implement and maintain the Stellar-specific parts.

#### 3. Requirements

* Core Exchange Support: The connector must support interaction with the core Stellar DEX orderbook.
* Order management: submitting orders, cancelling orders, listening to order updates and keeping a local copy of the orderbooks.
* Listen to trades.
* Support multiple Stellar transactions at a time. For example by using channel accounts, [Open Zeppelin relayer](https://docs.openzeppelin.com/relayer) or by batching actions.
* Keep an up-to-date view of the account balance.
* Support any trading pair.
* The connection should ideally use Stellar RPC and not Horizon.
* Official Integration: The completed connector must be added to the official Hummingbot repository to ensure long-term maintenance and discoverability.
* Technical Foundation: The implementation should leverage the existing Ripple (XRPL) integration as a reference for a non-EVM blockchain.
* (Optional) Advanced AMM Support: The solution should optionally include support for:
  * Soroban AMMs (e.g. Aquarius, Soroswap, …)
  * Strategies for intra-Soroban AMM arbitrage
  * Stellar AMMs (classic liquidity pools)

#### 4. Evaluation Criteria

* Technical capability
* Relevant experience: Prior Stellar experience is a big plus. Specifically experience handling Stellar XDR data types and building trading bots.
* Ecosystem alignment: The applicant ideally is aligned with ecosystem values and interested to maintain it as it proves ecosystem value, with support from e.g. the Public Goods Award.
* Ability to deliver within required timeline
* Coherent integration plan

#### 5. Expected Deliverables

* Connector Development: Implement the core logic for connecting Hummingbot to the Stellar network.
* Stellar DEX Orderbook Support: Enable trading strategies that interact directly with the Stellar Decentralized Exchange (DEX) orderbook.
* Official Integration: Ensure the connector meets all requirements for inclusion and long-term maintenance within the official Hummingbot repository.
* Documentation & Examples: Provide comprehensive documentation and example trading strategies (e.g., arbitrage, market-making) specific to the Stellar ecosystem.
* A set of mainnet transactions that demonstrates the bot’s order management activity and an example video of it working
* Test suite
* (Optional) AMM Support: Implement support for current and Soroban-based Automated Market Makers (AMMs), including the ability to perform intra-Soroban AMM arbitrage.

</details>

If you have an need for a tool or infrastructure that would meet an immediate ecosystem need but isn't listed above, it could be a good idea for an SCF RFP — let us know in the [Stellar Dev Discord](https://discord.gg/stellardev)!

#### 📅 Process & Timeline

1. Submit the SCF Interest form and indicate your interest in the RFP Track.

{% hint style="info" %}
Important: If you were referred by a member of the SCF community, make sure to include their unique referral code on this form.&#x20;
{% endhint %}

2. Eligible teams will be invited to submit to an upcoming Build round. Submit your Build form before the deadline and choose the RFP Track. In the submission form, clearly identify which open RFP you’re addressing.
3. Submissions are reviewed by 2 reviewers from that quarter’s Category Delegate Panel.
4. If reviewers agree Yes or No, the project moves forward. If reviewers disagree, a third reviewer is added to break the tie.&#x20;
5. Some teams may receive requested minor changes to their submission before funding.
6. After making any requested changes, awarded submissions receive their first tranche of funding.
