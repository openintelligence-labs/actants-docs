---
hide:
  - toc
---

# Build an AI agent without an API key

**actants** is a Python framework for LLM agents. It runs against a model on
your own machine by default, so there is no signup, no key, and no bill — and
one argument switches the same code to OpenAI or Anthropic when you want a
frontier model.

<div class="hero-code" markdown>

```python
from actants import Agent

agent = Agent()                    # no key, no config — runs on your machine
result = await agent.run("Summarise this repo's README.")
```

</div>

The entire setup is `pip install actants` and `ollama pull llama3.2`.

<div class="hero-actions" markdown>
[Get started in 5 minutes](quickstart.md){ .md-button .md-button--primary }
[See a real agent](cookbook/research-agent.md){ .md-button }
</div>

---

## Why not just call the API yourself?

Because the second week is where it gets expensive. An agent that survives
contact with real work needs tool calling, a retry policy, a token budget, and
somewhere to put conversation state. actants ships those as the default path
rather than as an integration you assemble.

<div class="oi-grid oi-grid--pairs" markdown>

<div class="oi-card" markdown>
**Start free, scale up later**

Develop against a local model at zero cost. Move to a frontier model by
changing one argument — the agent, tools, and memory code stay identical.
</div>

<div class="oi-card" markdown>
**Your prompts stay yours**

The framework sends no telemetry, ever. On the default local provider,
nothing leaves your machine at all.
</div>

<div class="oi-card" markdown>
**Speaks to other agents**

Native [MCP](mcp/server.md) and [A2A](a2a/server.md) — consume tools from
any MCP server, or expose your agent for others to call.
</div>

<div class="oi-card" markdown>
**Debuggable when it misbehaves**

OpenTelemetry GenAI tracing is built in, so you can see which step chose
which tool and what it cost.
</div>

</div>

## Switching providers is one argument

Same agent, same tools, same memory. Only the model changes.

=== "Local (default)"

    ```python
    from actants import Agent

    agent = Agent()
    ```

    No key. No account. Runs on your hardware.

=== "Anthropic"

    ```python
    from actants import Agent, LLM

    agent = Agent(llm=LLM(provider="anthropic", model="claude-sonnet-4-6"))
    ```

    Requires `pip install 'actants[anthropic]'`.

=== "OpenAI"

    ```python
    from actants import Agent, LLM

    agent = Agent(llm=LLM(provider="openai", model="gpt-5"))
    ```

    Requires `pip install 'actants[openai]'`.

Gemini, Groq, and Mistral work the same way. Every provider is an opt-in extra,
so an integration you do not install costs nothing at import time.

You can also switch without touching code at all — set `ACTANTS_PROVIDER` and
`ACTANTS_MODEL` in the environment, and `Agent()` picks them up.

## Give it a tool

Agents get useful when they can do things. Register a function, and the model
decides when to call it.

```python
from actants import Agent, ToolRegistry

tools = ToolRegistry()

async def add(a: int, b: int) -> int:
    return a + b

tools.register_function(
    "add",
    "Add two integers",
    add,
    input_schema={
        "type": "object",
        "properties": {"a": {"type": "integer"}, "b": {"type": "integer"}},
        "required": ["a", "b"],
    },
)

agent = Agent(tools=tools)
result = await agent.run("What is 17 + 25?")   # "17 + 25 = 42."
```

[Read the tools guide →](concepts/tools.md)

## Where to go next

<div class="oi-grid" markdown>

<div class="oi-card" markdown>
[**Quickstart →**](quickstart.md)

Install, run your first agent, add a tool. Five minutes.
</div>

<div class="oi-card" markdown>
[**Cookbook →**](cookbook/research-agent.md)

Runnable end-to-end recipes — research agents, MCP tools, agent pairs.
</div>

<div class="oi-card" markdown>
[**Coming from LangChain →**](migration/langchain.md)

Concept-by-concept mapping. Also [CrewAI](migration/crewai.md) and
[Pydantic AI](migration/pydantic-ai.md).
</div>

<div class="oi-card" markdown>
[**API reference →**](api/index.md)

Every public symbol, generated from the source.
</div>

</div>

## The details

??? info "What ships in the core install"

    The `Agent`, the `LLM` gateway, a tool registry, an in-memory cache, a cost
    tracker, retry and fallback policies, an embeddings client, SQLite and JSONL
    storage helpers, and OpenTelemetry tracing.

    Providers and interop layers are extras. Symbols resolve lazily on first
    attribute access, so an unused integration costs nothing at import time.

??? info "Specifications"

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

---

<span class="oi-badge">v0.5.2 on PyPI</span> · <span class="oi-badge">v0.5.3 in main</span> ·
MIT licensed · [Source](https://github.com/openintelligence-labs/actants)
