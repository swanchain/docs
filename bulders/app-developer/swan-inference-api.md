---
description: Call frontier and open-source AI models through one OpenAI-compatible API
---

# Swan Inference API

Swan Inference provides an **OpenAI-compatible REST API** for accessing decentralized AI models. If you've used the OpenAI API or any OpenAI-compatible client, you already know how to use Swan Inference — just change the base URL and API key.

**Base URL**: `https://inference.swanchain.io`

## Quick Start

### 1. Get an API Key

Sign up at [inference.swanchain.io](https://inference.swanchain.io) to get your API key. Keys use the `sk-swan-` prefix.

### 2. Make Your First Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "What is Swan Chain?"}
    ]
  }'
```
{% endtab %}

{% tab title="Python" %}
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
)

response = client.chat.completions.create(
    model="zai-org/GLM-4.7-Flash",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is Swan Chain?"},
    ],
)

print(response.choices[0].message.content)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://inference.swanchain.io/v1",
  apiKey: "sk-swan-YOUR-API-KEY",
});

const response = await client.chat.completions.create({
  model: "zai-org/GLM-4.7-Flash",
  messages: [
    { role: "system", content: "You are a helpful assistant." },
    { role: "user", content: "What is Swan Chain?" },
  ],
});

console.log(response.choices[0].message.content);
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
    "context"
    "fmt"
    openai "github.com/sashabaranov/go-openai"
)

func main() {
    config := openai.DefaultConfig("sk-swan-YOUR-API-KEY")
    config.BaseURL = "https://inference.swanchain.io/v1"
    client := openai.NewClientWithConfig(config)

    resp, err := client.CreateChatCompletion(
        context.Background(),
        openai.ChatCompletionRequest{
            Model: "zai-org/GLM-4.7-Flash",
            Messages: []openai.ChatCompletionMessage{
                {Role: "system", Content: "You are a helpful assistant."},
                {Role: "user", Content: "What is Swan Chain?"},
            },
        },
    )
    if err != nil {
        panic(err)
    }
    fmt.Println(resp.Choices[0].Message.Content)
}
```
{% endtab %}
{% endtabs %}

That's it — any library or tool that supports the OpenAI API format works with Swan Inference.

***

## Try Without an API Key

Swan Inference offers a **public playground** that lets you try AI inference without signing up.

