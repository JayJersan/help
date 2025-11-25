---
title: AccuKnox <> Bifrost Integration
description: Guide to integrate AccuKnox LLM Defense with Bifrost using a custom Go plugin.
---

# AccuKnox <> Bifrost Integration Guide

This guide demonstrates how to integrate AccuKnox LLM Defense with Bifrost using a custom plugin written in Go.

## Step 1: Install Dependencies

### Clone the Bifrost Repository

```bash
git clone https://github.com/maximhq/bifrost.git
cd bifrost
```

### Build the Bifrost HTTP Binary

!!! info "Note"
    For this POC we are using bifrost-http binary to test out the config. You can use this config with any Bifrost deployment method.

```bash
go build -o bifrost-http transports/bifrost-http/main.go
```

## Step 2: Download AccuKnox Plugin

Download the latest AccuKnox plugin for Bifrost:

### Using wget

```bash
wget https://github.com/accuknox/bifrost-poc/releases/latest/download/accuknox-plugin.so
```

### Using curl

```bash
curl -LO https://github.com/accuknox/bifrost-poc/releases/latest/download/accuknox-plugin.so
```

!!! tip "Source Code"
    The plugin source code is available at [github.com/accuknox/bifrost-poc](https://github.com/accuknox/bifrost-poc) for reference.

### Move the Plugin

```bash
mv accuknox-plugin.so /path/to/bifrost/
```

## Step 3: Configure Bifrost

Create or update your Bifrost configuration file with the following settings:

```json
{
    "log_level": "debug",
    "server": {
      "port": 8080,
      "host": "0.0.0.0"
    },
    "plugins": [
      {
        "enabled": true,
        "name": "accuknox-logger",
        "path": "./accuknox-plugin.so",
        "config": {
          "enabled": true,
          "api_key": "<YOUR_ACCUKNOX_JWT_TOKEN>",
          "user_info": "<username@accuknox.com>"
        }
      }
    ],
    "providers": {
      "openai": {
        "keys": [
          {
            "value": "<YOUR_OPENAI_API_KEY>",
            "models": ["gpt-3.5-turbo"],
            "weight": 1.0
          }
        ]
      }
    }
}
```

## Step 4: Configuration Details

### Plugin Configuration

- **`enabled`**: Set to `true` to activate the plugin
- **`name`**: Plugin identifier (must match the name returned by `GetName()`)
- **`path`**: Path to the compiled `.so` file
- **`api_key`**: Your AccuKnox LLM Defense JWT token (obtained from AccuKnox dashboard)
- **`user_info`**: Your email or username for tracking

### AccuKnox API Endpoint

The plugin automatically determines the AccuKnox API endpoint by decoding the JWT token's `iss` field:

- **dev**: `https://cwpp.dev.accuknox.com/llm-defence/application-query`
- **stage**: `https://cwpp.stage.accuknox.com/llm-defence/application-query`
- **demo**: `https://cwpp.demo.accuknox.com/llm-defence/application-query`
- **prod**: `https://cwpp.prod.accuknox.com/llm-defence/application-query`

### Provider Configuration

- Add your LLM provider credentials (OpenAI, Anthropic, etc.)
- Each key can be restricted to specific models
- Use `weight` for load balancing across multiple keys

## Step 5: Run Bifrost Server

### Copy the Plugin

```bash
cp accuknox-plugin.so /path/to/bifrost/
cd /path/to/bifrost/
```

### Start the Server

```bash
./bifrost-http -app-dir . -log-level debug -log-style pretty
```

## Step 6: Test the Integration

Send a test request using curl:

```bash
curl -X POST http://localhost:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "openai/gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Say hello"}],
    "max_tokens": 50
  }'
```
