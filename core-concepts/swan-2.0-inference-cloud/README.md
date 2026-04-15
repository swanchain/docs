---
description: Decentralized AI Inference Marketplace — Swan Chain's Next Evolution
---

# Swan 2.0: Inference Cloud

## Overview

Swan 2.0 marks Swan Chain's evolution from a UBI-subsidized computing network into a **market-driven AI inference marketplace**. Built as the Inference Cloud, it connects AI model consumers with GPU providers through a decentralized coordination layer, enabling anyone to access AI models via a single API key — or earn stablecoin revenue by sharing their GPU resources.

{% hint style="info" %}
**Try Swan Inference**: [https://inference.swanchain.io](https://inference.swanchain.io)

**New here?** See [How to Use Swan Inference](how-to-use.md) for a step-by-step guide, or jump to [live pricing comparison](https://inference.swanchain.io/pricing) against Anthropic, Google, and OpenRouter.

Swan 2.0 replaces UBI with inference revenue per [SIP-003](https://github.com/swanchain/governance/discussions/21). UBI tapers to zero over 3 months. See also [SIP-002](https://github.com/swanchain/governance/discussions/16) for the original transition proposal.
{% endhint %}

## What Changes in Swan 2.0

| Aspect | Swan 1.0 (UBI Model) | Swan 2.0 (Inference Cloud) |
|--------|----------------------|---------------------------|
| **Provider Rewards** | Flat UBI sampling | 95% inference revenue (stablecoins) |
| **Payment** | SWAN tokens only | Stablecoins (USDC/USDT) + Pay-with-SWAN (20% discount) |
| **Provider Roles** | Separate ECP and FCP | Unified Computing Provider (CP) |
| **Collateral** | SWAN tokens only | SWAN tokens or Stripe (credit card) |
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

## Model Catalog

Swan Inference serves two categories of models with different economics. The live catalog at [inference.swanchain.io/models](https://inference.swanchain.io/models) is the source of truth for current availability and pricing.

### Frontier Gateway

Closed frontier models (Claude, Gemini) proxied at a consistent discount to direct API pricing. These run on the upstream provider's infrastructure — Swan acts as an aggregator, passing on cost savings through bulk agreements and SWAN token incentives.

| Example | Direct pricing | Swan pricing | Savings |
|---------|---------------|-------------|---------|
| Claude Opus 4.6 | $5.00 / $25.00 per 1M tokens | $4.00 / $20.00 | ~20% |
| Gemini 2.5 Flash | $0.30 / $2.50 per 1M tokens | $0.18 / $1.50 | ~40% |

With Pay-with-SWAN, the discount stacks to 50–66% below going direct. See the [pricing comparison](https://inference.swanchain.io/pricing) for the live side-by-side.

### Open-Source on Decentralized GPUs

Open-source models served by Swan's decentralized provider network. This is where Swan's economics structurally beat centralized providers — any qualifying GPU owner (datacenter to consumer hardware) can become a provider and share in inference revenue.

| Example | Params | Swan pricing | Hardware tier |
|---------|--------|-------------|--------------|
| Qwen3 Coder 30B | 30B MoE | $0.05 / $0.10 per 1M tokens | A (24GB) |
| GLM 4.7 Flash | — | $0.05 / $0.36 per 1M tokens | A (24GB) |
| Sapphira L3.3 70B | 70B | $0.20 / $0.30 per 1M tokens | S (38GB+) |
| Whisper Large v3 | — | $0.003 per minute | C (8GB+) |

Open-source models are priced 50–66% below comparable centralized providers via the hardware tier system. See [Hardware Tiers](#hardware-tiers) under Provider Onboarding for the full VRAM-to-model mapping.

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

### Dual Deposit Options

Swan 2.0 accepts both stablecoins and SWAN as deposits into a single USD-denominated prepaid balance. SWAN deposits receive a **20% bonus in credits** at deposit time, creating organic buy pressure for the token.

| Deposit Method | Bonus | Example (depositing $100) |
|---------------|-------|-----------------------|
| USDC/USDT (Stripe or crypto) | 0% | $100 of credits |
| SWAN on Swan Mainnet | +20% | $120 of credits |

**Pay-with-SWAN flow:**

1. Consumer deposits SWAN tokens to their per-user deposit address on Swan Mainnet
2. Balance is credited in USD at the current SWAN/USD rate plus a 20% bonus
3. Subsequent inference requests draw from this unified USD balance
4. Providers receive 95% of the per-request fee in their preferred payout currency (USDC or SWAN)
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

The benchmark worker runs periodically (default: every 24 hours) to verify provider quality. Benchmark results expire after **30 days** — providers that miss benchmarks for 30+ days lose qualification and must re-benchmark to resume receiving traffic.

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
| Benchmark results expired (> 30 days) | Loses qualification until re-benchmarked |
| Inference success rate < 80% | Deprioritized in request routing |
| Uptime < 90% (30-day rolling) | Deprioritized in request routing |

{% hint style="info" %}
**New Provider Grace Period:** Providers registered within the last 7 days are exempt from uptime and success-rate deprioritization. This gives new providers time to build history without being penalized for insufficient data. Benchmark requirements and probation still apply during the grace period.
{% endhint %}

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

| Tier | Min VRAM | Example Hardware | Models | Status |
|------|----------|-----------------|--------|--------|
| **S** | 38GB+ | L40S, A100, H100 | 70B premium models | Recruiting new providers |
| **A** | 24GB | RTX 4090, 3090, A6000 | 24B–32B agent models | Activate idle inventory |
| **B** | 12GB | RTX 4070 Ti, 3080 Ti | 8B–12B free tier | Some current providers |
| **C** | 8GB | RTX 3070, 4060 | Embedding, Whisper | Lowest qualifying tier |
| **macOS** | 16GB+ unified | Apple Silicon M1/M2/M3/M4 | 8B–12B via Ollama | Entry-level providers |
| Rejected | < 8GB or legacy | TESLA P4, GTX 1050 Ti | None | No rewards |

Legacy GPUs (TESLA P4, GTX 1050 Ti) served the network well during the ZK-task era but cannot serve AI inference workloads at acceptable quality.

{% hint style="info" %}
**macOS Support:** Apple Silicon Macs can serve as providers using Ollama as the inference backend. While datacenter GPUs offer higher throughput, Macs are a low-friction entry point for new providers — no Docker, NVIDIA drivers, or Linux required.
{% endhint %}

### Requirements

**Linux (NVIDIA GPU):**
- GPU meeting at least Tier C requirements (≥ 8GB VRAM)
- Docker 24.0+ with NVIDIA Container Toolkit
- Inference server: SGLang (recommended), vLLM, or Ollama

**macOS (Apple Silicon):**
- Apple Silicon Mac (M1/M2/M3/M4) with 16GB+ unified memory
- Ollama installed (`brew install ollama`)

**Both platforms:**
- Swan's `computing-provider` agent installed
- No public IP, domain, or SSL setup required — providers connect via WebSocket behind NAT/firewall
- Pass initial inference benchmark (math, code, latency)
- Maintain > 50% uptime over a trailing 7-day window

### Registration Flow

1. **Sign up** at the [Swan Inference dashboard](https://inference.swanchain.io/provider-signup) and get a provider API key (`sk-prov-*`)
2. **Start a model server** — SGLang/vLLM (Linux) or Ollama (macOS)
3. **Install and run** the `computing-provider` agent — the setup wizard auto-discovers your models
4. **Pass benchmarks** — automated quality verification runs within minutes of connecting
5. **Admin approval** — most providers are approved within 24 hours
6. **Deposit collateral** via Stripe (credit card) or SWAN tokens on-chain
7. **Start earning** — receive inference requests with a 7-day grace period for full traffic priority

### Collateral

Providers must deposit collateral to become active on the network. Two deposit methods are available:

| Method | Currency | Processing | Refund |
|--------|----------|-----------|--------|
| **Stripe** | Credit/debit card (USD) | Instant | Refunded to original card (7-day waiting period) |
| **On-chain** | SWAN tokens | Requires gas (SwanETH) | Returned to wallet (7-day waiting period) |

See [Computing Provider Collateral](token/computing-provider-collateral/) for details on collateral amounts and the refund waiting period.

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