```bash
curl https://inference.swanchain.io/v1/playground/chat \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

No `Authorization` header required. The playground exposes a single small model (currently `zai-org/GLM-4.7-Flash`) — list it with:

```bash
curl https://inference.swanchain.io/v1/playground/models
```

| Limit | Value |
|-------|-------|
| Requests per hour | 5 per IP |
| Max output tokens | 100 |
| Streaming | Not supported |

{% hint style="info" %}
For full access to all models with higher limits, [sign up](https://inference.swanchain.io/signup) for a free account.
{% endhint %}

### Token Plan (Pro subscription)

For steady users of open-source models, the **Pro plan** is a flat **$6/month** (billed monthly by card) that includes a weekly token allowance on **standard-tier** models. Everything else is pay-as-you-go from your credit balance.

| | Pay-as-you-go | Pro ($6/month) |
|---|---|---|
| Standard-tier models | Per token, from credit balance | Included: 40M tokens/week, 1,500 requests/day |
| Premium-tier models (Claude, Gemini Pro, …) | Per token | Per token, from credit balance |
| Image generation | Per image | 75 images/day included |
| Rate limit | Per-category limits below | 50 requests/min, 8 concurrent |
| Payment | Credit balance (card or crypto deposit) | Stripe, monthly |

A model's tier is shown on its catalog page and in the `tier` field of `GET /api/v1/models`. Requests beyond the plan allowance fall back to pay-as-you-go if you have credit, otherwise they are rejected. See the [Token Plan FAQ](https://inference.swanchain.io/pricing) for current terms.

***

## Authentication

All API requests require an API key passed in the `Authorization` header:

```
Authorization: Bearer sk-swan-YOUR-API-KEY
```

| Key Prefix | Purpose |
|------------|---------|
| `sk-swan-*` | Consumer API key — for making inference requests |
| `sk-prov-*` | Provider API key — for GPU providers connecting to the network |

***

## API Endpoints

### List Models

Retrieve all available models and their current status.

```
GET /v1/models
```

```bash
curl https://inference.swanchain.io/v1/models \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY"
```

**Response:**

```json
{
  "object": "list",
  "data": [
    {
      "id": "zai-org/GLM-4.7-Flash",
      "object": "model",
      "owned_by": "swan-inference"
    },
    {
      "id": "anthropic/claude-sonnet-5",
      "object": "model",
      "owned_by": "swan-inference"
    }
  ]
}
```

Model IDs are organisation-prefixed exactly as shown (`zai-org/GLM-4.7-Flash`, `openai/gpt-5.5`, `deepseek-ai/DeepSeek-V3.2`, …) and must be passed verbatim. `GET /v1/models` lists IDs only; for prices, context windows, tier and how many providers are online per model, use the public catalog endpoint — no key required:

```bash
curl "https://inference.swanchain.io/api/v1/models?page_size=100"
```

Each entry carries `input_price` and `output_price` (USD per 1M tokens), `payout_input_price` / `payout_output_price` (what providers are paid), `tier` (`standard` or `premium`), `online_providers`, and `specs.context_length`. The same data is browsable at [inference.swanchain.io/models](https://inference.swanchain.io/models).

***

### Chat Completions

Generate chat-based text responses. This is the primary endpoint for interacting with LLMs.

```
POST /v1/chat/completions
```

**Request Body:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | string | Yes | Model ID (e.g., `zai-org/GLM-4.7-Flash`) |
| `messages` | array | Yes | Array of message objects with `role` and `content` |
| `temperature` | float | No | Sampling temperature (0-2). Default: 1.0 |
| `max_tokens` | integer | No | Maximum tokens to generate. Default: model-dependent |
| `stream` | boolean | No | Enable streaming responses. Default: false |
| `top_p` | float | No | Nucleus sampling threshold. Default: 1.0 |
| `stop` | string/array | No | Stop sequence(s) |
| `frequency_penalty` | float | No | Frequency penalty (-2 to 2). Default: 0 |
| `presence_penalty` | float | No | Presence penalty (-2 to 2). Default: 0 |

**Example — Standard Request:**

```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "meta-llama/Llama-3.2-3B",
    "messages": [
      {"role": "user", "content": "Explain blockchain in one sentence."}
    ],
    "temperature": 0.7,
    "max_tokens": 100
  }'
```

**Response:**

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1709500000,
  "model": "meta-llama/Llama-3.2-3B",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Blockchain is a decentralized, distributed digital ledger that records transactions across many computers so that no single record can be altered retroactively without the alteration of all subsequent blocks."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 12,
    "completion_tokens": 38,
    "total_tokens": 50
  }
}
```

***

### Streaming

Enable real-time token-by-token responses by setting `stream: true`. The response uses **Server-Sent Events (SSE)**.

{% tabs %}
{% tab title="cURL" %}
```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "zai-org/GLM-4.7-Flash",
    "messages": [{"role": "user", "content": "Write a haiku about GPUs."}],
    "stream": true
  }'
```
{% endtab %}

{% tab title="Python" %}
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
)

stream = client.chat.completions.create(
    model="zai-org/GLM-4.7-Flash",
    messages=[{"role": "user", "content": "Write a haiku about GPUs."}],
    stream=True,
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://inference.swanchain.io/v1",
  apiKey: "sk-swan-YOUR-API-KEY",
});

const stream = await client.chat.completions.create({
  model: "zai-org/GLM-4.7-Flash",
  messages: [{ role: "user", content: "Write a haiku about GPUs." }],
  stream: true,
});

for await (const chunk of stream) {
  process.stdout.write(chunk.choices[0]?.delta?.content || "");
}
```
{% endtab %}
{% endtabs %}

**Stream Response Format:**

Each SSE event contains a JSON chunk:

```
data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"delta":{"content":"Silicon"},"index":0}]}

data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","choices":[{"delta":{"content":" hearts"},"index":0}]}

data: [DONE]
```

***

### Embeddings

Generate vector embeddings for text. Useful for search, similarity, and RAG applications.

```
POST /v1/embeddings
```

**Request Body:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | string | Yes | Embedding model ID |
| `input` | string/array | Yes | Text to embed (string or array of strings) |

**Example:**

```bash
curl https://inference.swanchain.io/v1/embeddings \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "BAAI/bge-large-en-v1.5",
    "input": "Swan Chain is a decentralized AI computing blockchain."
  }'
