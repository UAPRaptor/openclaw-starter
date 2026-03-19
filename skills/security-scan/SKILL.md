---
name: security-scan
description: Run a security scan across all local repos and the machine's network exposure. Checks for hardcoded secrets, PII, and open ports visible to the internet. Results are written in plain English for non-technical users.
---

You are executing the `/security-scan` skill. Work through each section in order and present results in plain, jargon-free language. Assume the person reading this is not a developer.

---

## Section 1 — Secrets Scan (things that shouldn't be in code)

Scan all repos in the working directory and common sibling directories for:

**API keys and tokens:**
- OpenAI keys: `sk-proj-`, `sk-org-`, `sk-` followed by long strings
- Anthropic keys: `sk-ant-`
- GitHub tokens: `ghp_`, `github_pat_`
- AWS keys: `AKIA` followed by 16 uppercase characters
- Telegram bot tokens: number colon then `AA` then 33 characters
- Hex strings exactly 64 characters long that appear to be Bearer tokens
- Any line containing `token`, `secret`, `password`, `api_key`, or `apikey` followed by an actual value (not a placeholder)

**Credentials and keys:**
- `-----BEGIN PRIVATE KEY-----` or `-----BEGIN RSA PRIVATE KEY-----`
- Database connection strings with passwords embedded (e.g. `mongodb://user:password@`, `postgres://user:pass@`)
- `.env` files that were accidentally committed to git (run `git log --all -- "*.env"` to check history)

**Skip these — they are safe by design:**
- Firebase client-side API keys starting with `AIzaSy` in HTML/JS files (these are meant to be public)
- Placeholder values like `__YOUR_TOKEN__`, `YOUR_API_KEY_HERE`, `changeme`, `sk-xxx`
- Files listed in `.gitignore` that have never been committed

**How to present secrets findings:**
For each real finding, say:
> ⚠️ **Found in [filename]:** A [type of secret] appears to be hardcoded on line [N]. This is like leaving your house key under the welcome mat — anyone who sees this file can use it. You should: (1) remove it from the code, (2) add the file to .gitignore if it's a config file, (3) revoke and regenerate the key from the service that issued it.

If nothing is found:
> ✅ No hardcoded secrets found. Your keys and passwords are not visible in your code.

---

## Section 2 — PII Scan (personal information that shouldn't be in code)

Search all repo files for patterns that look like personally identifiable information committed accidentally:

- Email addresses that appear to be real (not `example@example.com`, `user@test.com`, or placeholder formats)
- Phone numbers in various formats (10-digit US numbers, international with +)
- Social Security Number patterns: `\d{3}-\d{2}-\d{4}`
- Credit card number patterns: 16-digit strings grouped as 4-4-4-4
- Full names combined with other personal data (name + address, name + DOB)
- Physical addresses committed in code files

**How to present PII findings:**
For each real finding, say:
> ⚠️ **Found in [filename], line [N]:** This looks like a real [email address / phone number / etc.]. Storing personal information in code files is a privacy risk — if this repo is ever made public or shared, that person's information is exposed. Remove it and replace with a placeholder or store it in a secure config file that isn't committed to git.

If nothing is found:
> ✅ No personal information (names, emails, phone numbers) found in code files.

---

## Section 3 — Internet Exposure Scan (what's visible outside your network)

**Step 1: Check external IP**
Run a command to find the machine's public IP address. On Windows use:
```
curl -s https://api.ipify.org
```

**Step 2: Scan common ports**
Using nmap if available, or PowerShell Test-NetConnection, check whether these ports are reachable from the local network perspective. Note which services are listening:

| Port | Plain-English name | What it's used for |
|------|-------------------|-------------------|
| 21 | FTP | Old-school file transfer — almost never needed today |
| 22 | SSH | Remote terminal access to the machine |
| 23 | Telnet | Very old remote access — unencrypted, should never be open |
| 25 | Email (SMTP) | Sending email — should only be open on mail servers |
| 80 | Website (HTTP) | Unencrypted web traffic |
| 443 | Secure website (HTTPS) | Encrypted web traffic — normal for web servers |
| 3000 | Dev server | Local development web server — should not be open to the internet |
| 3306 | MySQL database | Database — should never be open to the internet |
| 5432 | PostgreSQL database | Database — should never be open to the internet |
| 5984 | CouchDB | Database — should never be open to the internet |
| 6379 | Redis | Fast memory database — should never be open to the internet |
| 8080 | Alt web server | Secondary web port — common for dev servers |
| 8443 | Alt secure web | Secondary HTTPS port |
| 27017 | MongoDB | Database — should never be open to the internet |
| 11434 | Ollama | Local AI model server — should NOT be open to the internet |
| 1880 | Node-RED | Automation dashboard — should not be open to the internet |
| 8123 | Home Assistant | Smart home dashboard — protect carefully |

Check which of these are currently listening on the local machine:
- Windows: `netstat -an | findstr LISTENING`
- Then cross-reference: which of those are bound to `0.0.0.0` (visible to the network) vs `127.0.0.1` (local only, safe)

**Step 3: Context — LAN vs. internet exposure**

Most agents and AI tools run behind a home or office router (a LAN). This means:
- Ports open on the machine are usually only reachable from other devices on the same Wi-Fi/network — NOT from the internet
- The router acts like a locked front door — outside traffic can't get in unless the router is specifically told to forward it (called "port forwarding")
- The exception: cloud VMs, VPS servers, or machines with a direct internet connection have no router in front of them — every open port is directly reachable

**How to present network findings:**

For each port found listening on `0.0.0.0`:
> 🔍 **Port [N] ([plain name]) is open on this machine.**
> What it does: [plain English description]
> Risk level: [Low — normal / Medium — investigate / High — close this]
> Who can reach it: If you're behind a home or office router (most people are), this is only visible to other devices on your same Wi-Fi. It is NOT exposed to the internet unless you've set up port forwarding. If this is a cloud server or VPS, it IS exposed to the internet.
> What to do: [specific recommendation]

If Ollama (port 11434) is found:
> 🔍 **Port 11434 (Ollama — your local AI) is open.**
> Ollama runs your local AI models (like Llama or Qwen). It's designed to be a local-only service. If other devices on your network can reach it, they could send prompts to your AI without your knowledge. If you're behind a home router this is low risk, but you may want to restrict it to localhost only by setting `OLLAMA_HOST=127.0.0.1` in your environment.

End the network section with:
> **Bottom line:** [1-2 sentences summarizing whether the exposure looks normal or concerning given the context]

---

## Final Summary

End the report with a simple traffic-light summary:

```
SECURITY SCAN SUMMARY
─────────────────────
🔴 Critical issues (act now):     [count or "None"]
🟡 Things to look into:           [count or "None"]
✅ All clear:                     [count or "All clear"]

Scanned: [list of repos]
Date: [today's date]
```

Keep the whole report readable by someone who doesn't write code. No jargon without explanation. No false alarms — only flag things that are genuinely worth the user's attention.
