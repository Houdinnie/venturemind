---
name: venturemind-fastmcp-toolkit
description: VentureMind FastMCP toolkit — expose Python functions as MCP tools for the Nomad Flow swarm. Apache-2.0 · Python 3.10+ · PrefectHQ/fastmcp v3.2.4
metadata:
  source:
    repo: PrefectHQ/fastmcp
    attribution: PrefectHQ
    license: Apache-2.0
---

# VentureMind FastMCP Toolkit

Exposes Nomad Flow Python services as MCP-compliant tools using FastMCP's `@mcp.tool` decorator. All 10 Domain Swarms expose functions through a shared FastMCP server, enabling any MCP-capable LLM (Claude Code, Codex, etc.) to call VentureMind tools directly.

## Tool Conventions

```python
from fastmcp import FastMCP

mcp = FastMCP("venturemind")

@mcp.tool
def register_founder(data: dict) -> dict:
    """
    Register a new founder in the VentureMind CRM.
    Required: { name, email, country, entity_type }
    Returns: { founder_id, status, created_at }
    """
    return {"founder_id": "fnd_abc123", "status": "active", "created_at": "2026-05-09T00:00:00Z"}
```

## VentureMind Server — Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   VentureMind MCP Server                    │
│  FastMCP v3 · HTTP transport · Bearer auth · Port 4200      │
└─────────────────────────────────────────────────────────────┘
    │
    ├── Tax Strategist Tools
    │     ├── get_treaty_network(country: str) -> treaties[]
    │     ├── calculate_withholding(state: dict) -> breakdown
    │     └── validate_entity_type(entity: str, country: str)
    │
    ├── Legal Swarm Tools
    │     ├── generate_founder_agreement(founders: list)
    │     ├── generate_cap_table(shares: dict)
    │     └── validate_nda(enforcement_jurisdiction: str)
    │
    ├── Mobility Swarm Tools
    │     ├── score_neobanks(country: str, criteria: dict)
    │     ├── get_residency_path(country: str) -> pathway
    │     └── calculate_tx_cost(tx: dict) -> cost_breakdown
    │
    ├── Capital Swarm Tools
    │     ├── score_investors(criteria: dict) -> investors[]
    │     ├── generate_termsheet(profile: dict) -> doc
    │     └── calculate_valuation(revenue: float, stage: str)
    │
    └── Compliance Tools (SafetyNet)
          ├── verify_kyc(tier: int, data: dict) -> kyc_result
          ├── run_aml_check(transaction: dict) -> aml_result
          └── get_protocol_zero_status() -> status
```

## 3-Step Tool Discovery (Code Mode v3.1+)

```python
# 1. Search relevant tools by BM25 (no upfront catalog loading)
results = mcp.discover_tools(query="founder equity vesting cliff")

# 2. Get schemas for top-k ranked tools
schemas = mcp.get_tool_schemas([tool_id_1, tool_id_2])

# 3. Generate Python that chains call_tool() in a sandbox
code = mcp.generate_code(tools=schemas, task="Create cap table for 3 co-founders with 4-year cliff")
```

## Auth Configuration

```python
from fastmcp import FastMCP
from fastmcp.auth import BearerTokenAuth

mcp = FastMCP(
    "venturemind",
    auth=BearerTokenAuth(
        token=os.environ["VENTUREMIND_MCP_TOKEN"],
        audience="venturemind.nomadflow.ai"
    )
)
```

## Code Mode (v3.1+) — Reduces Context by 60%+

```python
# Traditional: all 47 tool schemas loaded upfront (~40k tokens)
await client.call_tool("register_founder", {...})

# Code Mode: BM25 search → schema fetch → execute (~8k tokens)
results = await client.discover_tools("founder registration equity")
schemas = await client.get_tool_schemas(results[:3])
code = await client.generate_code(schemas, task="register 2 founders, split 60/40")
# Generated Python executes in sandbox
```

## Error Handling

```python
@mcp.tool
def register_founder(data: dict) -> dict:
    try:
        return _do_register(data)
    except ValidationError as e:
        raise ToolError(f"Validation failed: {e}", context={"code": "VALIDATION_ERROR"})
    except PermissionError as e:
        raise ToolError(f"Access denied: {e}", context={"code": "ACCESS_DENIED"})
    except Exception as e:
        raise ToolError(f"Unexpected error: {e}", context={"code": "INTERNAL_ERROR"})
```

## Deployment

```bash
# Install
uv pip install fastmcp

# Run server
fastmcp run venturemind_server.py:mcp --transport http --port 4200
```

```bash
# Client: connect and call
from fastmcp import FastMCPClient

client = FastMCPClient("http://localhost:4200", token=os.environ["VENTUREMIND_MCP_TOKEN"])
result = await client.call_tool("register_founder", {
    "name": "Ryan P.",
    "email": "ryan@example.com",
    "country": "Zimbabwe",
    "entity_type": "LLC"
})
```
