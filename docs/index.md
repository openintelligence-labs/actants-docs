# actants

A Python framework for building LLM agents that runs against local Ollama by
default and treats cloud providers as opt-in extras.

```python
import asyncio
from actants import Agent

async def main():
    agent = Agent()
    result = await agent.run("Say hello.")
    print(result.content)

asyncio.run(main())
```

## At a glance

| Property | Notes |
|---|---|
| Default LLM provider | Ollama (no API key required) |
| Cloud providers | OpenAI, Anthropic, Gemini, Groq, Mistral (opt-in extras) |
| Concurrency | async / await |
| MCP | client + server (`actants.mcp`, requires `[mcp]` extra) |
| A2A | client + server (`actants.a2a`, requires `[a2a]` extra) |
| Tracing | OpenTelemetry GenAI semantic conventions |
| Telemetry from the framework itself | none |
| License | MIT |
| Python | 3.12+ |

## Where to start

<div class="oi-grid" markdown>
<div class="oi-card" markdown>
[**Quickstart →**](quickstart.md)

Install, run an agent, and add a tool.
</div>
<div class="oi-card" markdown>
[**Installation →**](installation.md)

Extras, platform notes, and editable installs.
</div>
<div class="oi-card" markdown>
[**Configuration →**](configuration.md)

Environment variables, settings, and app paths.
</div>
<div class="oi-card" markdown>
[**Agent →**](concepts/agent.md)

The `Agent` class, memory, hooks, and streaming.
</div>
<div class="oi-card" markdown>
[**MCP →**](mcp/server.md)

Expose or consume tools over the Model Context Protocol.
</div>
<div class="oi-card" markdown>
[**A2A →**](a2a/server.md)

Expose your agent to peers, or call theirs.
</div>
<div class="oi-card" markdown>
[**Cookbook →**](cookbook/research-agent.md)

Runnable end-to-end recipes.
</div>
<div class="oi-card" markdown>
[**API reference →**](api/index.md)

Every public symbol, generated from source.
</div>
</div>

## What you get

The core install ships the `Agent`, the `LLM` gateway, a tool registry, an
in-memory cache, a cost tracker, retry and fallback policies, an embeddings
client, SQLite and JSONL storage helpers, and OpenTelemetry tracing. Providers
and interop layers are extras, so an unused integration costs nothing at import
time — symbols resolve lazily on first attribute access.

<span class="oi-badge">v0.5.2 on PyPI</span> · <span class="oi-badge">v0.5.3 in main</span> ·
MIT licensed · [Source](https://github.com/openintelligence-labs/actants)
