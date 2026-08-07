# Installation

## Requirements

- **Python 3.12+** (3.13 supported)
- **Ollama** running locally for the default Ollama provider — install from
  [ollama.com](https://ollama.com) and run `ollama serve`. Not required if you
  only use cloud providers.

## Core install

```bash
pip install actants
```

That gives you the `Agent`, `LLM`, `ToolRegistry`, in-memory cache, cost
tracker, OTel tracing, retry/fallback policies, embeddings client, storage
helpers, and CLI primitives — built around the bundled Ollama provider.

## Extras

actants is split into **opt-in extras** so unused providers and integrations
do not pay for themselves at import time.

| Extra | What it adds | Install |
|---|---|---|
| `openai` | OpenAI provider | `pip install 'actants[openai]'` |
| `anthropic` | Anthropic / Claude provider | `pip install 'actants[anthropic]'` |
| `gemini` | Google Gemini provider (uses bundled httpx) | `pip install 'actants[gemini]'` |
| `groq` | Groq provider (OpenAI-compatible) | `pip install 'actants[groq]'` |
| `mistral` | Mistral provider (OpenAI-compatible) | `pip install 'actants[mistral]'` |
| `cache` | `SqliteVecCache` semantic cache (sqlite-vec) | `pip install 'actants[cache]'` |
| `cli` | Click + Rich CLI helpers | `pip install 'actants[cli]'` |
| `mcp` | MCP client + server (official `mcp` SDK) | `pip install 'actants[mcp]'` |
| `a2a` | A2A client + server (official `a2a-sdk`) | `pip install 'actants[a2a]'` |
| `all` | OpenAI + Anthropic + cache + cli + mcp | `pip install 'actants[all]'` |
| `dev` | pytest, ruff, build, twine | `pip install 'actants[dev]'` |

Combine extras with commas: `pip install 'actants[openai,anthropic,mcp,a2a]'`.

`all` does not include `a2a`, `gemini`, `groq`, or `mistral`. The A2A stack
pulls in a server (`starlette`, `uvicorn`), and the remaining provider extras
resolve to dependencies already covered by `openai` or by the bundled `httpx`.
Add `a2a` explicitly if you need it: `pip install 'actants[all,a2a]'`.

## Verify the install

```python
import actants
print(actants.__version__)        # 0.5.3
```

Bare `import actants` should complete in **under 50 ms** on any modern
machine — the package uses [PEP 562](https://peps.python.org/pep-0562/) lazy
attribute loading so symbols only resolve on first access.

## Run an agent locally

```bash
ollama pull llama3.2
ollama serve &                  # background process
python -c "
import asyncio
from actants import Agent
print(asyncio.run(Agent().run('Say hello in one sentence.')).content)
"
```

If Ollama is not running, you'll see `httpx.ConnectError`. Start `ollama serve`
or switch to a cloud provider (see [Configuration](configuration.md)).

## Editable install (for contributors)

```bash
git clone https://github.com/openintelligence-labs/actants
cd actants
pip install -e '.[dev,openai,anthropic,mcp,a2a,cache,cli]'
pytest                          # 153 tests should pass
```
