# VentureMind on FastMCP — MCP Tool Broker
> 25.1k GitHub stars · Apache-2.0 · Python 3.10+ · PrefectHQ/fastmcp v3.2.4

## What It Is

**PrefectHQ/fastmcp** is the standard Python framework for building MCP (Model Context Protocol) servers and clients. FastMCP is downloaded 1M times/day and powers 70% of all MCP servers globally.

> **Key role in VentureMind**: FastMCP exposes every Python service across all 10 Domain Swarms as MCP-compliant tools. Any MCP-capable LLM (Claude Code, Codex, Cursor, etc.) can discover and call VentureMind tools directly without manual SDK wiring.

## Why FastMCP for VentureMind

| VentureMind Need | FastMCP Answer |
|---|---|
| Universal tool exposure | `@mcp.tool` decorator auto-generates schemas, validation, docs from Python functions |
| MCP-capable LLMs (Claude Code, Codex) | Native MCP client support — connect with 3 lines of code |
| Production-grade transport | stdio (local), HTTP (remote), WebSocket — with auth and lifecycle management |
| Code Mode (v3.1+) | BM25 tool discovery → schema fetch → code generation → sandbox execution, 60% fewer tokens |
| Multi-provider composition | FileSystem, OpenAPI, Proxy, Skills providers — compose into one server |
| Tool versioning | `@tool(version="2.0")` — multiple versions served from one codebase |
| Scope-based auth | Per-tool, per-user, per-scope authorization with async checks |
| OpenTelemetry | Built-in tracing across all tool calls and flows |

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│            VentureMind MCP Server (FastMCP v3)               │
│  HTTP · Port 4200 · Bearer auth · Code Mode enabled         │
│  Tools: 47 across Tax / Legal / Mobility / Capital / Comp  │
└──────────────────────────────────────────────────────────────┘
          │ MCP Protocol (JSON-RPC over HTTP)
          ▼
┌──────────────────────────────────────────────────────────────┐
│              MCP-Capable LLM Clients                         │
│  Claude Code · Codex · Cursor · OpenAI Agents               │
│  Claude Desktop · Any MCP-compliant client                   │
└──────────────────────────────────────────────────────────────┘
```

## 47 Tools Across 5 Domains

| Domain | Tools | Example |
|---|---|---|
| **Tax Strategist** | 8 | `get_treaty_network`, `calculate_withholding`, `validate_entity_type` |
| **Legal Swarm** | 12 | `generate_founder_agreement`, `generate_cap_table`, `validate_nda` |
| **Mobility Swarm** | 10 | `score_neobanks`, `get_residency_path`, `calculate_tx_cost` |
| **Capital Swarm** | 9 | `score_investors`, `generate_termsheet`, `calculate_valuation` |
| **SafetyNet** | 8 | `verify_kyc`, `run_aml_check`, `get_protocol_zero_status` |

## Code Mode — 60% Token Reduction

Traditional MCP: all 47 tool schemas loaded upfront (~40k tokens per request).
Code Mode (v3.1+): BM25 search → fetch top-3 schemas → generate Python → execute in sandbox (~8k tokens).

```python
# Step 1: Discover relevant tools by natural query
results = await client.discover_tools("founder equity vesting cliff 4-year")

# Step 2: Get only the schemas you need
schemas = await client.get_tool_schemas(results[:3])

# Step 3: LLM generates and executes Python in sandbox
code = await client.generate_code(schemas, task="Create cap table for 3 co-founders with 4-year cliff, 1-year vesting")
```

## Provider Architecture (v3.0+)

```python
from fastmcp import FastMCP
from fastmcp.providers import FileSystemProvider, OpenAPIProvider

mcp = FastMCP("venturemind")

# Auto-load tools from VentureMind Python services
mcp.add_provider(FileSystemProvider("/home/workspace/VentureMind/services/"))

# Expose any REST API as MCP tools
mcp.add_provider(OpenAPIProvider("https://api.nomadflow.ai/openapi.json"))
```

## Integration Stack

| Component | Role |
|---|---|
| `venturemind-mcp-orchestrator` (SOUL.md) | Gatekeeping, scope enforcement, tool discovery authorization |
| `venturemind-fastmcp-toolkit` (SKILL.md) | Agent runtime: how to call, compose, and manage MCP tools |
| `venturemind_server.py` | Live FastMCP server — run with `fastmcp run venturemind_server.py:mcp` |
| **paperclip** | Orchestration reads tool catalog from this server |
| **Claude Code** | MCP-capable LLM connects to this server via `claude_desktop_config.json` |
| **Mem0** | Tool call history stored as agent memory |
| **SafetyNet** | Compliance checks on every tool call — logs to AgentDecisionLog |

## Deployment

```bash
# Install
uv pip install fastmcp

# Run server (HTTP + auth)
VENTUREMIND_MCP_TOKEN=$(python -c "import secrets; print(secrets.token_urlsafe(32))")
fastmcp run venturemind_server.py:mcp \
  --transport http \
  --port 4200 \
  --auth-token "$VENTUREMIND_MCP_TOKEN"
```

```json
// Claude Desktop: ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "venturemind": {
      "command": "fastmcp",
      "args": ["run", "/home/workspace/VentureMind/fastmcp/servers/venturemind_server.py:mcp"],
      "env": {
        "VENTUREMIND_MCP_TOKEN": "<your-token>"
      }
    }
  }
}
```

## Security — Tool-Level RBAC

```python
from fastmcp.auth import Scope, BearerTokenAuth

async def require_capital_scope(ctx):
    token = ctx.auth_context.token
    scopes = decode_jwt(token)["scopes"]
    if "capital:read" not in scopes and "capital:write" not in scopes:
        raise AuthorizationError("capital:scope required")

@mcp.tool(requires_auth=require_capital_scope)
def score_investors(criteria: dict) -> list[dict]:
    """Capital Swarm: investor scoring — requires capital:read scope."""
    return _score_investors(criteria)
```
