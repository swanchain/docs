---
description: >-
  Swan 2.0 turns Swan Chain into an AI inference marketplace: one OpenAI-compatible
  API in front of a network of independent GPU providers, paid per token.
---

# Swan 2.0: Inference Cloud

## Overview

Swan Inference ([inference.swanchain.io](https://inference.swanchain.io)) is a marketplace for AI inference. Developers call a single OpenAI-compatible API and pay per token from a prepaid credit balance or a monthly Token Plan. Independent GPU providers run the open-source [`computing-provider`](https://github.com/swanchain/computing-provider) agent, connect outbound over WebSocket, and are paid a published per-token payout price for every request they serve. The platform verifies that providers serve what they claim, routes requests to healthy providers, and settles usage.

Two audiences, two guides:

* **Developers** — [For Developers](how-to-use.md): playground, account, credits, first request. Then the [API Reference](../../bulders/app-developer/swan-inference-api.md).
* **GPU owners** — [For GPU Providers](become-a-provider.md): model server, agent, verification, collateral, payouts.

This page explains the design that both sit on.

## What changed from Swan 1.0

| Aspect | Swan 1.0 | Swan 2.0 (Inference Cloud) |
|--------|----------|---------------------------|
| **What providers sell** | Registered hardware, sampled by UBI tasks | AI inference actually served — LLM, multimodal, image, audio, embedding |
| **How providers are paid** | Daily UBI allocation in SWAN | Per token, at a payout price published for every model |
| **How consumers pay** | Task auctions in SWAN | USD credits (card or crypto: USDC, USDT, SWAN) or a monthly Token Plan; one OpenAI-compatible API |
| **Provider roles** | Separate ECP and FCP | One Computing Provider |
| **Network requirements** | Public IP, domain, TLS | None — the agent connects outbound over WebSocket |
| **Quality control** | Random sampling tasks | Registration and periodic benchmarks, fingerprint/logprob verification, context-window integrity checks, trust-weighted routing, collateral slashing with appeal |
| **Collateral** | Sized by Computing Units in SWAN | Per provider account: USDC on Ethereum or Base, SWAN on Swan Chain, or card |
| **Hardware requirements** | Any GPU | ≥ 8 GB VRAM, Ampere or newer; Apple Silicon via Ollama |

The Swan 1.0 material is preserved under [Legacy: Swan 1.0](../../swan-chain-campaign/README.md).

## How a request flows

```
  Developers (OpenAI SDKs, LiteLLM, agents, the web playground)
                          │  HTTPS  https://inference.swanchain.io/v1
                          ▼
  ┌─────────────────────────────────────────────────────────────┐
  │  Swan Inference                                             │
  │  OpenAI-compatible API ─► routing ─► WebSocket hub          │
  │  auth · rate limits · catalog · billing · verification      │
  └─────────────────────────────────────────────────────────────┘
          │ persistent WebSocket (outbound from provider)     │ HTTPS (fallback)
          ▼                                                   ▼
  GPU providers running computing-provider           Registered external endpoints
  (SGLang / vLLM / Ollama behind NAT is fine)         (OpenAI-compatible servers)
```

1. The developer sends an OpenAI-format request with a `sk-swan-*` key. Auth, validation and rate limits are applied at the edge.
2. The router picks a provider that is online for the model and whose reported context window fits the request, weighing recent health, current load and latency; degraded providers only receive probe traffic.
3. The request is forwarded over the provider's existing WebSocket connection (or, if no connected provider is available, to a registered external endpoint). Streamed tokens are relayed as they arrive.
4. Usage is metered, the consumer is charged at the model's price, and the provider is credited at the model's payout price.
5. The response carries attribution headers — `X-Swan-Provider-ID`, `X-Swan-Provider-Name`, `X-Swan-Connection-Mode`, `X-Swan-Latency-Ms`, `X-Swan-Request-ID` — so every answer is traceable to who served it. See [Response Headers](../../bulders/app-developer/swan-inference-api.md#response-headers).

Providers never need a public URL, a domain or a certificate: the agent dials `wss://inference-ws.swanchain.io` and everything flows over that connection.

## The API

| Endpoint | Purpose |
|----------|---------|
| `POST /v1/chat/completions` | Chat, streaming, tools, vision, JSON mode — forwarded to the model as sent |
| `POST /v1/completions` | Legacy text completion (compatibility shim over chat) |
| `POST /v1/embeddings` | Text embeddings |
| `POST /v1/images/generations` | Image generation |
| `POST /v1/audio/transcriptions` | Speech to text |
| `GET /v1/models` | Model IDs (no key required) |
| `GET /api/v1/models` | Full public catalog: prices, tier, context window, online providers |

```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [{"role": "user", "content": "Explain Swan Chain in one sentence."}]
  }'
```

Any OpenAI SDK works by changing `base_url` to `https://inference.swanchain.io/v1`. Full details, limits and error codes: [API Reference](../../bulders/app-developer/swan-inference-api.md).

## The catalog

The catalog is curated by the platform and is the source of truth for model IDs, prices and serving expectations. It has two kinds of models:

* **Open-source models on community GPUs** — served by independent providers running SGLang, vLLM or Ollama. Each model is advertised at a *canonical serving configuration* (precision and engine), and providers are verified against it.
* **Frontier gateway models** — `anthropic/*`, `openai/*`, `gemini/*`, `moonshotai/*` and similar, reachable through the same API and billing.

Every model carries a **tier** — `standard` or `premium` — which decides whether the Token Plan covers it, and publishes **two prices** per million tokens: what consumers pay and what the serving provider is paid. Browse it at [inference.swanchain.io/models](https://inference.swanchain.io/models) or fetch `GET /api/v1/models`. Because prices and availability change, this documentation does not reproduce the price list.

{% hint style="info" %}
A model is callable only while at least one provider is online for it. The catalog shows the live provider count per model, and the [network page](https://inference.swanchain.io/network) shows who is serving what right now. Providers looking for demand can use `computing-provider inference recommend-models`.
{% endhint %}

## Paying for inference

**Credits (pay-as-you-go).** Your balance is one USD-denominated pool. Fund it by card through Stripe (minimum $5; card processing fees are shown before you pay) or by crypto deposit to your personal deposit address (minimum $1): **USDC on Ethereum or Base**, or **SWAN on Swan Chain**. SWAN deposits are credited at the current SWAN/USD rate **plus a 20% bonus** — $100 of SWAN becomes $120 of credits. Requests deduct from the balance in real time; the ledger is under **Billing** in the dashboard.

**Token Plan (Pro).** $6/month, billed monthly by card. It includes **40M tokens per week and 1,500 requests per day on standard-tier models** (and 75 images/day), at 50 requests/min and 8 concurrent. Premium-tier models, and anything beyond the allowance, are pay-as-you-go from your credit balance. See [inference.swanchain.io/pricing](https://inference.swanchain.io/pricing).

**Playground.** [inference.swanchain.io/playground](https://inference.swanchain.io/playground) runs a small model for anonymous visitors, rate-limited per IP, so you can try the service before creating an account.

## Economics: the two-price model

There is no percentage commission. For every model the platform publishes:

* an **input and output price** — what the consumer pays per 1M tokens, and
* a **payout input and output price** — what the provider is paid per 1M tokens.

The platform's margin is the spread between the two, and a payout price can never exceed the consumer price. Providers do not quote their own prices; they choose which catalog models to serve at the published payout. At the time of writing the payout is **90% of the consumer price** for almost every model in the catalog — you can check any model yourself, since both prices are returned by `GET /api/v1/models`.

For Token Plan traffic, providers are credited at the same payout price; the total paid out for plan requests each month is capped at the plan revenue pool and pro-rated if usage exceeds it. Details in [How the Marketplace Works](../market-provider/inference-marketplace.md).

## Quality assurance

Trust in a marketplace of anonymous GPUs has to be earned per request. The platform:

* **Benchmarks** every provider at registration and periodically afterwards (math, code and latency tests; results expire after 30 days and must be refreshed).
* **Verifies identity of the served model** with fingerprint challenges (hashes of registered weight files), deterministic fixed-seed comparisons, and statistical logprob comparisons against the canonical serving configuration.
* **Checks context-window integrity** — that a provider really serves the context length it advertises, rather than truncating silently. See the [Provider Notice](provider-context-window-faq.md).
* **Weights routing by trust and health.** Verification results and success rates feed a trust score that influences how much traffic a provider receives; repeated failures open a circuit breaker until the provider recovers.
* **Slashes collateral** for verified misbehaviour, with a 48-hour appeal window on every penalty record. Honest small windows and modest hardware are never penalised — only misrepresentation is.

| Condition | Consequence |
|-----------|------------|
| Benchmark failure (1st) | Warning; temporary removal from routing |
| Consecutive benchmark failures | Collateral slash (10%, then 30% and network removal) |
| Benchmark results older than 30 days | Loses qualification until re-benchmarked |
| Verified model or context misrepresentation | Displayed context capped to the measured window; continued misrepresentation is slashed |
| Low success rate or uptime | Deprioritised in routing (new providers get a 7-day grace period) |

The platform publishes *what* it verifies and the *results* (trust, uptime, benchmark scores on the network page), but not the detection thresholds.

## Becoming a provider

**Hardware.** Enforced minimum: an NVIDIA GPU with **≥ 8 GB VRAM and compute capability 8.0 or newer** (Ampere onwards — RTX 3060 and up, A-series, H100), or an Apple Silicon Mac with 16 GB+ unified memory using Ollama. Older cards (Pascal/Turing, TESLA P4, GTX 10-series) are not eligible. As a guide to what you can serve well:

| VRAM | Example hardware | Typical models |
|------|-----------------|----------------|
| 38 GB+ | L40S, A100, H100 | 70B-class models (quantized) |
| 24 GB | RTX 4090, 3090, A6000 | 24B–32B models |
| 12 GB | RTX 4070 Ti, 3080 Ti | 8B–12B models |
| 8 GB | RTX 3070, 4060 | Small LLMs, embeddings, Whisper |
| 16 GB+ unified | Apple M-series | 8B–12B via Ollama |

**Flow.**

1. **Sign up** at [inference.swanchain.io/provider-signup](https://inference.swanchain.io/provider-signup) and save your provider key (`sk-prov-*`) — it is shown once.
2. **Start a model server** (SGLang or vLLM on Linux, Ollama on macOS) serving a catalog model.
3. **Install `computing-provider`** and run the setup wizard; it saves the key, discovers your model server and writes `models.json`.
4. **Connect.** The agent registers your models; the registration benchmark runs within minutes.
5. **Deposit collateral** (below).
6. **Activation is automatic** once collateral is verified, the GPU passes eligibility and the benchmark passes. New providers start a **24-hour probation** and must keep at least 50% uptime through it. Admin approval exists as a parallel path but is not required.
7. **Earn** per token for every request served.

| Status | Can connect | Earning | Meaning |
|--------|-------------|---------|---------|
| `pending` | yes | no | Awaiting collateral, GPU eligibility or benchmark |
| `under_review` | yes | no | Manual review requested |
| `approved` | yes | no | Admin approved, awaiting collateral |
| `activating` | yes | no | Collateral verified, activation in progress |
| `active` | yes | **yes** | Serving and earning ("Active (Probation)" for the first 24 h) |
| `suspended` | no | no | Suspended by admin or during a collateral refund |
| `rejected` | no | no | Application rejected |
| `offline` | no | no | Connection lost |

Step-by-step, with the exact config: [Become a Provider](become-a-provider.md).

### Collateral

Collateral is a refundable deposit that backs the slashing rules. Deposit on-chain to the platform's collateral contract on any supported chain, or pay by card:

| Chain | Chain ID | Token | Minimum | Collateral contract |
|-------|----------|-------|---------|--------------------|
| Ethereum | 1 | USDC | 20 USDC | `0x1dEe92Da8fc4878795418aEde112100A57286a9a` |
| Base | 8453 | USDC | 20 USDC | `0x7fac98B02f4Fcda9Ac49508eb2E97E4BE4fecE9B` |
| Swan Chain | 254 | SWAN | 35,000 SWAN | `0x7fac98B02f4Fcda9Ac49508eb2E97E4BE4fecE9B` |
| Card (Stripe) | — | USD | shown at checkout | — |

The current table is always available from `GET /api/v1/provider/collateral/contract` and from `computing-provider inference deposit`. Refunds have a **7-day waiting period**, cannot start while a payout is pending, and the provider is suspended from routing for the duration of the refund window. Detail: [Earnings and Collateral](../token/computing-provider-collateral/README.md).

### Earnings and payouts

* Earnings accrue per request at the model's payout price and are visible in the provider dashboard (daily/weekly/monthly, per model, CSV export).
* Settlement runs in daily batches; earnings from Token Plan traffic are held until the month-end pro-rating.
* Payouts are requested from the dashboard to your beneficiary wallet: **minimum $10**, a **flat $1 fee**, one request per chain per hour, one pending payout at a time.
* Providers can also convert earnings directly into inference credit without an on-chain round-trip.

## Smart contracts (Swan Chain mainnet, chain ID 254)

| Contract | Address |
|----------|---------|
| Provider collateral (SWAN) | `0x7fac98B02f4Fcda9Ac49508eb2E97E4BE4fecE9B` |
| SWAN token (bridged L2) | `0xBb4eC1b56cB624863298740Fd264ef2f910d5564` |

Collateral contracts on Ethereum and Base are listed above. Chain parameters and explorers: [Network Info](../../network-reference/readme/README.md); the full list is in [Contract Addresses](../../network-reference/contract-addresses.md).

## UBI has ended (SIP-003)

Under Swan 1.0 most daily UBI went to registered hardware that served nothing. [SIP-003](https://github.com/swanchain/governance/discussions/21) tapered the UBI allocation to zero and it is now **off**: there is no daily SWAN allocation for registered hardware, and provider income comes solely from the per-token payout on inference actually served. The Swan 1.0 UBI documentation is preserved under [Legacy: Swan 1.0](../../swan-chain-campaign/README.md) for the record.

## SWAN token utility

| Utility | Description |
|---------|-------------|
| **Pay with SWAN** | Deposit SWAN on Swan Chain for a 20% credit bonus — the lowest effective price on every model |
| **Provider collateral** | 35,000 SWAN on Swan Chain is one of the accepted collateral forms |
| **Governance** | Vote on protocol parameters, pricing policy and incentive changes through SIPs |

## In development

The following are being built and tested and are **not yet available on the production API**. They will be announced on the [pricing page](https://inference.swanchain.io/pricing) FAQ and in this documentation when live:

* **Per-request provider selection** — pin a request to a specific provider (`X-Swan-Provider`), optionally refusing fallbacks, with a receipt in the response saying who served it, why a fallback happened, and how it was billed. Explicit selection will always be pay-as-you-go.
* **Per-offering transparency** — each provider's offering of a model will show its own price source, quantization, 30-day uptime and typical time-to-first-token, and each model will state the context window it can *guarantee* across online providers versus the maximum any provider reports.

## Learn more

* **[For Developers](how-to-use.md)** — signup, credits, first request
* **[For GPU Providers](become-a-provider.md)** — full setup walkthrough
* **[How the Marketplace Works](../market-provider/inference-marketplace.md)** — routing, billing, settlement and verification in depth
* **[Computing Provider Income](../token/swan-provider-income.md)** and **[Earnings and Collateral](../token/computing-provider-collateral/README.md)**
* **[Provider Notice: Context-Window Integrity](provider-context-window-faq.md)**
* **[`computing-provider` on GitHub](https://github.com/swanchain/computing-provider)** — installation, configuration reference, troubleshooting
