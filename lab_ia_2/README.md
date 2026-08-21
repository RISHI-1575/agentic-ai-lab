# Agentic AI — Lab Test 2 Answers (Experiments 6–12)

Solved notebooks for the exam templates (experiments 6–10), plus reference copies of
experiments 11–12 (no exam template exists for these yet). Built from the course's
reference solutions. Run in **Jupyter Notebook** on Windows.

## Files

| File | Experiment |
|---|---|
| `6_knowledge_graph_answer.ipynb` | Agent querying a Neo4j knowledge graph (MCP + custom tools) |
| `7_multi-agent_supervisor_pattern_answer.ipynb` | Supervisor agent coordinating Calendar + Email sub-agents |
| `8_multi-agent_router_pattern_answer.ipynb` | Router pattern fanning a query out to GitHub/Notion/Slack sub-agents |
| `9_agent_evaluation_answer.ipynb` | Trajectory Match + LLM-as-judge evaluation of an agent |
| `10_agent_observability_answer.ipynb` | Full tracing of model/tool/retrieval calls with Arize Phoenix |
| `11_low-level_orchestration_using_langgraph_answer.ipynb` | Hand-built LangGraph workflow (durable execution, interrupts, human-in-the-loop) |
| `12_agent_harness_answer.ipynb` | Research agent built on LangGraph's `deepagents` harness |
| `AGENTS.md`, `skills/` | Support files needed by notebook 12 (memory + skills folder) |

Notebooks 11 and 12 have no fill-in-the-blank exam template (the course's `lab_tests/lab_test_2`
folder only covers 6–10) — they're included here as complete, already-solved reference copies
for the same study purpose.

## Setup — Windows, Anaconda Prompt

Follow the same base setup as Lab Test 1 (conda env `agentic_ai`, Ollama, core packages).
This batch needs a few extra pieces on top of that.

### Extra packages for 6–12

```bash
pip install langchain-neo4j
pip install agentevals
pip install arize-phoenix
pip install ipywidgets
pip install deepagents
pip install python-dotenv
```

### Extra models

```bash
ollama pull granite4.1:3b
ollama pull qwen3-embedding:0.6b
```

### Jupyter Notebook

```bash
pip install notebook
jupyter notebook
```
Make sure `agentic_ai` is active in the terminal before running `jupyter notebook`, and
select that kernel from **Kernel → Change kernel** if needed.

## Special instructions per experiment

### Notebook 6 (Knowledge Graph) — needs Neo4j MCP server running first

1. Install the Neo4j MCP server package (see course README) in the `agentic_ai` env.
2. Check it: `neo4j-mcp-server -v` (should be 1.5.3 or later).
3. Start it, pointed at the public demo graph:
```bash
neo4j-mcp-server --neo4j-uri "neo4j+s://demo.neo4jlabs.com:7687" --neo4j-database "companies" --neo4j-read-only true --neo4j-transport-mode "http" --neo4j-http-port 8003
```
4. Leave that terminal running, then execute the notebook.
5. (Optional) Browse the same graph visually at `https://demo.neo4jlabs.com:7473/browser/`
   with user/password `companies`/`companies` — loads slowly, don't close the window.

### Notebook 10 (Observability) — needs a Phoenix server running first

```bash
phoenix serve
```
Leave it running; its UI is at `http://localhost:6006` and it collects traces on
`http://localhost:4317`. Run this before executing the notebook.

### Notebook 12 (Agent Harness) — needs an Ollama API key + Phoenix

1. Generate an Ollama API key (used for the agent's web-search tool).
2. Create a `.env` file in this same folder containing: `OLLAMA_API_KEY=<your-key>`
3. Start `phoenix serve` first (same as notebook 10 — this notebook reuses the same tracing setup).
4. Keep `AGENTS.md` and the `skills/` folder in the same directory as the notebook —
   the agent reads them as relative paths at runtime.

## Notes

- Notebook 6 uses `granite4.1:3b` — found more reliable than Llama/Qwen at generating valid Cypher queries.
- Notebook 9 evaluates a `llama3.2:3b` agent using `granite4.1:3b` as the separate judge model.
- Notebooks 10 and 12 share the same OpenTelemetry/Phoenix instrumentation approach.
- None of the original course material under the parent folder was modified — this
  folder only adds new files alongside the untouched extracted reference notebooks.
