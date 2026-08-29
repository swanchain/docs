---
description: How Swan Inference routes, prices, bills, verifies and settles a request
---

# How the Marketplace Works

The Inference Marketplace is the mechanism behind [Swan 2.0: Inference Cloud](../swan-2.0-inference-cloud/README.md). This page follows a request from the API to a GPU and back, then covers pricing, billing, settlement and quality control. For the practical guides see [For Developers](../swan-2.0-inference-cloud/how-to-use.md) and [For GPU Providers](../swan-2.0-inference-cloud/become-a-provider.md).

## Request lifecycle

1. **Authenticate and validate.** A request to `https://inference.swanchain.io/v1/...` carries a consumer key (`sk-swan-*`). The body is validated (OpenAI shape, roles, `response_format`, size limits) and checked against the key's rate limits for the model's category. The raw body is kept so that fields the gateway does not model — `tools`, `seed`, `stream_options`, vendor-specific parameters — reach the model unchanged.
2. **Admit.** The gateway estimates the prompt's token count and considers only providers whose reported context window can hold it. Where a provider has not reported a window, the catalog value is assumed — the common case today, and the reason providers are asked to [declare their real window](../swan-2.0-inference-cloud/provider-context-window-faq.md).
3. **Select a provider.** Among providers online for the model, selection is weighted by recent health (success rate, uptime, verification trust), current load relative to capacity, and latency. Providers in a degraded state receive only a small share of probe traffic so recovery stays detectable without sending consumers into a failing node.
4. **Dispatch.** The request travels over the provider's existing WebSocket connection; streamed chunks are relayed to the consumer as they arrive. If no connected provider is available, a registered external OpenAI-compatible endpoint can serve as fallback. Transient provider failures are retried on another provider.
5. **Meter and bill.** Token usage from the response is recorded. The consumer is charged at the model's price (or the request is counted against their Token Plan); the provider is credited at the model's payout price.
6. **Attribute.** The response carries `X-Swan-Provider-ID`, `X-Swan-Provider-Name`, `X-Swan-Connection-Mode`, `X-Swan-Latency-Ms` and `X-Swan-Request-ID`. Nothing served on Swan is anonymous compute.

## Provider connection modes

| Mode | How | Who uses it |
|------|-----|-------------|
| **WebSocket** (`X-Swan-Connection-Mode: websocket`) | The open-source `computing-provider` agent dials `wss://inference-ws.swanchain.io`, authenticates with a provider key or a wallet signature, registers its models and keeps the connection open. Requests, verification challenges, heartbeats and warm-ups all flow over it. No inbound port, domain or certificate. | Community GPU providers |
| **External endpoint** (`external`) | A provider registers the URL of an OpenAI-compatible server (vLLM, SGLang, TGI, a gateway). The platform health-checks it and routes to it when no WebSocket provider is available. | Providers with existing serving infrastructure |

## Pricing: the two-price model

Every catalog model publishes two prices, both set by the platform, both per 1M tokens (per image for image models, per minute for audio):

