---
description: Decentralized AI Inference Marketplace for Real-Time Model Serving
---

# Inference Marketplace

The Inference Marketplace is Swan Chain's decentralized platform for AI model serving, introduced as part of [Swan 2.0](../swan-2.0-inference-cloud.md). Unlike the existing [AI Computing Marketplace](decentralized-ai-computing-marketplace/) which handles training workloads through task auctions, the Inference Marketplace provides **real-time, low-latency AI inference** through persistent WebSocket connections and an OpenAI-compatible API.

## How It Works

### Request Lifecycle

```
Consumer                Swan Inference              Provider
   │                         │                         │
   │  POST /v1/chat/...      │                         │
   │────────────────────────▶│                         │
   │                         │  Select best provider   │
   │                         │  (load balancing)        │
   │                         │                         │
   │                         │  Forward via WebSocket   │
   │                         │────────────────────────▶│
   │                         │                         │  Run inference
   │                         │  Stream response         │
   │                         │◀────────────────────────│
   │  Stream response        │                         │
   │◀────────────────────────│                         │
   │                         │                         │
   │                         │  Record usage            │
   │                         │  (tokens, latency)       │
```

1. **Consumer** sends an inference request via the REST API with their API key
2. **Swan Inference** selects the best available provider using health-aware load balancing
3. The request is forwarded to the provider over a persistent **WebSocket** connection
4. The provider runs inference on their GPU and streams the response back
5. Swan Inference records usage metrics (tokens processed, latency, success/failure)
6. Usage is aggregated for billing and settlement

### Provider Connection Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **WebSocket Provider** | GPU provider running `computing-provider` agent, connects via WebSocket | Primary path — no public IP required |
| **External Endpoint** | Existing OpenAI-compatible server (vLLM, TGI, OpenAI API) registered with Swan | Fallback — for providers with existing infrastructure |

The response includes an `X-Swan-Connection-Mode` header indicating which path was used.

## Provider Registration and Collateral

### Registration Flow

1. **Sign up** at the Swan Inference dashboard
2. **Upgrade to provider** and receive a provider API key (`sk-prov-*`)
3. **Deposit collateral** — stablecoin (USDC/USDT on-chain) or USD (via payment gateway)
4. **Connect** via WebSocket using the `computing-provider` agent
5. **Pass benchmark** — initial verification (math, code, latency tests)
6. **Begin serving** — provider becomes active and receives inference requests

### Collateral Options

| Type | Method | Verification |
|------|--------|-------------|
| **Stablecoin (USDC/USDT)** | On-chain deposit to ProviderCollateral contract | Automatic on-chain tx verification |
| **USD** | Stripe, PayPal, or bank transfer | Admin confirmation with payment reference |

Collateral status follows the lifecycle: `pending → confirmed → refund_requested → refunded`

The refund waiting period is **7 days** from the time of request, ensuring all pending settlements are cleared before funds are released.

See [Computing Provider Collateral](../token/computing-provider-collateral/) for detailed collateral amounts and slashing rules.

## Request Routing and Load Balancing

Swan Inference routes requests using configurable load balancing strategies:

| Strategy | Description |
|----------|-------------|
| **Health-Aware** | Routes to the provider with the best health score (default) |
| **Round-Robin** | Distributes requests evenly across providers |
| **Least-Connections** | Routes to the provider with the fewest active requests |

Additional routing features:

- **Health monitoring** with automatic circuit breaker for unhealthy providers
- **Model warmup** to pre-load models and reduce cold-start latency
- **Retry and failover** — up to 2 retries with exponential backoff if a provider fails
- **Rate limiting** per API key with tiered limits by model category

### Rate Limits (Default)

| Category | Requests/min |
|----------|-------------|
| LLM | 200 |
| Image | 60 |
| Embedding | 500 |
| Other | 200 |

## Pricing Model

### Consumer Pricing

Pricing varies by model category:

