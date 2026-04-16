---
description: Step-by-step guide to becoming a GPU Computing Provider on Swan 2.0 Inference Cloud
---

# Become a Provider

This guide walks through turning your GPU into an AI inference endpoint on Swan Chain — from signing up for a provider account, to installing the `computing-provider` agent, to earning stablecoin revenue from real inference traffic.

{% hint style="info" %}
Looking to **consume** models instead of provide? See [How to Use Swan Inference](how-to-use.md).

For hardware tiers, collateral economics, revenue splits, and slashing rules, see the [Provider Onboarding](README.md#provider-onboarding) section of the Swan 2.0 overview. This page focuses on the hands-on setup.
{% endhint %}

## 0. Check prerequisites

Providers connect **outbound** to Swan Inference over WebSocket — **no public IP, domain, or SSL setup is required**. You just need a capable GPU and one of two supported OS/inference-engine stacks:

| Platform | Minimum hardware | Inference engine |
|----------|-----------------|-----------------|
| **Linux (NVIDIA)** | GPU with ≥ 8 GB VRAM (Tier C); 24 GB+ recommended (Tier A) | [SGLang](https://github.com/sgl-project/sglang) (recommended), vLLM, or Ollama |
| **macOS (Apple Silicon)** | M1/M2/M3/M4 with ≥ 16 GB unified memory | [Ollama](https://ollama.com) |

Legacy GPUs (TESLA P4, GTX 1050 Ti, anything < 8 GB VRAM) cannot serve modern inference workloads and will not receive traffic. Full tier-to-model mapping is in [Hardware Tiers](README.md#hardware-tiers).

You'll also need:

- **Go 1.22+** to build the `computing-provider` agent
- **Docker 24.0+ with the NVIDIA Container Toolkit** (Linux only)
- A funded wallet or credit card for collateral (step 6)

## 1. Sign up and get your provider API key

Create a provider account at [inference.swanchain.io/provider-signup](https://inference.swanchain.io/provider-signup). You'll need an email, password, and the wallet address you want rewards paid to (the **beneficiary address**).

<figure><img src="../../.gitbook/assets/provider-how-to/provider-signup.png" alt="Provider signup form"><figcaption>Sign up as a provider — supply a display name and beneficiary wallet address.</figcaption></figure>

After signup, copy your provider API key (`sk-prov-*`) from the dashboard. **Save it immediately** — it's shown once and authenticates your GPU node to the network.

<figure><img src="../../.gitbook/assets/provider-how-to/provider-api-key.png" alt="Provider API key in dashboard"><figcaption>Your provider API key appears under the Provider section of the dashboard.</figcaption></figure>

{% hint style="info" %}
Consumer keys (`sk-swan-*`) and provider keys (`sk-prov-*`) are different. The `computing-provider` agent only accepts `sk-prov-*` keys.
{% endhint %}

## 2. Start a model server

Your GPU needs an OpenAI-compatible inference server running locally. Swan Inference will route requests to it via the `computing-provider` agent.

### Linux (NVIDIA) — SGLang

```bash
# Download model weights from HuggingFace
computing-provider models download Qwen/Qwen2.5-7B-Instruct

# Start SGLang serving the model on port 30000
docker run -d --gpus all -p 30000:30000 --ipc=host --name sglang \
  -v ~/.swan/models/Qwen/Qwen2.5-7B-Instruct:/models \
  lmsysorg/sglang:latest \
  python3 -m sglang.launch_server --model-path /models \
    --host 0.0.0.0 --port 30000 \
    --served-model-name Qwen/Qwen2.5-7B-Instruct
```

Verify it's healthy: `curl http://localhost:30000/v1/models`.

### macOS (Apple Silicon) — Ollama

```bash
brew install ollama
ollama serve &
ollama pull qwen2.5:7b
```

Verify it's healthy: `curl http://localhost:11434/api/tags`.

{% hint style="info" %}
The quickstart uses Qwen 2.5 7B as an example, but earnings scale with real token traffic. Browse the [model catalog](https://inference.swanchain.io/models) to find in-demand models with less provider competition.
{% endhint %}

## 3. Install the computing-provider agent

Clone and build from source (mainnet):

```bash
git clone https://github.com/swanchain/computing-provider.git
cd computing-provider
make clean && make mainnet && sudo make install

# Verify
computing-provider --version
```

Full install details including the NVIDIA Container Toolkit setup are in the [`computing-provider` README](https://github.com/swanchain/computing-provider#readme).

## 4. Run the setup wizard

The wizard auto-discovers your running model server, logs you in, and writes `config.toml` and `models.json`:

```bash
computing-provider setup
```

<figure><img src="../../.gitbook/assets/provider-how-to/provider-guide.png" alt="In-app provider guide with setup commands"><figcaption>The in-app <a href="https://inference.swanchain.io/provider-guide">Provider Guide</a> mirrors these steps with copy-paste commands for Linux and macOS.</figcaption></figure>

If you already have a `sk-prov-*` key, pass it directly:

```bash
computing-provider setup --api-key=sk-prov-xxxxxxxxxxxx
```

Config files land in `~/.swan/computing/`:

- `config.toml` — WebSocket URL, API key, node name
- `models.json` — mapping from Swan Inference model IDs to your local endpoints

## 5. Start the provider and pass benchmarks

Run the agent:

```bash
nohup computing-provider run >> cp.log 2>&1 &
```

Then check your status:

```bash
computing-provider inference status
```

You'll move through these stages automatically:

```
Connect ──▶ Benchmark ──▶ Approval ──▶ Collateral ──▶ Active
(instant)   (automatic)   (< 24 hrs)   (see step 6)    (earning)
```

| Stage | What happens | Typical duration |
|-------|-------------|-----------------|
| **Connect** | Agent opens a WebSocket to Swan Inference and registers your models | Instant |
| **Benchmark** | Automated math / code / latency checks verify inference quality | Minutes |
| **Approval** | Admin reviews your provider record | < 24 hours |
| **Collateral** | Deposit via Stripe or on-chain SWAN (step 6) | Instant |
| **Active** | Traffic starts flowing — you earn per-request revenue | Ongoing |

<figure><img src="../../.gitbook/assets/provider-how-to/provider-status.png" alt="Provider activation stages shown in the dashboard"><figcaption>The My Provider tab visualizes the activation flow: Start → Connect → Deposit Collateral → Approved → Active & Earning.</figcaption></figure>

## 6. Deposit collateral

Once approved, deposit collateral to unlock full traffic routing. Two options:

| Method | Currency | Processing | Refund |
|--------|----------|-----------|--------|
| **Stripe** | Credit/debit card (USD) | Instant | 7-day waiting period, back to original card |
| **On-chain** | SWAN tokens on Swan Mainnet | Requires SwanETH gas | 7-day waiting period, back to your wallet |

```bash
# Show instructions for your account (deposit address, minimum amount)
computing-provider inference deposit

# Verify deposit was seen on-chain
computing-provider inference deposit --check
```

<figure><img src="../../.gitbook/assets/provider-how-to/deposit-collateral.png" alt="Provider collateral deposit panel"><figcaption>Provider dashboard's Collateral Deposit panel — verify your wallet, then pay the required amount via Stripe or on-chain crypto.</figcaption></figure>

Collateral amounts scale with hardware tier and earning multiplier. See [Computing Provider Collateral](../token/computing-provider-collateral/) for the full table.

## 7. Monitor earnings and uptime

The Provider dashboard at [inference.swanchain.io/dashboard](https://inference.swanchain.io/dashboard) shows live earnings, request counts, and benchmark history.

<figure><img src="../../.gitbook/assets/provider-how-to/earnings-dashboard.png" alt="Provider earnings dashboard"><figcaption>Earnings dashboard with live request volume, per-model breakdown, and payout history.</figcaption></figure>

For a local view, the agent ships its own web dashboard:

```bash
computing-provider dashboard
# → http://localhost:3005
```

Set where payouts go:

```bash
computing-provider inference set-beneficiary 0xYourWalletAddress
```

{% hint style="info" %}
**New Provider Grace Period:** For the first 7 days after activation, uptime and success-rate deprioritization are waived. Use this window to stabilize your setup before full routing weight kicks in.
{% endhint %}

## Switching or adding models

Edit `~/.swan/computing/models.json` — the agent watches this file and hot-reloads without restarting. Start additional model servers on different ports and add them all to the JSON. Full walkthrough with multi-GPU pinning is in the [`computing-provider` README](https://github.com/swanchain/computing-provider#switching-models).

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `invalid provider API key` | Verify key starts with `sk-prov-` and check `ApiKey` in `~/.swan/computing/config.toml` |
| `WebSocket connection failed` | Confirm outbound port 443 is open; URL must be `wss://` not `http://` |
| Provider online but no requests | Model name mismatch — `--served-model-name` must exactly match the key in `models.json` and a model ID in the [catalog](https://inference.swanchain.io/models) |
| `could not select device driver "nvidia"` | Install the NVIDIA Container Toolkit; see [`computing-provider` README](https://github.com/swanchain/computing-provider#install-nvidia-container-toolkit) |
| Stuck in `pending` | Provider needs collateral + passing benchmark + hardware check. Run `computing-provider inference status` to see which condition is missing |

Full troubleshooting catalog: [`computing-provider` README — FAQ](https://github.com/swanchain/computing-provider#faq).

## Next steps

- **[Provider Onboarding](README.md#provider-onboarding)** — hardware tiers, revenue split, slashing rules
- **[Computing Provider Income](../token/swan-provider-income.md)** — contribution score formula and reward distribution
- **[Computing Provider Collateral](../token/computing-provider-collateral/)** — required amounts and refund process
- **[Inference Marketplace](../market-provider/inference-marketplace.md)** — how pricing, routing, and settlement work under the hood

Questions? Reach the team on [Discord](https://discord.gg/swanchain) or open an issue on the [`computing-provider` repo](https://github.com/swanchain/computing-provider/issues).
