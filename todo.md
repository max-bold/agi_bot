# TODO — Initial Milestones

## Phase 0 — Bootstrap

- [ ] Create repo
- [ ] Add project skeleton
- [ ] Define ToolManifest pydantic model
- [ ] Define common error model
- [ ] Setup SQLite schema

---

## Phase 1 — Registry

- [ ] Implement tool metadata storage
- [ ] Implement version storage
- [ ] Implement state transitions (staged/active/failed/disabled)
- [ ] Implement tool events logging

---

## Phase 2 — Runner

- [ ] Implement port allocator
- [ ] Implement process launch (uvicorn)
- [ ] Implement health check wait loop
- [ ] Implement stop/kill logic

---

## Phase 3 — Verifier

- [ ] Manifest validation
- [ ] Ruff integration
- [ ] Pytest integration with timeout
- [ ] Structured verification report

---

## Phase 4 — LLM Control Tools

- [ ] add_tool
- [ ] remove_tool
- [ ] get_tool
- [ ] list_tools
- [ ] tool_status
- [ ] invoke_tool

---

## Phase 5 — Observability

- [ ] Log invocation events
- [ ] Log errors
- [ ] Store last N logs per tool

---

## Phase 6 — MCP (Optional Early Integration)

- [ ] MCP client adapter
- [ ] Tool import from MCP server
- [ ] Unified invocation routing

---

## Later Research Topics

- [ ] Per-tool venv support
- [ ] Container isolation
- [ ] Tool scoring / reliability metrics
- [ ] Automatic tool pruning
- [ ] Self-healing strategies
