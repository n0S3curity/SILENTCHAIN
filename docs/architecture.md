# Architecture

## Overview

SILENTCHAIN AI Community Edition is a single-file Jython Burp Suite extension (~94KB). It implements several Burp interfaces to intercept HTTP traffic, analyze it with AI, and report findings as Burp Scanner Issues.

## Component Diagram

```
Burp Suite
  |
  v
BurpExtender (single .py file)
  |
  +-- IHttpListener ------> Intercepts proxy traffic
  +-- IScannerCheck -------> Passive scan integration
  +-- ITab ----------------> SILENTCHAIN UI tab
  +-- IContextMenuFactory -> Right-click "Analyze" menu
  |
  v
AI Analysis Thread (background)
  |
  +-- Deduplication (SHA256 hash check)
  +-- AI Provider (urllib2 HTTP calls)
  |     +-- Ollama API
  |     +-- OpenAI API
  |     +-- Claude API
  |     +-- Gemini API
  +-- Response Parser (structured finding extraction)
  |
  v
Finding Registration
  +-- IScanIssue (Burp Scanner Issue)
  +-- JTable model update (SILENTCHAIN tab)
```

## Data Flow

1. HTTP request/response passes through Burp's proxy.
2. `IHttpListener.processHttpMessage()` captures the traffic.
3. Deduplication check: SHA256(URL + parameters). Skip if already analyzed.
4. Background thread sends request/response data to the configured AI provider.
5. AI returns structured analysis with vulnerability type, severity, confidence, and description.
6. Finding is registered as a Burp Scanner Issue and added to the SILENTCHAIN tab.

## Key Design Decisions

- **Single file:** Burp's Jython extension loader requires a single .py file entry point. All code lives in one file to simplify loading.
- **No external packages:** Only Jython stdlib and Burp API imports are used. `urllib2` handles all HTTP communication.
- **Threading:** Python `threading.Thread` runs AI analysis off the Swing EDT to keep the UI responsive.
- **Java Swing UI:** All GUI components use `javax.swing` since that is what Burp's extension API provides.
