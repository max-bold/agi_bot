# Concept Flow

This document explains how agi_bot behaves.

---

## 1. Tool Creation

LLM generates:
- Tool manifest
- Python source code
- Tests

LLM calls:
    add_tool(manifest, code_bundle)

---

## 2. Verification Pipeline

Host performs:

1. Manifest validation
2. Static checks (lint)
3. Test execution
4. Process startup
5. Health check

If any stage fails:
- Structured error report returned to LLM
- Tool not activated

LLM may attempt repair and resubmit.

---

## 3. Activation

If verification succeeds:
- Tool version marked active
- Runner starts process
- Tool added to catalog

LLM now sees it as a regular callable tool.

---

## 4. Invocation

LLM calls tool by name.

Host:
- Validates input schema
- Routes to correct process
- Validates output schema
- Returns structured result

---

## 5. Modification

LLM may:

- get_tool
- modify code
- resubmit via add_tool (new version)

Old version remains available unless removed.

---

## 6. Removal

remove_tool:
- Stops process
- Marks version inactive
- Optionally deletes artifacts

---

## 7. MCP Integration

External MCP servers may be connected.

Their tools appear in catalog
but are not stored locally.

---

## Design Principles

- LLM never talks directly to processes.
- Every tool is versioned.
- No tool activates without tests.
- Control plane is separated from runtime.
- Observability is mandatory.
