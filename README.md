# OpenClaw Starter Configs

A collection of working reference configs for getting OpenClaw up and running. Each file covers a different setup scenario. Pick the one that matches how you want to run your AI model and go from there.

All sensitive values have been replaced with clearly labeled placeholders like `__YOUR_TELEGRAM_BOT_TOKEN__` — swap those out with your real values before using.

---

## Configs

### `configs/api-chatgpt.json` — ChatGPT via API
Uses OpenAI's API directly with a paid API key. Good if you want the latest GPT models and are comfortable paying per token. Connects to Telegram as the messaging interface.

**You need:**
- An OpenAI API key
- A Telegram bot token (from @BotFather)
- A gateway token (you create this yourself — any strong random string)

---

### `configs/local-llama.json` — Local LLM (Llama 3.1 8B via Ollama)
Runs a smaller local model using [Ollama](https://ollama.com). Llama 3.1 8B is lightweight, fast, and runs well on most modern computers with 8GB+ RAM. No API key needed — everything stays on your machine.

**You need:**
- [Ollama](https://ollama.com) installed and running
- Llama 3.1 pulled: `ollama pull llama3.1:8b`
- A gateway token

**Best for:** Everyday tasks, privacy-first setups, lower-end hardware.

---

### `configs/local-qwen.json` — Local LLM (Qwen 2.5 7B via Ollama)
Same local setup as the Llama config but using Qwen 2.5 7B-Instruct. Qwen is a strong alternative to Llama — particularly good at instruction following and multilingual tasks. Also runs via Ollama with no API key required.

**You need:**
- [Ollama](https://ollama.com) installed and running
- Qwen pulled: `ollama pull qwen2.5:7b-instruct`
- A gateway token

**Best for:** Users who want a local alternative to Llama, or multilingual use cases.

---

### `configs/oauth-chatgpt.json` — OAuth ChatGPT with Ollama Fallback
Uses ChatGPT via OAuth (no API key required — logs in through your existing ChatGPT account). Falls back to a local Qwen model via Ollama if the primary model is unavailable. This is the most resilient setup.

**You need:**
- A ChatGPT account (free or paid)
- [Ollama](https://ollama.com) installed with Qwen pulled (for fallback)
- A Telegram bot token
- A gateway token

**Best for:** Users who want ChatGPT quality without paying for API tokens, with a local fallback.

---

## How to Use

1. Copy the config file that matches your setup
2. Replace all `__PLACEHOLDER__` values with your real credentials
3. Update the `workspace` path to match your machine (replace `YOUR_USERNAME` with your actual username)
4. Drop it into your OpenClaw config directory and restart OpenClaw

For step-by-step install help, check out the [OpenClaw guide on SCRYPTO](https://scrypto.one) or use the OpenClaw Install Helper agent.

---

## Resources

- [OpenClaw](https://openclaw.ai) — official site and documentation
- [Ollama](https://ollama.com) — run local AI models
- [SCRYPTO](https://scrypto.one) — community hub where these configs came from