```

**Response:**

```json
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "index": 0,
      "embedding": [0.0023, -0.0091, 0.0152, ...]
    }
  ],
  "model": "BAAI/bge-large-en-v1.5",
  "usage": {
    "prompt_tokens": 10,
    "total_tokens": 10
  }
}
```

***

### Image Generation

Generate images from text prompts.

```
POST /v1/images/generations
```

**Request Body:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | string | Yes | Image model ID (e.g., `black-forest-labs/FLUX.1-schnell`) |
| `prompt` | string | Yes | Text description of the image to generate |
| `n` | integer | No | Number of images to generate. Default: 1 |
| `size` | string | No | Image size (e.g., `1024x1024`) |

**Example:**

```bash
curl https://inference.swanchain.io/v1/images/generations \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "black-forest-labs/FLUX.1-schnell",
    "prompt": "A futuristic data center powered by blockchain, digital art style",
    "n": 1,
    "size": "1024x1024"
  }'
```

**Response:**

```json
{
  "created": 1709500000,
  "data": [
    {
      "url": "https://inference.swanchain.io/images/generated/abc123.png"
    }
  ]
}
```

***

### Audio Transcription

Transcribe audio files to text.

```
POST /v1/audio/transcriptions
```

**Request Body** (multipart/form-data):

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `file` | file | Yes | Audio file (mp3, mp4, wav, webm, etc.) |
| `model` | string | Yes | Audio model ID (e.g., `Systran/faster-whisper-large-v3`) |
| `language` | string | No | Language code (e.g., `en`) |

**Example:**

```bash
curl https://inference.swanchain.io/v1/audio/transcriptions \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -F file="@audio.mp3" \
  -F model="Systran/faster-whisper-large-v3"
```

**Response:**

```json
{
  "text": "Hello, welcome to Swan Chain's decentralized AI inference platform."
}
```

***

## Choosing a Provider

By default Swan routes each request to the healthiest capable provider. To pick one yourself, add a request header:

| Request header | Value | Effect |
|----------------|-------|--------|
| `X-Swan-Provider` | a provider ID | Pins the request to that provider's offering of the model |
| `X-Swan-Allow-Fallbacks` | `true` (default) or `false` | With `false`, an unavailable or failing pinned provider produces an error instead of a fallback. Anything unparseable keeps the default. |

Provider IDs, and what each provider offers for a model, come from the public per-model providers endpoint (URL-encode the `/` in the model ID):

```bash
curl "https://inference.swanchain.io/api/v1/models/zai-org%2FGLM-4.7-Flash/providers"
```

Each offering carries `provider_id`, `provider_name`, `input_price` / `output_price` and `price_source` (`catalog` — providers do not set their own prices), `quantization` and `format` when the provider declared them, `uptime_30d` (absent when there is no evidence yet, never assumed 100%), `ttft_avg_ms` (a mean, named as such), and its context window with provenance — `context_length`, `context_source` (`reported`, `assumed`, `capped`), `reported_context_length`. The same information is on the model page under **Providers**.

```bash
curl https://inference.swanchain.io/v1/chat/completions \
  -H "Authorization: Bearer sk-swan-YOUR-KEY" \
  -H "X-Swan-Provider: <provider-id>" \
  -H "X-Swan-Allow-Fallbacks: false" \
  -H "Content-Type: application/json" \
  -d '{"model": "zai-org/GLM-4.7-Flash", "messages": [{"role": "user", "content": "Hello"}]}'