| Category | Pricing Unit | Examples |
|----------|-------------|----------|
| **LLM** | Per input token + per output token | Chat completions, text generation |
| **Embedding** | Per token | Text embeddings |
| **Image** | Per request | Image generation |
| **Audio** | Per request | Transcription |

Prices are listed transparently in the model catalog at [inference.swanchain.io/models](https://inference.swanchain.io/models). The default marketplace currency is **USDC**.

### Platform Fee

The platform charges a **5% fee** on each transaction. This fee funds protocol operations, staking rewards, and SWAN token burns.

### Revenue Distribution

When a consumer pays for an inference request:

| Recipient | Share | Description |
|-----------|-------|-------------|
| **Provider** | 70% | Paid in the request currency (USDC) |
| **Protocol Treasury** | 20% | Funds ecosystem development |
| **SWAN Buyback & Burn** | 10% | Deflationary mechanism |

## Settlement

### Off-Chain Ledger

Usage is tracked in real time on an off-chain payment ledger:

- Every inference request records: tokens processed, latency, model used, provider, consumer
- Provider earnings accumulate in the ledger
- Consumers are billed based on aggregated usage

### On-Chain Settlement

Settlement uses a **MerkleDistributor** smart contract for gas-efficient batch payouts:

1. **Daily batches** — Provider earnings are aggregated into settlement batches
2. **Merkle tree** — A Merkle tree is computed from all provider balances in the batch
3. **On-chain submission** — The Merkle root is submitted to the smart contract
4. **Provider claims** — Providers claim their earnings by submitting a Merkle proof

Settlement status follows: `pending → submitted → confirmed`

This approach settles many provider payments in a single on-chain transaction, minimizing gas costs.

### Minimum Payout

Providers must accumulate a minimum balance (default: **$50**) before a payout is triggered. This prevents dust transactions and reduces gas costs.

## Provider Earnings

Providers earn through two complementary streams:

### 1. Inference Revenue (Stablecoins)

Direct payment for serving inference requests, paid in the consumer's currency (typically USDC). This is the primary revenue stream and scales with the number of requests served.

### 2. Contribution Rewards (SWAN Tokens)

Daily SWAN token rewards allocated proportionally based on the provider's [Contribution Score](../token/swan-provider-income.md#swan-2.0-market-driven-income). This replaces the legacy UBI model and rewards providers for:

- Inference volume (requests processed)
- Token throughput (tokens generated)
- Uptime and availability
- Quality (success rate, latency)
- Model diversity (number of models served)

### Earnings Dashboard

The provider dashboard provides:

- **Daily/weekly/monthly** earnings views with CSV export
- **Per-model** performance metrics (requests, success rate, tokens processed)
- **Collateral status** and on-chain deposit tracking
- **Wallet verification** via MetaMask for secure payouts

## Comparison with AI Computing Marketplace

| Feature | AI Computing Marketplace | Inference Marketplace |
|---------|------------------------|----------------------|
| **Workload** | Training, batch compute | Real-time inference |
| **Latency** | Minutes to hours | Milliseconds to seconds |
| **Allocation** | Task auction (bidding) | Real-time routing (WebSocket) |
| **Payment** | Per task | Per token / per request |
| **Connection** | Job-based | Persistent WebSocket |
| **API** | Swan SDK / Orchestrator | OpenAI-compatible REST API |

Both marketplaces coexist within the Swan ecosystem, serving different use cases. The Inference Marketplace is optimized for interactive AI applications, while the AI Computing Marketplace handles batch training and compute-intensive tasks.

## Learn More

- **[Swan 2.0: Inference Cloud](../swan-2.0-inference-cloud.md)** — Overview of the Swan 2.0 platform
- **[Computing Provider Income](../token/swan-provider-income.md)** — Contribution scoring and reward distribution
- **[Computing Provider Collateral](../token/computing-provider-collateral/)** — Collateral requirements and slashing
