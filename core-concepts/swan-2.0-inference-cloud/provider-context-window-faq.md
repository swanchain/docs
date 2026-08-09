# Provider Notice: Context-Window Integrity FAQ

> **TL;DR:** The context length shown next to your model in the marketplace is
> a promise to users. If your backend actually serves less (a smaller
> `num_ctx` / `--max-model-len`), long requests silently break — and the
> platform detects this. Check your backends today, and upgrade your
> computing-provider so your real window is reported automatically. Honest
> small windows are fully supported; misrepresented ones will be capped and
> can be penalized.

## Why am I reading this?

The marketplace displays each model's **catalog context length** (for example
`131K`) on your Model Offerings page. If your backend really serves less,
routing still assumes the displayed value — and what happens next depends on
your serving engine. Both cases harm users:

* **vLLM / SGLang** — requests longer than `--max-model-len` are rejected with
  HTTP 400. Users get hard errors on prompts the marketplace said were
  supported, and every failure counts against your provider quality stats.
* **Ollama / llama.cpp** — worse: nothing fails visibly. When a prompt exceeds
  `num_ctx`, the runtime **silently discards half the context window** and
  answers from what remains. The request returns HTTP 200 with a confident
  answer computed from half of the user's input. Neither you nor the user gets
  any error signal.

## What should I do right now?

**1. Upgrade your computing-provider.** Recent versions automatically detect
your backend's real context window (from vLLM/SGLang's `/v1/models`
`max_model_len`) and report it to Swan Inference at registration and on every
heartbeat. Once reported, the marketplace displays *your* real window and
routing only sends you requests that fit it. For engines that don't expose a
window (such as Ollama), set it manually in `models.json`:

```json
{
  "TheDrummer/Cydonia-24B-v4.3": {
    "endpoint": "http://localhost:11434",
    "gpu_memory": 16000,
    "category": "text-generation",
    "local_model": "cydonia:24b",
    "context_length": 32768
  }
}
```

**2. Check every backend's actual setting:**

| Engine | Setting to check |
| --- | --- |
| vLLM | `--max-model-len` |
| SGLang | `--context-length` |
| Ollama | `num_ctx` (Modelfile) / `OLLAMA_CONTEXT_LENGTH` |

**3. Do the VRAM math before promising a big window.** KV cache is what limits
context. A rough rule for 20–30B models: about 80 KB per token at fp16 KV,
half that with fp8/q8 KV quantization (`--kv-cache-dtype fp8` on vLLM,
`OLLAMA_KV_CACHE_TYPE=q8_0` on Ollama). A 131k window needs roughly 10 GB of
KV headroom *after weights* for a single request. If you don't have that, you
cannot honestly serve 131k — declare what you can actually hold instead.

## How does the platform detect mismatches?

Two mechanisms:

1. **Truncation monitoring.** Your backend reports `usage.prompt_tokens` on
   every response. If it is far below the platform's own token estimate for
   the request, your backend truncated the input. This signature is
   unmistakable.
2. **Context verification challenges.** The audit system can send a
   recall-test prompt sized close to your displayed context: a marker at the
   start of the prompt that the model must repeat at the end. A truncating
   backend cannot pass, because truncation removes the marker.

## What are the consequences?

The enforcement sequence is designed so that honest providers are never hurt:

1. **First documented mismatch** → you receive a notice, and the displayed
   context for your offering is **capped to your measured window**. No
   penalty — users are simply no longer promised what you don't serve.
2. **Continued misrepresentation after notice** → collateral penalty under the
   standard slashing rules, with the standard appeal window.

**Serving a small context honestly is not an offense and never will be.**
Declaring 32k costs you nothing except long-context traffic you couldn't have
served correctly anyway. Only misrepresentation is penalized.

## Questions?

Contact the Swan Inference team via the provider dashboard or the usual
support channels. If you believe a verification result is wrong, the appeal
window applies to every penalty record.
