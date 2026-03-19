# OpenClaw Starter Kit

A collection of working reference configs, project templates, and ticket workflows for getting OpenClaw up and running. Pick the config that matches your setup, use the templates to structure your projects, and track work with tickets.

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
2. Manually paste your real values over every `__PLACEHOLDER__` — see the token guide below
3. Update the `workspace` path to match your machine (replace `YOUR_USERNAME` with your actual system username)
4. Drop it into your OpenClaw config directory and restart OpenClaw

For step-by-step install help, check out the [OpenClaw guide on SCRYPTO](https://scrypto.one) or use the OpenClaw Install Helper agent.

---

## Where to Find Each Token

### `__YOUR_GATEWAY_TOKEN__`
**You make this up yourself.** It's not from any service — it's just a password that OpenClaw uses to protect its local gateway. Create any long random string and paste it in. Example:

```
mySuperSecretGateway2025!
```

Or use a password manager to generate one. Just keep it consistent — whatever you put here is what OpenClaw will expect.

---

### `__YOUR_TELEGRAM_BOT_TOKEN__`
This comes from Telegram's BotFather bot.

1. Open Telegram and search for **@BotFather**
2. Send the message `/newbot`
3. Follow the prompts — pick a name and a username for your bot
4. BotFather will reply with a token that looks like this:

```
123456789:ABCDefgh1234567890abcdefGHIJKLMNOP
```

Copy that entire string and paste it in as your `botToken`.

---

### `__YOUR_OPENAI_API_KEY__` *(api-chatgpt.json only)*
This comes from your OpenAI account.

1. Go to [platform.openai.com](https://platform.openai.com)
2. Click your profile → **API Keys**
3. Click **Create new secret key**
4. Copy it immediately — OpenAI only shows it once

It will look like:

```
sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Note: Using the API costs money per token. Make sure you have billing set up in your OpenAI account.

---

### `__NOT_REQUIRED_FOR_OLLAMA__` *(local configs only)*
Leave this as-is or replace it with any dummy string — Ollama runs locally and does not require an API key. The field exists in the config format but is ignored for local models.

---

## Project Templates

Ready-to-use markdown files for structuring an agent-managed project. Copy the `templates/project/` folder into your workspace's `projects/[name]/` directory.

```
templates/project/
├── spec.md           Product specification (goals, features, user flows)
├── milestones.md     6-milestone delivery plan with acceptance criteria
├── status.md         Live project status tracker
└── AGENT-TODO.md     Agent task backlog (guides agents toward ticket workflow)
```

**How to use:**
1. Create a project folder: `mkdir -p projects/my-app/tickets/open projects/my-app/tickets/closed`
2. Copy the templates: `cp templates/project/* projects/my-app/`
3. Replace `[PROJECT NAME]` and `[DATE]` placeholders
4. Agents will read these files at session start and update them as they work

---

## Ticket System

All work items — bugs, features, tasks, questions — are tracked as ticket files in `tickets/open/` and `tickets/closed/`. This replaces ad-hoc TODO lists with a structured, enterprise-style workflow.

### Ticket Types

| Type | Prefix | When to use |
|------|--------|-------------|
| **BUG** | `BUG-001.md` | Defects, regressions, things that are broken |
| **FEAT** | `FEAT-001.md` | New feature requests |
| **TASK** | `TASK-001.md` | General work items (replaces TODO entries) |
| **QUESTION** | `QUESTION-001.md` | Clarification needed before work can proceed |
| **EPIC** | `EPIC-001.md` | Groups related tickets into a larger initiative |

### Ticket Lifecycle

```
proposed → ready → in-progress → fixed → passed → released
                 → blocked
                              → qa-failed → fixed (loop)
```

### How to use

1. Copy the right template from `templates/tickets/` to `tickets/open/`
2. Rename it with the next number: `BUG-001.md`, `FEAT-002.md`, etc.
3. Fill in the fields (title, description, priority, severity for bugs)
4. Agents update **Status** as work progresses
5. When done, move from `tickets/open/` to `tickets/closed/`

Templates are in:

```
templates/tickets/
├── BUG-template.md       Bug report (includes severity, steps to reproduce)
├── FEAT-template.md      Feature request
├── TASK-template.md      General work item
├── QUESTION-template.md  Clarification needed
└── EPIC-template.md      Initiative grouping child tickets
```

### Priority Levels

| Level | Meaning |
|-------|---------|
| D1 | Critical — blocks everything, fix immediately |
| D2 | High — important, fix before next milestone |
| D3 | Medium — normal priority |
| D4 | Low — nice to have, do when time allows |

---

## Resources

- [OpenClaw](https://openclaw.ai) — official site and documentation
- [Ollama](https://ollama.com) — run local AI models
- [SCRYPTO](https://scrypto.one) — community hub where these configs came from
- [OpenClaw Mission Control](https://github.com/UAPRaptor/OpenClaw-Multi-Agent-Workflow) — installer, monitor dashboard, and agent management
