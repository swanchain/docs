# SillyTavern and Janitor AI

Swan Inference is an OpenAI-compatible API, so any frontend that can point at a custom OpenAI endpoint works — including the two most common roleplay clients. You bring your own key and pay per token; there is no separate subscription for either app.

**What you need first**

1. An API key from [inference.swanchain.io](https://inference.swanchain.io) → **API Keys** (`sk-swan-…`).
2. Credit on the account, or an active Token Plan. See [pricing](https://inference.swanchain.io/pricing).
3. A model ID. Browse [the model catalog](https://inference.swanchain.io/models), or list what is servable right now:

```bash
curl "https://inference.swanchain.io/v1/models?available=true"
```

Popular choices for character chat include `BruhzWater/Sapphira-L3.3-70b-0.1`, `Steelskull/L3.3-MS-Nevoria-70b`, and `TheDrummer/Cydonia-24B-v4.3`. Copy the `id` exactly — it is case-sensitive and contains a slash.

***

## SillyTavern

SillyTavern runs on your own machine and talks to Swan from its local server.

1. Open the **API Connections** panel (the plug icon).
2. **API**: `Chat Completion`.
3. **Chat Completion Source**: `Custom (OpenAI-compatible)`.
4. **Custom Endpoint (Base URL)**:

   ```
   https://inference.swanchain.io/v1
   ```

   Include `/v1`, and nothing after it — SillyTavern appends `/chat/completions` itself.
5. **Custom API Key**: your `sk-swan-…` key.
6. Click **Connect**. The **Available Models** dropdown fills from the catalog; pick your model.

If the dropdown stays empty, the base URL is usually the culprit — a trailing `/chat/completions` or a missing `/v1` both produce exactly that symptom.

**Settings that matter**

- **Context size**: set it to the model's window, not above. Each model page lists a *guaranteed* context (the smallest window across online providers) and a *maximum*. Sizing to the guaranteed figure means any provider can serve you; going above it means only some can.
- **Streaming**: supported — leave it on for token-by-token output.
- Temperature, top_p, frequency/presence penalty, and stop sequences are all passed through to the provider.

***

## Janitor AI

Janitor AI runs in your browser and calls the API directly from it, using its proxy configuration.

1. Open any character, then **API Settings** (or the ⚙ icon in chat).
2. Choose the **Proxy** / custom-API option.
3. **Proxy URL** — Janitor wants the *full* completions URL, unlike SillyTavern:

   ```
   https://inference.swanchain.io/v1/chat/completions
   ```

4. **API Key**: your `sk-swan-…` key.
5. **Model**: type the model ID exactly, e.g. `TheDrummer/Cydonia-24B-v4.3`. Janitor does not fetch the catalog for you.
6. Save, then send a test message.

{% hint style="info" %}
Because Janitor AI sends requests from your browser, your key travels from your own machine to Swan — it is not stored on Janitor's servers. Treat the key like a password anyway: use a separate key for each app, and revoke it from the dashboard if you paste it somewhere you did not intend.
{% endhint %}

***

## Troubleshooting

| Symptom | Cause |
|---|---|
| `401` / "invalid API key" | Key is missing, mistyped, or revoked. Keys start `sk-swan-`. |
| `402` / "insufficient balance" | No credit. Top up, or check whether your Token Plan covers this model — plans do not cover requests that name a specific provider. |
| `404` / "model does not exist" | Model ID wrong. It is case-sensitive and includes the org prefix (`TheDrummer/Cydonia-24B-v4.3`, not `Cydonia-24B-v4.3`). |
| `429` | Rate limit. Wait for the window in the `Retry-After` header. |
| `503` / "no providers" | No provider is currently serving that model. Check `?available=true` and pick another. |
| Empty model list in SillyTavern | Base URL should end at `/v1` — no `/chat/completions`. |
| Replies cut off mid-sentence | Response-length setting is low, or context + response exceeds the model's window. |

## Which model?

Every model's page on [inference.swanchain.io/models](https://inference.swanchain.io/models) shows its price per million tokens, its context window and where that figure comes from, and which providers are serving it right now. Prices are set per model — whichever provider ends up serving your request, you pay the same, so a busy model failing over to a second provider never costs more.