```

### The receipt

Every response — pinned or not, streaming or not — says how it was routed and billed:

| Response header | Values |
|-----------------|--------|
| `X-Swan-Route-Mode` | `auto` or `explicit` |
| `X-Swan-Requested-Provider` | The provider you asked for (explicit only) |
| `X-Swan-Fallback-Reason` | Empty when your provider served it; otherwise `requested_provider_unavailable` (not online for the model) or `requested_provider_failed` (it errored and another provider served the request) |
| `X-Swan-Billing-Type` | `pay_as_you_go` or `subscription` |
| `X-Swan-Context-Source`, `X-Swan-Context-Length` | The context window the serving provider was admitted with, and whether it was `reported` by that provider or `assumed` from the catalog |

`X-Swan-Provider-ID` still names who actually served the request, so `X-Swan-Requested-Provider` ≠ `X-Swan-Provider-ID` together with a fallback reason is exactly how a fallback shows up.

### Errors

| Status | code | When |
|--------|------|------|
| `400` | `no_fallback_available` | `X-Swan-Allow-Fallbacks: false` and the pinned provider is not online for the model, or the request cannot be served by it |
| `502` | `no_fallback_available` | Streaming with fallbacks disabled, and the pinned provider failed after the stream was accepted |
| `402` | insufficient balance | A pinned request with an empty credit balance — see billing below |

### Billing

**Explicit selection is always pay-as-you-go**, charged from your credit balance at the model's catalog price. This holds for Token Plan subscribers (the plan's weekly allowance is untouched and does not cover the request) and it holds when a fallback serves a pinned request. The receipt says so: `X-Swan-Billing-Type: pay_as_you_go`. A subscriber with an active plan but no credit therefore gets `402` on a pinned request — top up, or drop the header. Rationale and the consumer FAQ: [pricing page](https://inference.swanchain.io/pricing).

### Guaranteed vs maximum context

The model objects in `GET /api/v1/models` state `max_context_length` (the largest window any online provider reports, with `max_context_basis`) and `guaranteed_context_length` (the smallest known window across online providers, with `guaranteed_context_basis` ∈ `reported` | `partial` | `unknown` | `no_online_providers`). Size long-context requests against the guaranteed figure; anything above it depends on which provider you land on.

***

## Supported Models

The catalog spans five categories. It changes often — the [live catalog](https://inference.swanchain.io/models) is authoritative; the examples below are real IDs at the time of writing.

| Category | Endpoint | Examples | Priced |
|----------|----------|----------|--------|
| **LLM** (open-source, community GPUs) | `/v1/chat/completions` | `zai-org/GLM-4.7-Flash`, `deepseek-ai/DeepSeek-V3.2`, `Qwen/Qwen3-Coder-30B-A3B-Instruct`, `TheDrummer/Cydonia-24B-v4.3`, `meta-llama/Llama-4-Scout-17B-16E-Instruct` | Per input + output token |
| **Frontier gateway** (multimodal) | `/v1/chat/completions` | `anthropic/claude-sonnet-5`, `anthropic/claude-opus-4-8`, `openai/gpt-5.5`, `gemini/gemini-3.5-flash`, `moonshotai/Kimi-K2.5` | Per input + output token |
| **Image** | `/v1/images/generations` | `black-forest-labs/FLUX.1-schnell`, `stabilityai/stable-diffusion-xl-base-1.0` | Per image |
| **Audio** | `/v1/audio/transcriptions` | `Systran/faster-whisper-large-v3` | Per minute of audio |
| **Embedding** | `/v1/embeddings` | `BAAI/bge-large-en-v1.5` | Per token |

{% hint style="info" %}
A model is only callable while at least one provider is online for it. `online_providers` in `GET /api/v1/models` and the provider count on each model page tell you that in real time; a request for a model with no provider returns `404`.
{% endhint %}

***

## Rate Limits

Requests are rate-limited per API key, by model category:

| Model Category | Requests per Minute |
|----------------|-------------------|
| LLM | 200 |
| Image | 60 |
| Embedding | 500 |
| Other | 200 |

Free (zero-priced) models are limited to **10 requests/min**. Pro plan requests are limited to **50 requests/min** and **8 concurrent**. Separately, the platform caps **system-wide** concurrency at 100 in-flight requests; when that is reached you receive `503` with a `Retry-After` header rather than a per-key `429`.

When rate-limited, the API returns HTTP `429 Too Many Requests` with a `Retry-After` header.

***

## Request Limits

| Parameter | Limit |
|-----------|-------|
| Max input tokens (LLM) | 128,000 |
| Max output tokens (LLM) | 16,384 |
| Max input tokens (Embedding) | 8,192 |
| Max request body size | 10 MB |
| Max messages per request | 100 |
| Max message length | 100,000 characters |
| Request timeout | 120 seconds |

***

## Error Handling

The API returns standard HTTP error codes with JSON error bodies:

```json
{
  "error": {
    "message": "Invalid API key provided",
    "type": "authentication_error",
    "code": "invalid_api_key"
  }
}
```

| Status Code | Meaning |
|-------------|---------|
| `400` | Bad request — check your request body |
| `401` | Unauthorized — invalid or missing API key |
| `402` | Insufficient balance — top up credits (pay-as-you-go), or the request is outside your Token Plan |
| `404` | Model not found or no providers available |
| `429` | Rate limit exceeded — slow down |
| `500` | Internal server error |
| `503` | Service unavailable — all providers busy |

The platform automatically retries failed requests (up to 2 retries with exponential backoff) when a provider is temporarily unavailable, so most transient errors are handled transparently.

***

## Response Headers

Every inference response says which provider served it. This is how the marketplace stays accountable — a request is never anonymous compute.

| Header | Description |
|--------|-------------|
| `X-Swan-Request-ID` | Unique request ID. Quote it when contacting support or filing an issue. (`X-Request-ID` is also set, with the same value.) |
| `X-Swan-Provider-ID` | ID of the provider that handled the request — the same ID shown on the [network page](https://inference.swanchain.io/network) |
| `X-Swan-Provider-Name` | That provider's display name |
| `X-Swan-Connection-Mode` | How the provider is connected: `websocket` (the `computing-provider` agent) or `external` (a registered OpenAI-compatible endpoint) |
| `X-Swan-Latency-Ms` | End-to-end latency measured by the platform |
| `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` | Rate-limit window for your key and this model category |
| `Retry-After` | Seconds to wait, on `429` and `503` |

Streaming responses carry the same headers on the initial response.

***

## Using with LLM Frameworks

Swan Inference works with any framework that supports OpenAI-compatible APIs.

### LangChain (Python)

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    base_url="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
    model="zai-org/GLM-4.7-Flash",
)

response = llm.invoke("What is decentralized AI?")
print(response.content)
```

