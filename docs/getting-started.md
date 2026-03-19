# Getting Started with SILENTCHAIN AI Community Edition

SILENTCHAIN AI Community Edition is a Jython Burp Suite extension that performs AI-powered passive vulnerability scanning of HTTP traffic. It detects OWASP Top 10 vulnerabilities using LLM-based analysis of request/response pairs.

## Quick Start

1. Ensure Burp Suite Professional or Community is installed with Jython configured.
2. Download `silentchain_ai_community.py`.
3. In Burp Suite: **Extender > Extensions > Add > Extension type: Python** and select the file.
4. Open the **SILENTCHAIN** tab to configure your AI provider (Ollama, OpenAI, Claude, Gemini, or ClaudeCode).
5. Set your target scope in Burp and browse the target through the proxy.
6. Findings appear in the SILENTCHAIN tab as requests are analyzed.

## Supported AI Providers

| Provider | Default Model | API Key Required |
|----------|--------------|-----------------|
| Ollama   | llama3.1:8b  | No              |
| OpenAI   | gpt-4        | Yes             |
| Claude   | claude-3-sonnet | Yes          |
| Gemini   | gemini-pro   | Yes             |
| ClaudeCode | N/A (uses local CLI) | No     |

## Data Privacy

The built-in DataSanitizer automatically redacts sensitive data (API keys, credentials, PII, internal IPs) before sending prompts to cloud AI providers. It is enabled by default and can be toggled via the `sanitize_enabled` setting. See [Configuration](configuration.md#data-privacy-datasanitizer) for details.

## Requirements

- Burp Suite (Professional or Community Edition)
- Jython 2.7+ standalone JAR configured in Burp
- An AI provider endpoint (local Ollama or cloud API key)
- For ClaudeCode provider: `claude` CLI installed and authenticated locally
