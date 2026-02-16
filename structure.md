# Project Structure

High-level structure of `agi_bot`.

```text
agi_bot/
│
├── core/
│   ├── registry/        # Tool metadata, versioning, storage
│   ├── runner/          # Process management (start/stop/health)
│   ├── verifier/        # Validation, linting, testing
│   ├── catalog/         # LLM-visible tool catalog
│   ├── gateway/         # Control-plane LLM tools implementation
│   └── memory/          # Optional KV storage
│
├── adapters/
│   ├── rest/            # Internal REST tool adapter
│   └── mcp/             # MCP integration layer
│
├── tools_store/         # Versioned tool artifacts
│   └── <tool_id>/<version>/
│       ├── manifest.json
│       ├── src/
│       ├── tests/
│       └── build.log
│
├── tests/               # Core system tests
│
├── README.md
├── structure.md
├── flow.md
└── todo.md
```

---

## Core Modules

### registry

Responsible for:

* storing tool metadata
* version tracking
* activation state management
* runtime information (pid, port, health)
* tool event history

Backed by:

* SQLite database
* filesystem storage for tool artifacts

Each tool version is immutable once created.

---

### runner

Responsible for:

* launching tool processes (via `uvicorn`)
* allocating ports
* performing health checks
* stopping and restarting tools
* reporting runtime status

Each tool runs as an isolated process.

LLM never communicates directly with tool processes.

---

### verifier

Responsible for:

* manifest validation (pydantic models)
* static checks (ruff)
* optional type checks (future: pyright/mypy)
* test execution (`pytest` with timeout)
* generating structured verification reports

Tools cannot activate unless verification succeeds.

---

### catalog

Responsible for building the LLM-visible tool list.

For each active tool it exposes:

* name
* description
* input schema
* output schema
* version
* health status

Routing information (port, process info) is hidden from the LLM.

---

### gateway

Implements the internal control-plane LLM tools:

* `add_tool`
* `remove_tool`
* `get_tool`
* `list_tools`
* `tool_status`
* `invoke_tool`
* `connect_mcp_server` (optional)
* `disconnect_mcp_server`

This is the only interface the LLM uses to manage tools.

---

### memory (optional in MVP)

Provides:

* tool-scoped key-value storage
* host-scoped key-value storage
* optional TTL support

Used for runtime memory, not long-term data storage.

---

## Adapters

### rest

Defines the internal REST contract for tools.

Each tool must expose:

* `GET /health`
* `POST /invoke`
* optionally `GET /schema`

The host communicates with tools exclusively through this interface.

---

### mcp

Provides integration with external MCP servers.

Responsibilities:

* connect/disconnect MCP servers
* import external tools into catalog
* route invocation to MCP transport
* normalize schemas

External tools are treated similarly to internal tools but are not stored locally.

---

## tools_store Layout

Each tool version is stored as:

```
tools_store/<tool_id>/<version>/
    manifest.json
    src/
    tests/
    build.log
```

Characteristics:

* versions are immutable
* activation state is tracked in registry
* build artifacts and logs are preserved for debugging

---

## Design Constraints

* LLM never accesses tool processes directly.
* Every tool is versioned.
* Activation requires passing verification.
* Control plane is strictly separated from runtime.
* Observability is part of the core design.
* Future isolation strategies (venv/container) must not break architecture boundaries.

```
```
