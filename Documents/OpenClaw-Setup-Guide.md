# OpenClaw - Complete Setup Guide (Beginner Friendly)

OpenClaw is a powerful AI assistant that connects to your chat apps (Telegram, Discord, WhatsApp, etc.) and lets you interact with AI models through chat, a web dashboard, or your terminal.

Think of it as your **personal AI bot** that you control, running on your own computer.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Install OpenClaw](#2-install-openclaw)
3. [Initial Setup](#3-initial-setup)
4. [Set Up AI Models](#4-set-up-ai-models)
   - [Option A: Ollama (Free, Local)](#option-a-ollama-free-runs-on-your-computer)
   - [Option B: Anthropic Claude API (Paid)](#option-b-anthropic-claude-api-paid)
   - [Option C: OpenAI API (Paid)](#option-c-openai-api-paid)
5. [Connect Telegram](#5-connect-telegram)
6. [Using OpenClaw](#6-using-openclaw)
   - [CLI (Command Line)](#a-cli-command-line)
   - [TUI (Terminal Chat UI)](#b-tui-terminal-chat-ui)
   - [Web Dashboard](#c-web-dashboard-browser)
7. [Testing Everything](#7-testing-everything)
8. [Stopping and Restarting OpenClaw](#8-stopping-and-restarting-openclaw)
9. [Install as Background Service (Auto-Start)](#9-install-as-a-background-service-optional)
10. [Troubleshooting](#10-troubleshooting)
11. [Quick Reference Cheat Sheet](#11-quick-reference-cheat-sheet)
12. [Useful Links](#12-useful-links)

---

## 1. Prerequisites

Before you begin, make sure you have these installed:

| Software | Why You Need It | Download Link |
|----------|----------------|---------------|
| **Node.js** (v18+) | OpenClaw runs on Node.js | https://nodejs.org/ |
| **npm** | Comes with Node.js, used to install OpenClaw | (included with Node.js) |
| **Git** (optional) | Useful for updates | https://git-scm.com/downloads |

**Verify your installs** by opening a terminal and running:

```bash
node --version
# Should show v18.x.x or higher

npm --version
# Should show 9.x.x or higher
```

---

## 2. Install OpenClaw

Open your terminal (Command Prompt, PowerShell, or Git Bash) and run:

```bash
npm install -g openclaw
```

**Verify the installation:**

```bash
openclaw --version
# Should show something like: 2026.2.26
```

---

## 3. Initial Setup

Run the interactive setup wizard. This creates your configuration files:

```bash
openclaw setup --wizard
```

The wizard will walk you through:
- Creating your config at `~/.openclaw/openclaw.json`
- Setting up a workspace directory
- Basic gateway configuration

You can also run the full configuration wizard at any time:

```bash
openclaw configure
```

Or configure specific sections:

```bash
openclaw configure --section model      # Just model setup
openclaw configure --section channels   # Just chat channels
openclaw configure --section gateway    # Just gateway settings
```

---

## 4. Set Up AI Models

You need at least one AI model for OpenClaw to use. You have three options:

---

### Option A: Ollama (Free, Runs on Your Computer)

Ollama lets you run AI models locally on your machine. **No API keys, no cost, fully private.**

#### Step 1: Install Ollama

Download and install from: https://ollama.com/download

#### Step 2: Pull a model

Open a terminal and download a model:

```bash
# Small and fast (recommended to start)
ollama pull llama3.2

# Medium size, smarter
ollama pull llama3.1

# If you have a powerful GPU (16GB+ VRAM)
ollama pull llama3.1:70b
```

#### Step 3: Start Ollama

Ollama usually starts automatically after install. Verify it's running:

```bash
ollama list
# Should show your downloaded models
```

#### Step 4: Configure OpenClaw to use Ollama

```bash
openclaw models auth add
# Select Ollama when prompted

# Set your default model
openclaw models set ollama/llama3.2
```

**Verify it works:**

```bash
openclaw models status
# Should show Ollama as configured with your model
```

> **Tip:** Ollama runs at `http://localhost:11434` by default. OpenClaw auto-detects it.

---

### Option B: Anthropic Claude API (Paid)

Claude is made by Anthropic and is excellent at coding, writing, and reasoning.

#### Step 1: Get your API Key

1. Go to https://console.anthropic.com/
2. Sign up or log in
3. Go to **API Keys** section
4. Click **Create Key**
5. Copy the key (starts with `sk-ant-...`)

#### Step 2: Add the key to OpenClaw

```bash
openclaw models auth add
# Select Anthropic when prompted
# Paste your API key
```

Or add it directly:

```bash
openclaw models auth paste-token
# Follow the prompts to paste your Anthropic key
```

#### Step 3: Set Claude as your default model

```bash
# Use Claude Sonnet (good balance of speed and quality)
openclaw models set anthropic/claude-sonnet-4-6

# Or use Claude Opus (most capable, slower)
openclaw models set anthropic/claude-opus-4-6

# Or use Claude Haiku (fastest, cheapest)
openclaw models set anthropic/claude-haiku-4-5-20251001
```

> **Pricing:** Check https://www.anthropic.com/pricing for current rates.

---

### Option C: OpenAI API (Paid)

#### Step 1: Get your API Key

1. Go to https://platform.openai.com/
2. Sign up or log in
3. Go to **API Keys** (in the left sidebar)
4. Click **Create new secret key**
5. Copy the key (starts with `sk-...`)

#### Step 2: Add the key to OpenClaw

```bash
openclaw models auth add
# Select OpenAI when prompted
# Paste your API key
```

#### Step 3: Set OpenAI as your default model

```bash
# GPT-4o (recommended)
openclaw models set openai/gpt-4o

# GPT-4o mini (cheaper, faster)
openclaw models set openai/gpt-4o-mini
```

> **Pricing:** Check https://openai.com/pricing for current rates.

---

### Check Your Model Setup

After configuring any model, verify everything is working:

```bash
# See all configured models
openclaw models list

# See detailed status
openclaw models status
```

---

## 5. Connect Telegram

This lets you chat with your AI through Telegram.

### Step 1: Create a Telegram Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot`
3. Choose a **name** for your bot (e.g., "My OpenClaw Bot")
4. Choose a **username** for your bot (must end in `bot`, e.g., `my_openclaw_bot`)
5. BotFather will give you a **token** that looks like: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`
6. **Copy this token** - you'll need it next

### Step 2: Add Telegram to OpenClaw

```bash
openclaw channels add --channel telegram --token YOUR_BOT_TOKEN_HERE
```

Replace `YOUR_BOT_TOKEN_HERE` with the actual token from BotFather.

### Step 3: Verify the Connection

```bash
# Check channel status
openclaw channels status

# Or check overall status
openclaw status
```

You should see Telegram listed as connected.

### Step 4: Find Your Chat ID

To send messages, you need to know your Telegram chat ID:

```bash
openclaw directory --channel telegram
```

Or simply **open Telegram and send any message to your bot** - OpenClaw will pick it up once the gateway is running.

---

## 6. Using OpenClaw

There are **three ways** to interact with OpenClaw:

### A. CLI (Command Line)

Send messages directly from your terminal:

```bash
# Send a message to someone on Telegram
openclaw message send --channel telegram --target CHAT_ID --message "Hello from OpenClaw!"

# Read recent messages
openclaw message read --channel telegram --target CHAT_ID

# Talk to the AI agent directly
openclaw agent --message "What is the capital of France?"

# Talk to the AI and deliver the reply to Telegram
openclaw agent --to CHAT_ID --message "Tell me a joke" --deliver
```

### B. TUI (Terminal Chat UI)

A full interactive chat interface right in your terminal:

```bash
openclaw tui
```

This opens a chat window where you can:
- Type messages and get AI responses
- See conversation history
- Send replies to your connected channels

**TUI Options:**

```bash
# Connect to a specific session
openclaw tui --session my-project

# Send an initial message
openclaw tui --message "Let's brainstorm ideas"

# Auto-deliver replies to your chat channels
openclaw tui --deliver
```

Press `Ctrl+C` to exit the TUI.

### C. Web Dashboard (Browser)

A web-based control panel:

```bash
openclaw dashboard
```

This opens your browser with the OpenClaw control UI where you can:
- Monitor all channels
- View conversations
- Manage settings
- See agent activity

If you just want the URL without opening the browser:

```bash
openclaw dashboard --no-open
```

---

## 7. Testing Everything

Follow this checklist to make sure everything works:

### Test 1: Health Check

```bash
openclaw doctor
```

This scans your entire setup and reports any problems. If issues are found:

```bash
openclaw doctor --fix
```

### Test 2: Gateway is Running

```bash
openclaw health
```

Should return a healthy status. If not, start the gateway first (see Section 8).

### Test 3: Model is Responding

```bash
openclaw agent --message "Say hello in 5 words"
```

You should get an AI response back.

### Test 4: Telegram is Connected

```bash
openclaw channels status
```

Should show Telegram as "connected" or "online".

### Test 5: Send a Test Message

```bash
openclaw message send --channel telegram --target YOUR_CHAT_ID --message "Hello! OpenClaw is working!"
```

Check your Telegram - you should see the message from your bot.

### Test 6: Receive a Message

1. Open Telegram
2. Send a message to your bot
3. Watch your gateway terminal - you should see the incoming message logged

---

## 8. Stopping and Restarting OpenClaw

This is the section you'll use most often - **how to start OpenClaw after closing it, and how to stop it.**

### Starting OpenClaw (Every Time You Want to Use It)

You need to start the **Gateway** - this is the core service that handles everything:

```bash
# Start the gateway (runs in foreground - keeps the terminal busy)
openclaw gateway run
```

> **Keep this terminal window open!** The gateway stops when you close the terminal.

If the port is already in use (e.g., from a previous crash):

```bash
openclaw gateway run --force
```

### Stopping OpenClaw

- If running in foreground: Press `Ctrl+C` in the gateway terminal
- If running as a service:

```bash
openclaw gateway stop
```

### Quick Start Routine (Do This Every Time)

Every time you restart your computer or want to use OpenClaw:

```
Step 1:  If using Ollama, make sure it's running (check system tray or run: ollama list)
Step 2:  Open a terminal
Step 3:  Run: openclaw gateway run
Step 4:  (Optional) Open a second terminal and run: openclaw tui
Step 5:  Start chatting via Telegram, TUI, or CLI!
```

### Check if OpenClaw is Already Running

```bash
openclaw gateway status
```

---

## 9. Install as a Background Service (Optional)

If you don't want to manually start the gateway every time, install it as a system service that starts automatically:

### Install the Service

```bash
openclaw gateway install
```

This registers OpenClaw with Windows Task Scheduler so it starts on boot.

### Manage the Service

```bash
# Start the service
openclaw gateway start

# Stop the service
openclaw gateway stop

# Restart the service
openclaw gateway restart

# Check service status
openclaw gateway status

# Remove the service
openclaw gateway uninstall
```

---

## 10. Troubleshooting

### "Gateway not running" Error

```bash
openclaw gateway run --force
```

### "No model configured" Error

```bash
openclaw models status
# Then set a model:
openclaw models set ollama/llama3.2
```

### Telegram Bot Not Responding

```bash
# Check channel status
openclaw channels status

# Re-add the token
openclaw channels add --channel telegram --token YOUR_BOT_TOKEN

# Check logs
openclaw channels logs
```

### General Issues

```bash
# Run the doctor - it finds and fixes common problems
openclaw doctor --fix

# Check detailed logs
openclaw logs

# Nuclear option - reset everything (you'll need to reconfigure)
openclaw reset
```

### Ollama Not Detected

Make sure Ollama is running:

```bash
ollama list
# If it shows your models, Ollama is running

# If not, start it manually
ollama serve
```

---

## 11. Quick Reference Cheat Sheet

| What You Want to Do | Command |
|---------------------|---------|
| **Start OpenClaw** | `openclaw gateway run` |
| **Stop OpenClaw** | `Ctrl+C` in gateway terminal |
| **Check health** | `openclaw doctor` |
| **Check status** | `openclaw status` |
| **Open chat UI in terminal** | `openclaw tui` |
| **Open web dashboard** | `openclaw dashboard` |
| **Send a message** | `openclaw message send --channel telegram --target ID --message "Hi"` |
| **Read messages** | `openclaw message read --channel telegram --target ID` |
| **Ask AI a question** | `openclaw agent --message "your question"` |
| **See configured models** | `openclaw models list` |
| **Change AI model** | `openclaw models set MODEL_NAME` |
| **See all channels** | `openclaw channels list` |
| **Reconfigure** | `openclaw configure` |
| **View help for any command** | `openclaw COMMAND --help` |

---

## 12. Useful Links

| Resource | URL |
|----------|-----|
| **OpenClaw Documentation** | https://docs.openclaw.ai/cli |
| **Ollama Download** | https://ollama.com/download |
| **Ollama Model Library** | https://ollama.com/library |
| **Anthropic Console (API Key)** | https://console.anthropic.com/ |
| **Anthropic Pricing** | https://www.anthropic.com/pricing |
| **OpenAI Platform (API Key)** | https://platform.openai.com/ |
| **OpenAI Pricing** | https://openai.com/pricing |
| **Telegram BotFather** | https://t.me/BotFather |
| **Node.js Download** | https://nodejs.org/ |

---

> **Remember:** The most important thing is to run `openclaw gateway run` - that's the engine that powers everything. Without the gateway running, nothing else works!
