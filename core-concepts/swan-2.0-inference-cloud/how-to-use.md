---
description: Step-by-step guide to using Swan 2.0 Inference Cloud as a consumer
---

# How to Use Swan Inference

This guide walks through using Swan Inference as a developer consuming AI models — starting with a no-signup trial, then creating an account, topping up, and making real API requests.

{% hint style="info" %}
Looking to earn by providing GPU resources instead? See the provider onboarding section in [Swan 2.0: Inference Cloud](README.md#provider-onboarding).
{% endhint %}

## 0. Try it now — no signup

The fastest way to see Swan Inference in action: open the [playground](https://inference.swanchain.io/playground), pick a model, and send a message. No account, no API key, no credit card.

<figure><img src="../../.gitbook/assets/inference-how-to/playground.png" alt="Swan Inference playground"><figcaption>Playground — runs GLM 4.7 Flash for anonymous visitors, rate-limited to 5 requests per hour per IP.</figcaption></figure>

Prefer the command line? There's an unauthenticated endpoint behind the playground UI:

```bash
curl https://inference.swanchain.io/v1/playground/chat \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [{"role": "user", "content": "Say hello in 5 words."}]
  }'
```

Limits: GLM 4.7 Flash only, 100 output tokens, no streaming, 5 requests per hour per IP. Good enough to kick the tires — sign up below for the full API.

## 1. Sign up and get your API key

Create a free account at [inference.swanchain.io/signup](https://inference.swanchain.io/signup) — email and password only, no credit card required.

<figure><img src="../../.gitbook/assets/inference-how-to/signup.png" alt="Swan Inference signup form"><figcaption>Sign up with email and password.</figcaption></figure>

After signing up, navigate to **Keys** in the dashboard. Your API key (`sk-swan-*`) is generated automatically — copy it and keep it secret.

<figure><img src="../../.gitbook/assets/inference-how-to/dashboard-api-key.png" alt="Dashboard showing API key"><figcaption>Your API key appears under Keys in the dashboard.</figcaption></figure>

## 2. Top up credits

Inference requests are paid per token, deducted from your account balance in real time. Fund your account via Stripe (credit card) or crypto deposit (USDC / USDT / SWAN on multiple EVM chains).

- **Stripe:** instant processing, minimum deposit $5
- **Crypto:** per-user HD-derived deposit address shared across EVM chains, minimum $1

<figure><img src="../../.gitbook/assets/inference-how-to/deposit.png" alt="Deposit credits via Stripe or crypto"><figcaption>Add funds via Stripe card payment or crypto deposit.</figcaption></figure>

### 20% bonus when depositing SWAN

Depositing SWAN tokens on Swan Mainnet credits your account with a **20% bonus on top of the USD value** — $100 of SWAN becomes $120 of credits. Your account balance is a single USD-denominated pool regardless of how it was funded, so there's nothing special to toggle at request time; you simply get more credits per dollar when you deposit SWAN.

Combined with Swan's already-lower per-token pricing, the deposit bonus pushes effective rates roughly 50–66% below going direct to Anthropic or Google for comparable models. Flip the **Pay with: SWAN** toggle on the [pricing page](https://inference.swanchain.io/pricing) to see the effective rate across every model.

<figure><img src="../../.gitbook/assets/inference-how-to/swan-toggle.png" alt="Pay-with-SWAN toggle on pricing page"><figcaption>Flip the Pay-with toggle to SWAN to preview the effective rate after the 20% deposit bonus.</figcaption></figure>

Usage is deducted from your balance per request. View balance, usage, and the transaction ledger under **Billing** in the dashboard.

## 3. Browse models

The [Models page](https://inference.swanchain.io/models) lists every available model with live pricing, context length, and provider count. Click any model for details and code examples.

<figure><img src="../../.gitbook/assets/inference-how-to/models.png" alt="Swan Inference models catalog"><figcaption>Live models catalog.</figcaption></figure>

The [Pricing page](https://inference.swanchain.io/pricing) compares SwanChain's rates side-by-side against Anthropic, Google, OpenRouter, and other providers for hero models — so you can see how prices stack up at a glance.

<figure><img src="../../.gitbook/assets/inference-how-to/pricing.png" alt="Swan Inference pricing comparison"><figcaption>Pricing page with competitor comparison.</figcaption></figure>

## 4. Make your first inference request

Swan Inference is fully OpenAI-compatible — any existing OpenAI SDK or integration works by changing two things: the base URL and the API key. The examples below use `zai-org/GLM-4.7-Flash`, one of the cheapest hero models at $0.05 input / $0.36 output per 1M tokens.

### curl

```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [{"role": "user", "content": "Say hello in 5 words."}]
  }'
```

### OpenAI Python SDK

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-KEY",
)

response = client.chat.completions.create(
    model="zai-org/GLM-4.7-Flash",
    messages=[{"role": "user", "content": "Say hello in 5 words."}],
)
print(response.choices[0].message.content)
```

### OpenAI Node.js SDK

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://inference.swanchain.io/v1",
  apiKey: "sk-swan-YOUR-KEY",
});

const response = await client.chat.completions.create({
  model: "zai-org/GLM-4.7-Flash",
  messages: [{ role: "user", content: "Say hello in 5 words." }],
});
console.log(response.choices[0].message.content);
```

Streaming, embeddings, image generation, and audio transcription all work identically to OpenAI. See [OpenAI-Compatible API](README.md#openai-compatible-api) for the full endpoint list.

## Next steps

- **[Inference Marketplace](../market-provider/inference-marketplace.md)** — deeper on how pricing, routing, and settlement work
- **[Provider onboarding](README.md#provider-onboarding)** — want to earn by sharing GPU resources instead?
- **[API reference](https://inference.swanchain.io/docs)** — full list of endpoints, parameters, and error codes

Questions? Reach the team on [Discord](https://discord.gg/swanchain) or open an issue on [GitHub](https://github.com/swanchain).
