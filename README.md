# agi_bot

`agi_bot` is an experimental runtime that allows an LLM to write, validate, version and execute its own tools.

The goal is simple:

> What happens if we give an LLM the ability to extend itself — safely?

This project is not a chatbot.
It is not a framework for prompt chains.
It is a control-plane for AI self-extension.

---

## Core Idea

An LLM can:

- generate new tools (Python code + manifest)
- submit them via `add_tool`
- pass validation and tests
- activate them
- use them like any other tool

Each tool runs as an isolated process with its own lifecycle and versioning.

The host system remains in control.

---

## Architecture Overview

Two planes:

### Control Plane (LLM tools)
Internal tools available to the LLM:
- `add_tool`
- `remove_tool`
- `get_tool`
- `list_tools`
- `tool_status`
- `invoke_tool`
- `connect_mcp_server` (optional)

These are not REST endpoints.
They are system-level LLM tools.

### Data Plane (Runtime)
Each tool:
- Runs in its own process
- Exposes minimal REST API (`/invoke`, `/health`)
- Has versioned source and manifest
- Is validated before activation

---

## Why This Exists

Most AI systems use tools.

This project explores something different:

> What if tools are not predefined — but created by the LLM itself?

This is a controlled experiment in:
- self-extension
- runtime mutation
- versioned AI capability growth
- tool lifecycle governance

---

## Status

Early stage.
MVP in progress.

---

## Contributing

This is an open research project.

If you are interested in:
- AI agents
- tool governance
- sandboxing
- AI self-modification safety
- runtime design
- MCP integration
- versioned AI systems

You are welcome.

Open issues, propose architecture changes, challenge assumptions.

This is an experiment. Let’s do it properly.
