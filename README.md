# Agentic AI — Lab Test 1 Answers (Experiments 1–5)

Solved notebooks for the exam templates, built from the course's reference solutions.
Run in **Jupyter Notebook** on Windows. This README covers setup on a fresh college
PC and quick checks for a PC that's already been set up before.

## Files

| File | Experiment |
|---|---|
| `1_simple_hello_world_agent_answer.ipynb` | Hello World Agent (non-streaming + streaming) |
| `2_simple_customer_service_agent_answer.ipynb` | Customer Service Agent (cancel order / update address) |
| `3_tool_calling_local_and_api_answer.ipynb` | Local tools (add/subtract) + Wikipedia API tool |
| `4_tool_calling_mcp_answer.ipynb` | MCP server as a tool (math + weather servers) |
| `4_tool_calling_mcp_math_server.py` | Standalone MCP math server used by notebook 4 |
| `4_tool_calling_mcp_weather_server.py` | Standalone MCP weather server used by notebook 4 |
| `5_memory_short-term_answer.ipynb` | Agent with short-term (in-memory) conversation memory |

## Setup — Windows, Anaconda Prompt

### 1. Check what's already there before installing anything

```bash
conda env list
```
If `agentic_ai` is listed, just activate it and skip creating it again:
```bash
conda activate agentic_ai
```

Check Ollama is installed and running:
```bash
ollama --version
curl http://localhost:11434
```
`curl` should return "Ollama is running". If Ollama was installed before, it usually
auto-starts in the background (system tray) — you rarely need to launch it manually.
If the check fails, start it:
```bash
ollama serve
```
(leave that terminal window open, use a separate window for everything else)

Check which models are already pulled:
```bash
ollama list
```

Check which Python packages are already installed (with `agentic_ai` active):
```bash
pip show agent-framework agent-framework-ollama agent-framework-openai langchain langchain-ollama langchain-community langchain-mcp-adapters wikipedia mcp ollama 2>&1 | findstr /C:"Name:" /C:"WARNING"
```
Anything printed as `WARNING: Package(s) not found` is missing — install only those.

### 2. Fresh machine — full install

```bash
conda create -n agentic_ai python=3.12.13
conda activate agentic_ai
```

Install Ollama: download `OllamaSetup.exe` from
https://github.com/ollama/ollama/releases/tag/v0.24.0, run it, allow Visual C++
Redistributables if prompted, then close and reopen the terminal.

Pull the models used across these notebooks:
```bash
ollama pull llama3.2:3b
ollama pull qwen3.5:2b
```

Install the packages:
```bash
pip install agent-framework
pip install ollama
pip install agent-framework-ollama --pre
pip install agent-framework-openai
pip install langchain
pip install langchain-ollama
pip install langchain-community
pip install langchain-mcp-adapters
pip install wikipedia
pip install mcp
```

### 3. Jupyter Notebook

```bash
pip install notebook
jupyter notebook
```
This opens Jupyter in your browser. Open each `_answer.ipynb` file from here.
Jupyter uses whichever environment it was launched from, so make sure
`agentic_ai` is active in the terminal **before** running `jupyter notebook`.
If a wrong kernel is selected, use the notebook's **Kernel → Change kernel**
menu and pick the one matching `agentic_ai` (Python 3.12.13).

## Special instruction — Notebook 4 (MCP)

Notebook 4 needs two standalone MCP servers running in the background **before**
you execute its cells. This step is required every time, regardless of whether
the machine was set up before:

1. Open **two separate Anaconda Prompt** windows.
2. In each: `conda activate agentic_ai`
3. Window A: `python 4_tool_calling_mcp_math_server.py` (listens on port 8001)
4. Window B: `python 4_tool_calling_mcp_weather_server.py` (listens on port 8002)
5. Leave both windows running, then run the notebook 4 cells from Jupyter.

## Notes

- Notebooks 1, 2, 3, 5 use `llama3.2:3b`; notebook 4 uses `qwen3.5:2b` (found more
  reliable for the weather-related part of that query).
- Notebook 2's tools are intentionally scoped to just `cancel_order` and
  `update_delivery_address`, matching the original exam template's instructions
  (the reference solution also includes an `issue_refund` tool, left out here).
- Notebook 3's tools are scoped to just `add` and `subtract` for the same reason
  (the reference solution also includes `multiply`).
