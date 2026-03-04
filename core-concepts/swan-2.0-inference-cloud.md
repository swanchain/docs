---
description: Decentralized AI Inference Marketplace — Swan Chain's Next Evolution
---

# Swan 2.0: Inference Cloud

## Overview

Swan 2.0 marks Swan Chain's evolution from a UBI-subsidized computing network into a **market-driven AI inference marketplace**. Built as the Inference Cloud, it connects AI model consumers with GPU providers through a decentralized coordination layer, enabling anyone to access AI models via a single API key — or earn stablecoin revenue by sharing their GPU resources.

{% hint style="info" %}
**Try Swan Inference**: [https://inference.swanchain.io](https://inference.swanchain.io)

Swan 2.0 replaces UBI with inference revenue per [SIP-003](https://github.com/swanchain/governance/discussions/21). UBI tapers to zero over 3 months. See also [SIP-002](https://github.com/swanchain/governance/discussions/16) for the original transition proposal.
{% endhint %}

## What Changes in Swan 2.0

| Aspect | Swan 1.0 (UBI Model) | Swan 2.0 (Inference Cloud) |
|--------|----------------------|---------------------------|
| **Provider Rewards** | Flat UBI sampling | 95% inference revenue (stablecoins) |
| **Payment** | SWAN tokens only | Stablecoins (USDC/USDT) + Pay-with-SWAN (20% discount) |
| **Provider Roles** | Separate ECP and FCP | Unified Computing Provider (CP) |
| **Collateral** | SWAN tokens only | SWAN tokens |
| **Work Verification** | Random sampling tasks | Periodic benchmarks (math, code, latency) |
| **Workloads** | Training, ZK proofs | AI inference (LLM, image, audio, embedding, multimodal) |
| **Hardware Requirements** | None (any GPU earned UBI) | Tiered: min 8GB VRAM, legacy GPUs rejected |

## Architecture

Swan Inference uses a hub-and-spoke architecture where the central platform coordinates between consumers and providers:

```
┌─────────────────────────────────────────────────────────────────┐
│                    External Consumers                            │
│              (Meganova AI, LiteLLM, Direct API)                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      SWAN INFERENCE                              │
│  ┌─────────────────┐          ┌─────────────────────────────┐   │
│  │  REST API        │          │  WebSocket Hub              │   │
│  │  (Port 8100)     │─────────▶│  (Port 8081)                │   │
│  │                  │          │  - Provider connections      │   │
│  │  /api/v1/*       │          │  - Inference dispatch        │   │
│  │  /v1/* (OpenAI)  │          │  - Load balancing            │   │
│  │                  │          │  - Health monitoring          │   │
│  └─────────────────┘          └─────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                    │                         │
                    ▼ WebSocket               ▼ HTTP (fallback)
┌───────────────────────────────┐  ┌─────────────────────────────┐
│      GPU Providers            │  │   External Endpoints         │
│  (H100, A100, 4090, 3090)    │  │   (vLLM, TGI, OpenAI API)   │
└───────────────────────────────┘  └─────────────────────────────┘
```

**Request Flow:**

1. Consumer sends a request to the REST API
2. The Hub checks for available WebSocket providers
3. If WebSocket providers are available: route via WebSocket (primary path)
4. If no WebSocket providers: route to External Endpoints (fallback)
5. Response includes `X-Swan-Connection-Mode` header indicating which route was used

### Key Components

| Component | Role |
|-----------|------|
| **REST API** | OpenAI-compatible consumer-facing API |
| **WebSocket Hub** | Real-time provider connections, inference dispatch, load balancing |
| **Marketplace Service** | Model catalog, search, pricing |
| **Payment Ledger** | Off-chain usage tracking, invoicing |
| **Benchmark Sampler** | Periodic quality checks and scoring |
| **Settlement Batcher** | On-chain settlement via MerkleDistributor |

## OpenAI-Compatible API

Swan Inference provides a drop-in replacement for OpenAI's API. Any existing OpenAI SDK or integration works with Swan Inference by changing the base URL and API key.

### Supported Endpoints

| Endpoint | Description |
|----------|-------------|
| `/v1/chat/completions` | Chat-based text generation (streaming supported) |
| `/v1/embeddings` | Text embeddings |
| `/v1/images/generations` | Image generation |
| `/v1/audio/transcriptions` | Audio-to-text transcription |
| `/v1/models` | List available models |

### Example Request

```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-r1-distill-llama-70b",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

## Tiered Model Catalog

Swan Inference organizes models into hardware-based tiers with per-token pricing set at 50–66% below comparable centralized providers.

### Tier S: Premium (70B)

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Sapphira L3.3-70B | 70B | $0.20 | $0.30 | 38GB |
| Nevoria 70B | 70B | $0.20 | $0.30 | 38GB |
| Euryale v2.3 70B | 70B | $0.20 | $0.30 | 38GB |

### Tier A: Agent & Advanced (24B–32B)

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Qwen3 32B | 32B | $0.08 | $0.12 | 18GB |
| Devstral 24B | 24B | $0.06 | $0.10 | 14GB |
| Mistral Small 3.2 24B | 24B | $0.06 | $0.10 | 14GB |

### Tier B: Free & Growth (8B–12B)

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Violet Lotus 12B | 12B | $0.02 | $0.04 | 7GB |
| Stheno 8B | 8B | $0.01 | $0.03 | 5GB |
| Qwen3 8B | 8B | $0.01 | $0.03 | 5GB |
| Llama 4 Scout 17B | 17B MoE | $0.03 | $0.05 | 12GB |

### Tier C: Utility Models

| Model | Type | Pricing | Unit | Min VRAM |
|-------|------|---------|------|----------|
| Whisper Large v3 | Audio | $0.003 | per minute | 4GB |
| BGE / E5 Large | Embedding | $0.005 | per M tokens | 2GB |
| FLUX.1 Schnell | Image Gen | $0.003 | per image | 12GB |
| SDXL | Image Gen | $0.002 | per image | 8GB |

Model availability is shown in real time at [inference.swanchain.io/models](https://inference.swanchain.io/models).

### Free Tier

| Parameter | Specification |
|-----------|--------------|
| Available Models | Stheno 8B, Qwen3 8B (Tier B only) |
| Daily Token Limit | 50,000 tokens (~25 conversations) |
| Rate Limit | 5 requests per minute |
| Concurrency | 1 simultaneous request |
| Registration | API key only (free, no KYC) |
| Upgrade Path | USDC top-up or Pay-with-SWAN |

## Payment & Revenue Split

### Dual Payment Options

Swan 2.0 accepts both stablecoins and SWAN as payment. Users who pay with SWAN receive a **20% discount**, creating organic buy pressure.

| Payment Method | Discount | Example (1M Tier B output tokens) |
|---------------|----------|-----------------------------------|
| USDC/USDT | 0% (base price) | $0.03 |
| SWAN | 20% discount | $0.024 (saves $0.006) |

**Pay-with-SWAN flow:**

1. Consumer sends an inference request with `payment: "SWAN"`
2. SWAN amount is calculated from base USD price minus 20% discount
3. SWAN is deducted from the user's prepaid balance
4. Provider receives 95% in their preferred currency (USDC or SWAN)
5. 5% goes to the Growth Fund

### Provider-First Revenue Split (95/5)

During the bootstrap phase, Swan adopts an aggressive provider-first model to attract quality hardware:

| Recipient | Share | Purpose |
|-----------|-------|---------|
| **Computing Provider** | 95% | Direct payout in stablecoins (USDC/USDT) |
| **Growth Fund** | 5% | Provider recruitment, integrations, dev tooling, liquidity |
| Protocol Treasury | 0% | Deferred until network reaches sustainability threshold |

The Growth Fund is reinvested into network expansion — provider onboarding bounties, DEX liquidity seeding, and integration grants. Spending is reported monthly with transaction hashes.

### Dynamic Revenue Split Schedule

As network revenue grows, the split adjusts through governance votes:

| Phase | Daily Revenue | Provider | Growth Fund | Treasury |
|-------|--------------|----------|-------------|----------|
| **Bootstrap** | < $100 | 95% | 5% | 0% |
| Growth | $100 – $1,000 | 90% | 5% | 5% |
| Maturity | $1,000 – $10,000 | 85% | 3% + 2% burn | 10% |
| Scale | > $10,000 | 80% | 2% + 3% burn | 15% |

Each phase transition requires a governance vote with a 7-day voting period. Revenue thresholds are measured as a 30-day rolling average.

## Quality Assurance

### Benchmarks

The benchmark worker runs periodically (default: every 24 hours) to verify provider quality:

| Test | Pass Threshold |
|------|---------------|
| **Math Accuracy** | ≥ 50% |
| **Code Generation** | ≥ 50% |
| **Response Latency** | ≤ 5000ms |

### Slashing Conditions

| Condition | Consequence |
|-----------|------------|
| Benchmark failure (1st) | Warning + 24h suspension from task routing |
| Consecutive failure (2nd) | 10% collateral slashed |
| Consecutive failure (3rd) | 30% collateral slashed + network removal |
| Inference success rate < 80% | Deprioritized in request routing |
| Uptime < 90% (30-day rolling) | Deprioritized in request routing |

### Health Monitoring

- **Automatic health checks** with configurable thresholds for WebSocket and external endpoints
- **Circuit breaker** to prevent cascading failures
- **Load balancing** with health-aware routing (round-robin, least-connections, or health-aware strategies)
- **Model warmup** to pre-load models and reduce cold-start latency

### Provider Leaderboard

Providers are ranked by a performance-based leaderboard using availability metrics, success rates, and latency.

## Provider Onboarding

### Hardware Tiers

To receive inference traffic and earn revenue, providers must meet minimum hardware requirements:

| Tier | Min VRAM | Example GPUs | Models | Status |
|------|----------|-------------|--------|--------|
| **S** | 38GB+ | L40S, A100, H100 | 70B premium models | Recruiting new providers |
| **A** | 24GB | RTX 4090, 3090, A6000 | 24B–32B agent models | Activate idle inventory |
| **B** | 12GB | RTX 4070 Ti, 3080 Ti | 8B–12B free tier | Some current providers |
| **C** | 8GB | RTX 3070, 4060 | Embedding, Whisper | Lowest qualifying tier |
| Rejected | < 8GB or legacy | TESLA P4, GTX 1050 Ti | None | No rewards |

Legacy GPUs (TESLA P4, GTX 1050 Ti) served the network well during the ZK-task era but cannot serve AI inference workloads at acceptable quality.

### Requirements

- GPU meeting at least Tier C requirements (≥ 8GB VRAM)
- Swan's `computing-provider` agent installed
- No public IP, domain, or SSL setup required — providers connect via WebSocket behind NAT/firewall
- Pass initial inference benchmark (math, code, latency)
- Maintain > 50% uptime over a trailing 7-day window

### Registration Flow

1. Sign up at the Swan Inference dashboard and get a provider API key (`sk-prov-*`)
2. Install the `computing-provider` agent on your GPU machine
3. Deposit collateral (SWAN tokens)
4. Pass the initial benchmark verification
5. Begin receiving inference requests and earning stablecoin revenue

### Collateral

Providers must deposit SWAN token collateral to become active on the network. See [Computing Provider Collateral](token/computing-provider-collateral/) for details on collateral amounts and the 7-day refund waiting period.

## On-Chain Settlement

Swan Inference uses a **MerkleDistributor** smart contract for gas-efficient batch settlement:

1. The platform aggregates provider earnings into daily settlement batches
2. A Merkle tree is computed from all provider balances
3. The Merkle root is submitted on-chain
4. Providers claim their earnings by submitting a Merkle proof

This approach minimizes gas costs by settling many provider payments in a single on-chain transaction.

### Smart Contracts

| Contract | Address (Swan Chain Mainnet) |
|----------|----------------------------|
| **ProviderCollateral** | `0x557f306f917009cf83c32b8b32a79202e79948e5` |
| **SWAN Token** | `0xAF90ac6428775E1Be06BAFA932c2d80119a7bd02` |

{% hint style="info" %}
Swan Chain Mainnet operates on Chain ID **254** with RPC at `https://mainnet-rpc01.swanchain.io`. See [Network Info](../network-reference/readme/) for full details.
{% endhint %}

## UBI Sunset (SIP-003)

Swan 2.0 eliminates UBI entirely per [SIP-003](https://github.com/swanchain/governance/discussions/21). Providers earn solely from inference revenue (95% of fees). The taper schedule:

| Period | UBI Level | SWAN/day | Notes |
|--------|-----------|----------|-------|
| Swan 2.0 Launch (Mar 16 – Apr 9) | 100% | 58,369 | Platform goes live, governance vote Apr 1–7 |
| Month 1 post-vote (Apr 10 – May 9) | 50% | 29,185 | Contribution-weighted; legacy hardware earns zero |
| Month 2 post-vote (May 10 – Jun 9) | 20% | 11,674 | Providers earn primarily from inference |
| Month 3+ (Jun 10 onwards) | **0%** | 0 | UBI permanently off. Inference revenue only. |

### Why Stop UBI

Under Swan 1.0, 75% of daily UBI went to providers with 0% uptime. SIP-003 redirects all incentives toward GPUs that actually serve inference. The breakeven where inference revenue matches the best current UBI payout is just **$25/day total network revenue**.

### Safety Valve

If the network cannot sustain minimum viable provider economics ($50/day revenue) by Month 3, governance can vote to extend UBI at 25% (contribution-weighted only) for 3 additional months.

## SWAN Token Utility

After UBI stops, SWAN token utility is:

| Utility | Description |
|---------|-------------|
| **Pay-with-SWAN** | 20% inference discount for consumers — creates organic buy pressure |
| **Provider Collateral** | Required deposit to join the network |
| **Governance** | Vote on protocol parameters, revenue splits, and phase transitions |

## Learn More

- **[Inference Marketplace](market-provider/inference-marketplace.md)** — How the marketplace works: pricing, routing, and settlement
- **[Computing Provider Income](token/swan-provider-income.md)** — Contribution score formula and reward distribution
- **[Computing Provider Collateral](token/computing-provider-collateral/)** — Collateral requirements and slashing
- **[SIP-001: FCP Subsidy Program](https://github.com/swanchain/governance/discussions/11)** — Stage 1 funding for computing providers
- **[SIP-002: Unified CP & Contribution Rewards](https://github.com/swanchain/governance/discussions/16)** — Original transition proposal
- **[SIP-003: Inference Cloud Economics](https://github.com/swanchain/governance/discussions/21)** — Full model catalog, 95/5 revenue split, UBI sunset, Pay-with-SWAN