### LlamaIndex

```python
from llama_index.llms.openai_like import OpenAILike

llm = OpenAILike(
    api_base="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
    model="zai-org/GLM-4.7-Flash",
)

response = llm.complete("Explain DePIN in simple terms.")
print(response.text)
```

### LiteLLM

```python
import litellm

response = litellm.completion(
    model="openai/zai-org/GLM-4.7-Flash",
    messages=[{"role": "user", "content": "Hello!"}],
    api_base="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
)

print(response.choices[0].message.content)
```

### Vercel AI SDK (TypeScript)

```typescript
import { createOpenAI } from "@ai-sdk/openai";
import { generateText } from "ai";

const swan = createOpenAI({
  baseURL: "https://inference.swanchain.io/v1",
  apiKey: "sk-swan-YOUR-API-KEY",
});

const { text } = await generateText({
  model: swan("zai-org/GLM-4.7-Flash"),
  prompt: "What is Swan Chain?",
});

console.log(text);
```

***

## Pricing

All prices are in **USD per 1M tokens** (per image for image models, per minute for audio) and are deducted from your credit balance. Your balance is one USD pool however you funded it — card, USDC, USDT or SWAN.

| Category | Pricing unit |
|----------|-------------|
| **LLM / frontier** | Per input token + per output token |
| **Embedding** | Per input token |
| **Image** | Per image |
| **Audio** | Per minute of audio |

Every model publishes **two** prices: what you pay and what the serving provider is paid (`payout_*` in the catalog API). The platform keeps the spread; there is no separate percentage fee added to your bill. Current prices for each model are at [inference.swanchain.io/models](https://inference.swanchain.io/models), and the [pricing page](https://inference.swanchain.io/pricing) compares hero models against other gateways.

Token usage is included in every response under the `usage` field.

***

## Network Stats

Public endpoints are available for monitoring network health:

| Endpoint | Description |
|----------|-------------|
| `GET /api/v1/stats/network` | Aggregate network stats (providers, requests, capacity) |
| `GET /api/v1/stats/leaderboard` | Provider leaderboard ranked by performance |
| `GET /api/v1/stats/gpu` | GPU distribution and VRAM capacity across the network |
| `GET /api/v1/stats/utilization` | Network utilization metrics |
| `GET /api/v1/stats/model-demand` | Model demand data (useful for providers choosing which models to serve) |
| `GET /api/v1/dashboard/summary` | Dashboard summary with request and capacity metrics |

These endpoints do not require authentication.

***

## Learn More

- **[For Developers](../../core-concepts/swan-2.0-inference-cloud/how-to-use.md)** — signup, credits and first request, with screenshots
- **[AI Agent Integrations](claw-tools-integration.md)** — configuring OpenClaw, Nanobot and other agents to use Swan Inference
- **[Swan 2.0: Inference Cloud](../../core-concepts/swan-2.0-inference-cloud/README.md)** — Architecture and platform overview
- **[Inference Marketplace](../../core-concepts/market-provider/inference-marketplace.md)** — How the marketplace works (routing, pricing, settlement)
- **[Model Catalog](https://inference.swanchain.io/models)** — Browse all available models with real-time availability and pricing
