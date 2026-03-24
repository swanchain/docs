---
description: Connect OpenClaw, ZeroClaw, Nanobot, and other Claw-family AI agents to Swan Inference
---

# Claw-Family AI Agent Integration

Swan Inference provides an OpenAI-compatible API that works with all Claw-family AI agent tools. This guide covers how to connect each tool to Swan Inference for decentralized AI inference.

## Prerequisites

1. A Swan Inference API key (`sk-swan-*`) — [sign up here](https://inference.swanchain.io/signup)
2. The Claw tool of your choice installed

## Quick Start

All Claw-family tools support custom OpenAI-compatible endpoints. The core configuration is the same across all tools:

| Setting | Value |
|---------|-------|
| Base URL | `https://inference.swanchain.io/v1` |
| API Key | `sk-swan-YOUR-API-KEY` |
| Model | Any model from [inference.swanchain.io/models](https://inference.swanchain.io/models) |

Popular models available on Swan Inference:

| Model | Category | Use Case |
|-------|----------|----------|
| `deepseek-r1-distill-llama-70b` | LLM | Reasoning, code generation |
| `Qwen/Qwen2.5-7B-Instruct` | LLM | General chat, fast responses |
| `meta-llama/Llama-3.3-70B-Instruct` | LLM | General purpose |

***

## Tool-Specific Setup

### OpenClaw (TypeScript, 250K+ stars)

The most popular AI assistant in the Claw family. Supports 25+ messaging platforms.

{% tabs %}
{% tab title="Config File" %}

Edit your OpenClaw configuration to add Swan Inference as a provider:

```json
{
  "models": {
    "providers": {
      "swan": {
        "api": "openai-completions",
        "baseUrl": "https://inference.swanchain.io/v1",
        "apiKey": "sk-swan-YOUR-API-KEY",
        "models": [
          "deepseek-r1-distill-llama-70b",
          "Qwen/Qwen2.5-7B-Instruct"
        ]
      }
    },
    "default": "swan:deepseek-r1-distill-llama-70b"
  }
}
```

{% endtab %}
{% tab title="Environment Variables" %}

```bash
export OPENAI_API_BASE=https://inference.swanchain.io/v1
export OPENAI_API_KEY=sk-swan-YOUR-API-KEY
openclaw
```

{% endtab %}
{% endtabs %}

***

### ZeroClaw (Rust, 28K+ stars)

Ultra-lightweight (3.4MB binary, <10ms startup). Best for quick testing.

{% tabs %}
{% tab title="config.toml" %}

```toml
[provider]
name = "openai-compatible"
base_url = "https://inference.swanchain.io/v1"
api_key = "sk-swan-YOUR-API-KEY"
model = "deepseek-r1-distill-llama-70b"
```

{% endtab %}
{% tab title="CLI" %}

```bash
zeroclaw --provider "custom:https://inference.swanchain.io/v1" \
         --api-key "sk-swan-YOUR-API-KEY" \
         --model "deepseek-r1-distill-llama-70b"
```

{% endtab %}
{% endtabs %}

***

### PicoClaw (Go, 26K+ stars)

Lightweight Go binary (<8MB). Designed for edge and ARM devices.

```yaml
# config.yaml
provider:
  type: openai-compatible
  base_url: https://inference.swanchain.io/v1
  api_key: sk-swan-YOUR-API-KEY
  model: deepseek-r1-distill-llama-70b
```

***

### Nanobot (Python, 32K+ stars)

Python-native, installable via pip. Best for Python developers.

{% tabs %}
{% tab title="Install" %}

```bash
pip install nanobot-ai
```

{% endtab %}
{% tab title="Config" %}

```json
{
  "provider": "custom",
  "apiBase": "https://inference.swanchain.io/v1",
  "apiKey": "sk-swan-YOUR-API-KEY",
  "model": "deepseek-r1-distill-llama-70b"
}
```

{% endtab %}
{% tab title="Python API" %}

```python
from nanobot import Agent

agent = Agent(
    provider="custom",
    api_base="https://inference.swanchain.io/v1",
    api_key="sk-swan-YOUR-API-KEY",
    model="deepseek-r1-distill-llama-70b",
)

response = agent.chat("Explain quantum computing in simple terms")
print(response)
```

{% endtab %}
{% endtabs %}

***

### NanoClaw (TypeScript, 25K+ stars)

Container-per-session isolation. Good for multi-tenant deployments.

```json
{
  "llm": {
    "provider": "openai-compatible",
    "baseUrl": "https://inference.swanchain.io/v1",
    "apiKey": "sk-swan-YOUR-API-KEY",
    "model": "deepseek-r1-distill-llama-70b"
  }
}
```

***

### IronClaw (Rust, 10K+ stars)

Security-focused with TEE and encrypted vault. Built by NEAR AI.

```bash
export LLM_BACKEND=openai_compatible
export LLM_BASE_URL=https://inference.swanchain.io/v1
export API_KEY=sk-swan-YOUR-API-KEY
export MODEL=deepseek-r1-distill-llama-70b

ironclaw
```

***

### NemoClaw (TypeScript + Python, 15K+ stars)

NVIDIA's reference stack with kernel sandbox and privacy router.

Configure the inference provider in your NemoClaw deployment:

```yaml
inference:
  provider: openai-compatible
  base_url: https://inference.swanchain.io/v1
  api_key: sk-swan-YOUR-API-KEY
  model: deepseek-r1-distill-llama-70b
```

{% hint style="info" %}
NemoClaw is in early preview (alpha). Configuration may change.
{% endhint %}

***

### Moltworker (TypeScript, Cloudflare Workers)

Runs on Cloudflare's edge network (330+ cities).

```json
{
  "models": {
    "providers": {
      "swan": {
        "type": "openai-compatible",
        "baseUrl": "https://inference.swanchain.io/v1",
        "apiKey": "sk-swan-YOUR-API-KEY"
      }
    }
  }
}
```

Or set via Cloudflare AI Gateway:

```bash
AI_GATEWAY_BASE_URL=https://inference.swanchain.io/v1
```

***

### NullClaw (Zig, 6.7K+ stars)

Ultra-minimal (678KB binary, 1MB RAM). For IoT and embedded devices.

```bash
nullclaw --provider "custom:https://inference.swanchain.io/v1" \
         --api-key "sk-swan-YOUR-API-KEY" \
         --model "Qwen/Qwen2.5-7B-Instruct"
```

Or in config:

```toml
[provider]
url = "custom:https://inference.swanchain.io/v1"
api_key = "sk-swan-YOUR-API-KEY"
model = "Qwen/Qwen2.5-7B-Instruct"
```

{% hint style="info" %}
For resource-constrained devices, use smaller models like `Qwen/Qwen2.5-7B-Instruct` for faster responses.
{% endhint %}

***

## Choosing the Right Tool

| Scenario | Recommended Tool | Why |
|----------|-----------------|-----|
| Quick local testing | **ZeroClaw** | 3.4MB, boots in <10ms |
| Python project | **Nanobot** | pip install, native Python API |
| Multi-platform bot | **OpenClaw** | 25+ messaging platforms |
| Edge / ARM device | **PicoClaw** | 8MB Go binary, runs anywhere |
| IoT / embedded | **NullClaw** | 678KB, runs on $5 hardware |
| Security-critical | **IronClaw** | TEE, encrypted vault, WASM sandbox |
| Multi-tenant SaaS | **NanoClaw** | Container-per-session isolation |
| Enterprise / NVIDIA | **NemoClaw** | Kernel sandbox, policy enforcement |
| Global edge deploy | **Moltworker** | Cloudflare Workers, 330+ cities |

## Playground (No API Key)

All tools can also use Swan Inference's public playground for testing without an API key:

```
Base URL: https://inference.swanchain.io/v1/playground
```

The playground is rate-limited (5 requests/hour per IP) with restricted models, but requires no signup.

## Troubleshooting

### Connection refused / timeout
- Verify the base URL includes `/v1`: `https://inference.swanchain.io/v1`
- Check your API key starts with `sk-swan-`

### Model not found
- List available models: `curl https://inference.swanchain.io/v1/models -H "Authorization: Bearer sk-swan-YOUR-KEY"`
- Model IDs are case-sensitive (e.g., `Qwen/Qwen2.5-7B-Instruct`, not `qwen2.5-7b`)

### Rate limited (429)
- Default rate limit is 200 requests/min for LLM models
- Check `X-RateLimit-Remaining` header in responses
- Consider upgrading to Pro subscription ($6/month) for higher limits

### Streaming not working
- Ensure your tool is configured for streaming (`stream: true`)
- Swan Inference supports SSE streaming on `/v1/chat/completions`

## Learn More

- [Swan Inference API Reference](swan-inference-api.md)
- [Available Models](https://inference.swanchain.io/models)
- [Sign Up](https://inference.swanchain.io/signup)
- [Subscription Plans](swan-inference-api.md#subscription-plan)
