---
description: Decentralized AI Inference Marketplace — Swan Chain's Next Evolution
---

# Swan 2.0: Inference Cloud

## Overview

Swan 2.0 marks Swan Chain's evolution from a UBI-subsidized computing network into a **market-driven AI inference marketplace**. Built as the Inference Cloud, it connects AI model consumers with GPU providers through a decentralized coordination layer, enabling anyone to access 40+ AI models via a single API key — or earn rewards by sharing their GPU resources.

{% hint style="info" %}
**Try Swan Inference**: [https://inference.swanchain.io](https://inference.swanchain.io)

Swan 2.0 builds on the network bootstrapped by UBI (Swan 1.0). Existing UBI mechanisms continue operating during the transition. See [SIP-002](https://github.com/swanchain/governance/discussions/16) for the full transition proposal.
{% endhint %}

## What Changes in Swan 2.0

| Aspect | Swan 1.0 (UBI Model) | Swan 2.0 (Inference Cloud) |
|--------|----------------------|---------------------------|
| **Provider Rewards** | UBI sampling + paid jobs | Contribution-based rewards + inference revenue |
| **Payment** | SWAN tokens only | Stablecoins (USDC/USDT) + SWAN tokens |
| **Provider Roles** | Separate ECP and FCP | Unified Computing Provider (CP) |
| **Collateral** | SWAN tokens only | SWAN tokens (stablecoin option under governance review) |
| **Work Verification** | Random sampling tasks | Periodic benchmarks (math, code, latency) |
| **Workloads** | Training, ZK proofs | AI inference (LLM, image, audio, embedding, multimodal) |

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
    "model": "llama-3.2-3b",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

## Supported Models

Swan Inference hosts **42+ AI models** across five categories:

| Category | Examples | Pricing Model |
|----------|----------|---------------|
| **LLM** | Llama 3, DeepSeek, Qwen, Mistral | Per input/output token |
| **Image** | FLUX, Stable Diffusion | Per request |
| **Audio** | Whisper | Per request |
| **Embedding** | BGE, E5 | Per token |
| **Multimodal** | Llama Vision, Qwen-VL | Per token |

Model availability is shown in real time at [inference.swanchain.io/models](https://inference.swanchain.io/models).

## Dual Token Payment Model

Swan 2.0 introduces **stablecoin payments** alongside SWAN tokens:

### For Consumers

- Pay for inference in **USDC** (default marketplace currency)
- Transparent per-token or per-request pricing
- API key authentication (`sk-swan-*` keys)

### For Providers

Providers earn through two revenue streams:

1. **Inference Revenue** — Stablecoin (USDC) earnings from serving inference requests
2. **SWAN Token Rewards** — Contribution-based rewards from the daily reward pool (replacing UBI)

When paid inference generates protocol revenue, the split is:

| Recipient | Share |
|-----------|-------|
| **Provider** | 70% (paid in request currency) |
| **Protocol Treasury** | 20% |
| **SWAN Buyback & Burn** | 10% |

The platform charges a **5% platform fee** on each transaction.

## Quality Assurance

### Benchmarks

The benchmark worker runs periodically (default: every 24 hours) to verify provider quality:

| Test | Pass Threshold |
|------|---------------|
| **Math Accuracy** | ≥ 50% |
| **Code Generation** | ≥ 50% |
| **Response Latency** | ≤ 5000ms |

Providers that fail benchmarks may be suspended from receiving requests. With slashing enabled, providers that fail consecutively can have a portion of their collateral slashed (configurable, default 10% per consecutive failure).

### Health Monitoring

- **Automatic health checks** with configurable thresholds for WebSocket and external endpoints
- **Circuit breaker** to prevent cascading failures
- **Load balancing** with health-aware routing (round-robin, least-connections, or health-aware strategies)
- **Model warmup** to pre-load models and reduce cold-start latency

### Provider Leaderboard

Providers are ranked by a performance-based leaderboard using availability metrics, success rates, and latency.

## Provider Onboarding

### Requirements

- GPU with ≥ 24 GB VRAM (≥ 48 GB recommended for priority routing)
- Swan's `computing-provider` agent installed
- No public IP, domain, or SSL setup required — providers connect via WebSocket behind NAT/firewall

### Registration Flow

1. Sign up at the Swan Inference dashboard and get a provider API key (`sk-prov-*`)
2. Install the `computing-provider` agent on your GPU machine
3. Deposit collateral (stablecoin or SWAN tokens)
4. Pass the initial benchmark verification
5. Begin receiving inference requests and earning rewards

### Collateral

Providers must deposit collateral to become active on the network. See [Computing Provider Collateral](token/computing-provider-collateral/) for details on collateral types, amounts, and the 7-day refund waiting period.

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

## Transition from UBI (Swan 1.0)

The transition from UBI to contribution-based rewards follows a 3-phase plan per [SIP-002](https://github.com/swanchain/governance/discussions/16):

| Phase | Duration | Description |
|-------|----------|-------------|
| **Phase 1: Hybrid** | Month 1 | 75% UBI + 25% contribution-weighted rewards |
| **Phase 2: Contribution-Weighted** | Month 2 | 50% UBI + 50% contribution-weighted rewards |
| **Phase 3: Pure Contribution** | Month 3+ | 100% contribution-based (governance vote required) |

During the transition, existing ECP and FCP roles merge into a unified **Computing Provider (CP)** classification. All providers are evaluated equally based on contribution metrics. See [Computing Provider Income](token/swan-provider-income.md) for the full contribution score formula.

## Learn More

- **[Inference Marketplace](market-provider/inference-marketplace.md)** — How the marketplace works: pricing, routing, and settlement
- **[Computing Provider Income](token/swan-provider-income.md)** — Contribution score formula and reward distribution
- **[Computing Provider Collateral](token/computing-provider-collateral/)** — Collateral requirements and slashing
- **[SIP-001: FCP Subsidy Program](https://github.com/swanchain/governance/discussions/11)** — Stage 1 funding for computing providers
- **[SIP-002: Unified CP & Contribution Rewards](https://github.com/swanchain/governance/discussions/16)** — Full transition proposal