| | Who | Where to see it |
|---|---|---|
| **Price** (`input_price`, `output_price`) | What the consumer pays | Model page, [pricing page](https://inference.swanchain.io/pricing), `GET /api/v1/models` |
| **Payout price** (`payout_input_price`, `payout_output_price`) | What the serving provider is paid | Provider dashboard, `GET /api/v1/models` |

The platform margin is the spread. There is **no percentage commission** and no separate platform fee line on a bill. Two invariants are enforced whenever the catalog changes: a priced model must have a payout price, and the payout can never exceed the price. Providers choose which models to serve; they do not set prices. At the time of writing the payout is 90% of the consumer price for almost every model.

Consumer-facing prices are denominated in USD and deducted from the credit balance whatever it was funded with. Zero-priced models exist in the catalog but are rare and only callable while someone serves them; they carry a stricter shared rate limit (10 requests/min per key).

## Consumer billing

**Credits.** Consumers prepay into a USD balance by card (Stripe, minimum $5, processing fee shown before purchase) or by sending crypto to a personal deposit address (minimum $1): USDC on Ethereum (chain 1) or Base (8453), or SWAN on Swan Chain (254). SWAN deposits are credited at the live SWAN/USD rate plus a **20% bonus**. Each request reserves an estimate from the balance, then settles to the exact token usage; the balance and a double-entry ledger are visible under **Billing**.

**Token Plan (Pro).** $6 per month, billed monthly by card. Included: 40M tokens/week and 1,500 requests/day on **standard-tier** models, 75 images/day, 50 requests/min, 8 concurrent. **Premium-tier** models (Claude, Gemini Pro and similar) are never covered and bill per token from the credit balance, as does any usage beyond the allowance. Plan usage is counted at admission time, so a request that would exceed the allowance falls through to pay-as-you-go if credit is available and is otherwise rejected.

**Playground.** Anonymous, browser-only, one small model, rate-limited per IP (5 requests/hour), non-streaming. A way to try the service, not an API.

## Provider earnings and settlement

* Every served request credits the provider `tokens × payout price`. Reference nodes that the platform runs to baseline a model earn nothing for that model.
* **Token Plan traffic** is credited at the same payout price, but the month's total plan payouts are capped at the plan revenue pool (subscribers × $6). If usage exceeds the pool, payouts are pro-rated across providers by contribution; these earnings are held as *settlement pending* until month end.
* **Settlement** aggregates usage into daily batches per provider and collateral chain.
* **Payouts** are requested from the provider dashboard to the beneficiary wallet: minimum $10, flat $1 fee, one request per chain per hour, one pending payout at a time. Earnings can alternatively be converted into inference credit on the same account.

## Collateral

Before activation a provider deposits refundable collateral, which backs the slashing rules:

| Chain | Chain ID | Token | Minimum | Contract |
|-------|----------|-------|---------|----------|
| Ethereum | 1 | USDC | 20 | `0x1dEe92Da8fc4878795418aEde112100A57286a9a` |
| Base | 8453 | USDC | 20 | `0x7fac98B02f4Fcda9Ac49508eb2E97E4BE4fecE9B` |
| Swan Chain | 254 | SWAN | 35,000 | `0x7fac98B02f4Fcda9Ac49508eb2E97E4BE4fecE9B` |
| Card (Stripe) | — | USD | shown at checkout | — |

Live values: `GET /api/v1/provider/collateral/contract`. Refunds carry a 7-day waiting period, are blocked while a payout is pending, and suspend the provider from routing until complete. See [Earnings and Collateral](../token/computing-provider-collateral/README.md).

## Activation and probation

A `pending` provider is activated automatically when three conditions hold: collateral verified; GPU eligible (≥ 8 GB VRAM, compute capability 8.0+); registration benchmark passed. Activation starts a 24-hour probation — at least 50% uptime through it keeps the provider `active`, otherwise it returns to `pending`. Admin approval is a parallel path, not a prerequisite. Status values are listed in the [overview](../swan-2.0-inference-cloud/README.md#becoming-a-provider).

## Verification and trust

The platform continuously checks that a provider serves the model it claims, at the serving configuration the catalog advertises:

| Check | What happens | What a provider needs |
|-------|--------------|-----------------------|
| **Benchmarks** | Math, code and latency tests at registration and periodically; results expire after 30 days | A healthy backend; SGLang or vLLM recommended |
| **Fingerprint** | The platform names registered weight files; the agent returns their hashes | Weights on disk that match the catalog model |
| **Deterministic** | A fixed-seed prompt compared with a reference response | Seed support in the backend |
| **Logprob** | Statistical comparison of token log-probabilities with the canonical configuration | `logprobs` support in the backend (probed at registration) |
| **Context-window integrity** | Truncation monitoring and recall challenges sized near the advertised window | An honestly declared `context_length` — see the [notice](../swan-2.0-inference-cloud/provider-context-window-faq.md) |

Results move a **trust score** that weights routing. Repeated failures trip a circuit breaker that stops traffic until the provider recovers. Verified misrepresentation is escalated: first a notice and a cap of the displayed context to what was measured, then a collateral slash, every penalty record carrying a **48-hour appeal window**. The platform publishes what it checks and the outcomes — trust, uptime and benchmark scores appear on the [network page](https://inference.swanchain.io/network) — but not the detection thresholds.

## Relationship to the Swan 1.0 marketplaces

The auction-based [AI Computing Marketplace](decentralized-ai-computing-marketplace/README.md), the [Storage Market](storage-market.md) and the [ZK Proof Marketplace](indexing-and-caching-marketplace/README.md) belong to Swan 1.0 and are documented under [Legacy: Swan 1.0](../../swan-chain-campaign/README.md). Swan 2.0 has one product — inference — and one provider role.

## Learn more

* **[Swan 2.0: Inference Cloud](../swan-2.0-inference-cloud/README.md)** — the overview
* **[API Reference](../../bulders/app-developer/swan-inference-api.md)** — endpoints, headers, limits, errors
* **[Become a Provider](../swan-2.0-inference-cloud/become-a-provider.md)** — the setup walkthrough
* **[Computing Provider Income](../token/swan-provider-income.md)** — how earnings are computed
