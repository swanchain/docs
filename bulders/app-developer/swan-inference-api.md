---
description: Call 42+ AI Models via an OpenAI-Compatible API
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
    "model": "deepseek-r1-distill-llama-70b",
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
    model="deepseek-r1-distill-llama-70b",
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
  model: "deepseek-r1-distill-llama-70b",
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
            Model: "deepseek-r1-distill-llama-70b",
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
      "id": "deepseek-r1-distill-llama-70b",
      "object": "model",
      "owned_by": "swan-inference"
    },
    {
      "id": "llama-3.2-3b",
      "object": "model",
      "owned_by": "swan-inference"
    }
  ]
}
```

You can also browse the full model catalog with pricing at [inference.swanchain.io/models](https://inference.swanchain.io/models).

***

### Chat Completions

Generate chat-based text responses. This is the primary endpoint for interacting with LLMs.

```
POST /v1/chat/completions
```

**Request Body:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | string | Yes | Model ID (e.g., `deepseek-r1-distill-llama-70b`) |
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
    "model": "llama-3.2-3b",
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
  "model": "llama-3.2-3b",
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
    "model": "deepseek-r1-distill-llama-70b",
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
    model="deepseek-r1-distill-llama-70b",
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
  model: "deepseek-r1-distill-llama-70b",
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
    "model": "bge-large-en-v1.5",
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
  "model": "bge-large-en-v1.5",
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
| `model` | string | Yes | Image model ID (e.g., `flux-1-schnell`) |
| `prompt` | string | Yes | Text description of the image to generate |
| `n` | integer | No | Number of images to generate. Default: 1 |
| `size` | string | No | Image size (e.g., `1024x1024`) |

**Example:**

```bash
curl https://inference.swanchain.io/v1/images/generations \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "flux-1-schnell",
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
| `model` | string | Yes | Audio model ID (e.g., `whisper-large-v3`) |
| `language` | string | No | Language code (e.g., `en`) |

**Example:**

```bash
curl https://inference.swanchain.io/v1/audio/transcriptions \
  -H "Authorization: Bearer sk-swan-YOUR-API-KEY" \
  -F file="@audio.mp3" \
  -F model="whisper-large-v3"
```

**Response:**

```json
{
  "text": "Hello, welcome to Swan Chain's decentralized AI inference platform."
}
```

***

## Supported Models

Swan Inference hosts **42+ models** across five categories:

| Category | Models | Pricing |
|----------|--------|---------|
| **LLM** | DeepSeek R1 (70B), Llama 3 (3B, 8B, 70B), Qwen 2.5, Mistral, Phi-3 | Per input/output token |
| **Image** | FLUX.1 Schnell, Stable Diffusion XL | Per request |
| **Audio** | Whisper Large V3 | Per request |
| **Embedding** | BGE Large, E5 Large | Per token |
| **Multimodal** | Llama 3.2 Vision, Qwen-VL | Per token |

{% hint style="info" %}
Model availability depends on online providers. Check real-time status at [inference.swanchain.io/models](https://inference.swanchain.io/models) or call `GET /v1/models`.
{% endhint %}

***

## Rate Limits

Requests are rate-limited per API key:

| Model Category | Requests per Minute |
|----------------|-------------------|
| LLM | 200 |
| Image | 60 |
| Embedding | 500 |
| Other | 200 |

Maximum concurrent requests: **100** per API key.

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
| `404` | Model not found or no providers available |
| `429` | Rate limit exceeded — slow down |
| `500` | Internal server error |
| `503` | Service unavailable — all providers busy |

The platform automatically retries failed requests (up to 2 retries with exponential backoff) when a provider is temporarily unavailable, so most transient errors are handled transparently.

***

## Response Headers

Swan Inference includes helpful headers in every response:

| Header | Description |
|--------|-------------|
| `X-Request-ID` | Unique request correlation ID for tracing |
| `X-Swan-Connection-Mode` | How the request was routed: `websocket` or `external` |

Use `X-Request-ID` when contacting support or debugging request issues.

***

## Using with LLM Frameworks

Swan Inference works with any framework that supports OpenAI-compatible APIs.

### LangChain (Python)

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    base_url="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
    model="deepseek-r1-distill-llama-70b",
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
    model="deepseek-r1-distill-llama-70b",
)

response = llm.complete("Explain DePIN in simple terms.")
print(response.text)
```

### LiteLLM

```python
import litellm

response = litellm.completion(
    model="openai/deepseek-r1-distill-llama-70b",
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
  model: swan("deepseek-r1-distill-llama-70b"),
  prompt: "What is Swan Chain?",
});

console.log(text);
```

***

## Pricing

| Category | Pricing Unit | Billed In |
|----------|-------------|-----------|
| **LLM** | Per input token + per output token | USDC |
| **Embedding** | Per token | USDC |
| **Image** | Per request | USDC |
| **Audio** | Per request | USDC |

View current pricing for each model at [inference.swanchain.io/models](https://inference.swanchain.io/models).

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

- **[Swan 2.0: Inference Cloud](../../core-concepts/swan-2.0-inference-cloud.md)** — Architecture and platform overview
- **[Inference Marketplace](../../core-concepts/market-provider/inference-marketplace.md)** — How the marketplace works (routing, pricing, settlement)
- **[Model Catalog](https://inference.swanchain.io/models)** — Browse all available models with real-time availability and pricing
