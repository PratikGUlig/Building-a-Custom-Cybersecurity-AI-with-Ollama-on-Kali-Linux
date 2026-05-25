# 🔴 RedTeamGPT — Local AI Cybersecurity Assistant with Ollama

> A fully offline, custom-trained red team LLM assistant built on **Kali Linux** using **Ollama** + **Open WebUI**. No API keys. No cloud. No data leaks.

---

## 📸 Demo

| CLI Mode | Open WebUI |
|----------|------------|
| Analyzing Nmap scan results like a senior pentester | Browser-based chat with model switching |

*(See the full report PDF/DOCX in this repo for all 21 step-by-step screenshots)*

---

## 🧠 What Was Built

Two custom models were trained on top of Alibaba's **Qwen2.5** base:

| Model | Base | Size | Persona |
|-------|------|------|---------|
| `redteam-assistant` | qwen2.5:3b | 3.1B params | Cybersecurity lab assistant for learning & defensive testing |
| `redteam-pro` | qwen2.5:7b | 7.6B params | Expert penetration tester — deeper reasoning, architectural inference |

Both run **100% locally** — no internet required after setup.

---

## 🗂️ Repository Structure

```
redteamgpt-ollama/
│
├── Modelfile-assistant        # Modelfile for redteam-assistant (3B)
├── Modelfile-pro              # Modelfile for redteam-pro (7B)
├── RedTeam_LLM_Report.docx    # Full step-by-step training report with screenshots
├── README.md                  # This file
└── docker-compose.yml         # Open WebUI + Ollama stack (optional)
```

---

## ⚙️ Prerequisites

- Kali Linux (or any Debian-based distro)
- 8 GB RAM minimum (16 GB recommended for the 7B model)
- ~8 GB free disk space
- Docker (for Open WebUI)
- No GPU required — runs in CPU mode

---

## 🚀 Quick Start

### 1. Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Verify it's running:

```bash
ollama --version
systemctl status ollama
```

---

### 2. Pull a Base Model

```bash
# Lightweight (3B) — faster, less RAM
ollama pull qwen2.5:3b

# Powerful (7B) — better reasoning, more RAM
ollama pull qwen2.5:7b
```

---

### 3. Create the Custom Models

```bash
# Build the assistant model (3B)
ollama create redteam-assistant -f Modelfile-assistant

# Build the pro model (7B)
ollama create redteam-pro -f Modelfile-pro
```

---

### 4. Run via CLI

```bash
ollama run redteam-assistant
# or
ollama run redteam-pro
```

**Example prompt:**
```
>>> Analyze this scan like a security analyst:
PORT    STATE  SERVICE  VERSION
22/tcp  open   ssh      OpenSSH 9.2
80/tcp  open   http     Apache 2.4
443/tcp open   https    nginx
```

---

### 5. Deploy Open WebUI (Browser Interface)

```bash
sudo apt install docker.io -y

sudo docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

Then open **http://localhost:3000** in your browser, create an admin account, and select your model.

---

## 📋 Modelfile Overview

### redteam-assistant (3B)

```
FROM qwen2.5:3b

SYSTEM """
You are RedTeamGPT, a cybersecurity lab assistant for authorized learning
and defensive testing environments.

Your expertise:
- network reconnaissance analysis
- Nmap interpretation
- web application security concepts
- OWASP Top 10
- Linux privilege escalation concepts
- Active Directory fundamentals
- MITRE ATT&CK mapping
- Sigma/SIEM query explanation
- Bash/Python scripting for security automation
- detection engineering concepts

Behavior:
- explain technical concepts clearly
- think like a security analyst
- help interpret scan results and logs
- provide safe, educational guidance only
- ask clarifying questions when scan context is missing
"""
```

### redteam-pro (7B)

```
FROM qwen2.5:7b

SYSTEM """
You are RedTeamGPT, an expert cybersecurity analyst for authorized lab
and defensive learning environments.

Your expertise includes:
- penetration testing methodology
- network reconnaissance
- Nmap analysis
- service fingerprinting
- web application assessment concepts
- Linux privilege escalation concepts
- Active Directory security concepts
- MITRE ATT&CK mapping
- SIEM investigation
- detection engineering
- Bash/Python security automation

Rules:
- think step-by-step like a senior security analyst
- infer likely architecture from scan data
- identify missing information
- suggest logical next validation steps
- explain reasoning clearly
- avoid generic security boilerplate
- be technically precise
"""
```

---

## 🧪 Example Outputs

### Nmap Scan Analysis (redteam-pro)

**Input:**
```
Analyze this like a penetration tester.
PORT    STATE  SERVICE  VERSION
22/tcp  open   ssh      OpenSSH 9.2
80/tcp  open   http     Apache 2.4
443/tcp open   https    nginx
```

**Output highlights:**
- Probable host role: public-facing web server with SSH-based admin backend
- Architecture clues: Apache + nginx co-deployment → possible reverse proxy
- Reconnaissance priorities: OS fingerprinting, SSH enumeration, web app scanning
- Next step commands: `nmap -sV -O`, `whatweb`, `httpx`, `gobuster`

---

## 🛡️ Ethical Use

This project is built strictly for:
- ✅ Authorized penetration testing lab environments
- ✅ Cybersecurity education and defensive training
- ✅ CTF (Capture the Flag) challenges
- ✅ Security research on systems you own or have permission to test

❌ **Never use these tools or models against systems without explicit written authorization.**

All model system prompts enforce ethical use boundaries. The models are instructed to provide educational guidance only.

---

## 📄 Full Report

The file `RedTeam_LLM_Report.docx` in this repo contains the complete 21-screenshot walkthrough covering every step from Ollama installation to interactive Open WebUI sessions.

---

## 🔧 Tech Stack

| Component | Tool |
|-----------|------|
| OS | Kali Linux (VMware) |
| LLM Runtime | [Ollama](https://ollama.com) v0.24.0 |
| Base Models | Qwen2.5:3b, Qwen2.5:7b (Alibaba) |
| Web Interface | [Open WebUI](https://github.com/open-webui/open-webui) v0.9.5 |
| Container | Docker |

---

## 🤝 Contributing

Pull requests are welcome! If you build a better Modelfile, have prompt improvements, or test on other base models (Mistral, LLaMA, etc.), feel free to open a PR.

---

## 📜 License

MIT License — free to use, modify, and distribute. See `LICENSE` for details.

---

*Built on Kali Linux · Powered by Ollama · Runs fully offline*
