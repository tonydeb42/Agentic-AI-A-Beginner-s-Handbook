# Agentic AI: A Beginner's Handbook

A guided, notebook-first path from a single LLM call to agents that can plan, use tools, retrieve documents, delegate work, pause for human approval, and be evaluated.

This handbook is for developers who know a little Python and want a structured way into agentic AI. Each chapter teaches a small set of ideas, shows the code, and gives you something to practice. You do not need prior experience with LangChain, LangGraph, or any agent framework.

**Contents**

- [What you will be able to do](#what-you-will-be-able-to-do)
- [Who this is for](#who-this-is-for)
- [How to use this handbook](#how-to-use-this-handbook)
- [Setup](#setup)
- [Two model tracks](#two-model-tracks)
- [Notebook conventions](#notebook-conventions)
- [Chapter guide](#chapter-guide)
- [Capstone projects](#capstone-projects)
- [Troubleshooting](#troubleshooting)
- [Glossary](#glossary)
- [References](#references)

---

## What you will be able to do

| # | Chapter | You will learn | You will build |
|---|---------|----------------|----------------|
| 1 | [LangChain](1.%20Langchain.ipynb) | Models, structured output, tools, the agent loop, streaming, HITL, summarization | A tool-using assistant with approvals |
| 2 | [LangGraph and MCP](2.%20Langraph.ipynb) | State graphs, reducers, checkpointing, ReAct, MCP servers and clients | A graph-based agent connected to MCP tools |
| 3 | [Vectorless RAG](3.%20Vectorless%20Rag.ipynb) | Vector RAG vs tree-based retrieval, indexing, evidence discipline, hybrid RAG | A document-question workflow with citations |
| 4 | [Deep Agents](4.%20Deepagents.ipynb) | Planning, filesystem working memory, subagents, skills, persistence | A delegating research agent |
| 5 | [Guardrails](5.%20Guardrails.ipynb) | Deterministic checks, model judges, PII, human review, layered defense | A guarded email agent |
| 6 | [LLM Evaluation](6.%20LLM%20Evaluation.ipynb) | Datasets, targets, LLM-as-judge, deterministic metrics, experiments | An evaluated Q&A application |
| 7 | [LLM Gateway](7.%20LLM%20Gateway.ipynb) | LiteLLM SDK, Router, Proxy, caching, fallbacks, rate limits | A gateway-backed, multi-model setup |

By the end you will have touched the full loop: **build an agent, connect it to tools, guard it, evaluate it, and serve it behind a gateway.**

---

## Who this is for

- You can write and run Python functions.
- You are comfortable with a terminal and installing packages.
- You have used a notebook before, or can learn as you go.
- You do **not** need to know LangChain, LangGraph, RAG, or agent frameworks.

You should be comfortable with these ideas before starting:

- variables, functions, classes, imports
- lists, dictionaries, type hints
- JSON-like data
- environment variables

If you are new to environments and packages, follow the setup section carefully and do it once before Chapter 1.

---

## How to use this handbook

Each chapter follows the same loop:

1. **Read** the mental model and comparison sections.
2. **Run** the executable cells and change one thing.
3. **Practice** with the exercise in the chapter guide below.
4. **Answer** the checkpoint questions without looking back.

### Recommended path

```text
1. LangChain
      |
      v
2. LangGraph and MCP
      |
      v
3. Vectorless RAG
      |
      v
4. Deep Agents
      |
      v
5. Guardrails
      |
      v
6. LLM Evaluation
      |
      v
7. LLM Gateway
```

### Fast track

If your goal is "build a guarded agent as soon as possible", use this order:

```text
1. LangChain -> 2. LangGraph and MCP -> 5. Guardrails -> 6. LLM Evaluation
```

Then return to **3. Vectorless RAG**, **4. Deep Agents**, and **7. LLM Gateway**.

### Suggested schedule

| Session | Chapter | Time | Focus |
|---------|---------|------|-------|
| 1 | Chapter 1 | 90 min | Models, tools, the agent loop |
| 2 | Chapter 1 | 45 min | HITL and summarization practice |
| 3 | Chapter 2 | 120 min | Graphs, state, checkpoints, MCP |
| 4 | Chapter 3 | 60 min | Retrieval and evidence |
| 5 | Chapter 4 | 90 min | Planning, delegation, working files |
| 6 | Chapter 5 | 60 min | Guardrail layers |
| 7 | Chapter 6 | 75 min | Datasets and evaluation |
| 8 | Chapter 7 | 90 min | Gateway, routing, fallbacks |
| 9 | Capstone | 2-4 hrs | Combine at least three chapters |

You can stretch this over two weeks by doing one session every other day.

---

## Setup

Do this once. You will need Python **3.12** (3.11 or newer should also work) and about 1 GB of free disk space.

### 1. Open the project folder

All notebooks expect to be launched from the folder that contains this README.

```powershell
cd "D:\agentic ai practice"
```

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, run this once:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Your prompt should now start with `(.venv)`.

### 3. Install dependencies

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

The requirements file is grouped by chapter. If you only want to start with Chapters 1 and 2, you can install just those packages:

```powershell
pip install -U langchain langgraph langchain-ollama langchain-google-genai fastmcp pydantic python-dotenv jupyterlab ipykernel
```

### 4. Configure environment variables

Copy the example file:

**Windows:**

```powershell
Copy-Item .env.example .env
```

**macOS / Linux:**

```bash
cp .env.example .env
```

Open `.env` and fill in only the keys for the track and chapters you plan to use. Never commit `.env` to a repository.

### 5. Load `.env` in a notebook

Notebooks do not load `.env` automatically. Add this as the **first code cell** in any notebook, before other imports:

```python
from dotenv import load_dotenv

load_dotenv()
```

Alternatively, set the variables in the same terminal before launching Jupyter. The notebook kernel inherits them.

### 6. Start Jupyter

```powershell
jupyter lab
```

Open the notebook for the chapter you are working on and select the `Python (.venv)` kernel. If you do not see it, run:

```powershell
python -m ipykernel install --user --name agentic-ai --display-name "Python (agentic-ai)"
```

### 7. Verify the setup

Run this cell. It should print versions without errors:

```python
import sys
print(sys.version)

import langchain
import langgraph
print("langchain", langchain.__version__)
print("langgraph", langgraph.__version__)
```

If you plan to use all chapters, also check:

```python
import deepagents
import langsmith
import litellm
print("optional packages loaded")
```

### Optional: local models with Ollama

Ollama lets you run a model on your own machine, with no API key.

1. Install Ollama from <https://ollama.com>.
2. Pull the small model used in Chapter 1:

```powershell
ollama pull qwen2.5-coder:3b
```

3. Start the server if it is not already running:

```powershell
ollama serve
```

4. In a notebook:

```python
from langchain_ollama import ChatOllama

model = ChatOllama(model="qwen2.5-coder:3b", temperature=0)
```

Small local models are good for learning the API. They are less reliable at tool calling than hosted models.

---

## Two model tracks

You can follow the handbook with a free/local track or a paid API track. Both are valid. Pick one for your first pass and do not switch mid-chapter unless a chapter says otherwise.

| Chapter | Free / local track | API track |
|---------|--------------------|-----------|
| 1. LangChain | Gemini free tier or Ollama | OpenAI, Anthropic, or Gemini |
| 2. LangGraph and MCP | Gemini or Ollama | OpenAI or Anthropic |
| 3. Vectorless RAG | No model calls required for the walkthrough | Any model for your own extension |
| 4. Deep Agents | Ollama (`ollama:qwen2.5-coder:3b`), best effort | OpenAI or Anthropic recommended |
| 5. Guardrails | Gemini or Ollama | OpenAI or Anthropic |
| 6. LLM Evaluation | LangSmith free tier + Gemini target/judge with small edits | OpenAI as written in the notebook |
| 7. LLM Gateway | One provider key routed through LiteLLM | Multiple provider keys |

### Where to get keys

- Google AI Studio: <https://aistudio.google.com/apikey>
- OpenAI: <https://platform.openai.com/api-keys>
- Anthropic: <https://console.anthropic.com>
- LangSmith: <https://smith.langchain.com>

Keep keys in `.env` or a secrets manager. Do not paste real keys into notebooks, screenshots, or source control.

### Switching models

Most examples accept a LangChain model object or a `provider:model` string. For example:

```python
from langchain_google_genai import ChatGoogleGenerativeAI

model = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash-lite",
    temperature=0,
)
```

```python
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4.1-mini", temperature=0)
```

Deep Agents chapters use the string form:

```python
model="openai:gpt-4.1-mini"
```

Replace any `YOUR_MODEL_NAME` placeholder with a model your account can access.

---

## Notebook conventions

A few conventions appear throughout the handbook:

- **Section banners** such as `# 5. MODEL TOOL CALLING` mark the start of a topic.
- **Conceptual cells** are design walkthroughs, not executable programs. They are labelled in the first line or described as pseudocode. Read them, then implement the idea in your own scratch cell.
- **Commented imports and calls** are optional paths. Uncomment the one that matches your environment.
- **Placeholders** like `YOUR_MODEL_NAME`, `YOUR_MODEL_PROVIDER`, and `os.environ[...]` must be replaced or exported before running.
- **Single continuous walkthroughs**: Chapters 1 and 2 are written as one long annotated script. For hands-on practice, copy a section into a new cell or a scratch notebook and modify it there.

Whenever a cell does not run, ask three questions in order:

1. Is the right kernel active and the virtual environment installed?
2. Is the model name valid and the API key exported?
3. Does the installed library version still support this API?

---

## Chapter guide

### Chapter 1: LangChain foundations

**Notebook:** [1. Langchain.ipynb](1.%20Langchain.ipynb)

**Goal:** Understand the progression from a plain model call to a tool-using agent, then add human approval and context management.

**Prerequisites:** Setup complete. A Gemini key or Ollama running.

**Install:**

```powershell
pip install -U langchain langchain-ollama langchain-google-genai langgraph pydantic
```

**Environment:**

- `GOOGLE_API_KEY` for Gemini, or
- a running Ollama server with `qwen2.5-coder:3b`

**Read and run, in order:**

1. Models, `model.invoke()`, and structured output. Confirm that `model.with_structured_output(...)` returns a Pydantic object.
2. Tools and `model.bind_tools(...)`. Notice that the model **requests** a tool but does not execute it.
3. `create_agent(...)`. This is the automatic model-tool loop.
4. `invoke()`, `stream()`, and `batch()`. Compare the shapes of the three results.
5. HITL with `HumanInTheLoopMiddleware`: approve, edit, reject.
6. `SummarizationMiddleware`: watch old messages get compressed while recent messages stay intact.

**Practice:**

1. Add a second tool, such as `get_time(city)`, and ask a question that requires both tools.
2. Run the same weather question through `invoke()` and `stream()` and describe the difference.
3. Trigger the HITL pause and resume it three times: once approving, once editing the recipient, once rejecting.
4. Change the summarization trigger to a lower token count and observe when compression happens.

**Checkpoint:**

- What is the difference between `model.bind_tools()` and `create_agent()`?
- Why does HITL require a checkpointer and a stable `thread_id`?
- Where does agent structured output appear in the result?

**Expected result:** a working weather agent, a structured `WeatherAnswer`, a paused-and-resumed email approval, and a summarization demo.

---

### Chapter 2: LangGraph and MCP

**Notebook:** [2. Langraph.ipynb](2.%20Langraph.ipynb)

**Goal:** Learn the graph model underneath agent frameworks, then connect graphs to external tools through the Model Context Protocol.

**Prerequisites:** Chapter 1.

**Install:**

```powershell
pip install -U langgraph langchain-core fastmcp langchain-google-genai langchain-ollama
```

**Environment:** same as Chapter 1 for the React agent section.

**Before you run:** the React agent example needs a model object. Define one at the top of a scratch cell or before that section:

```python
from langchain_google_genai import ChatGoogleGenerativeAI

model = ChatGoogleGenerativeAI(model="gemini-2.5-flash-lite", temperature=0)
```

**Read and run, in order:**

1. `StateGraph`, state, nodes, edges, `START`, and `END`.
2. Multiple nodes and the order of state updates.
3. Reducers: why `Annotated[list, add]` appends instead of replacing.
4. Checkpointing with `InMemorySaver` and a stable `thread_id`.
5. `ToolNode` and `create_react_agent`.
6. HITL with `interrupt(...)` and `Command(resume=...)`.
7. FastMCP servers over stdio and HTTP, then a single-server and multi-server client.

**Practice:**

1. Add a third node to the math graph, multiply the result by 3, and predict the final value before running.
2. Run the chat example twice with the same `thread_id`, then once with a new ID. Explain the difference.
3. Create a `weather_server.py` file from the stdio section, run it through a FastMCP client, and call `get_weather`.
4. Switch the server to HTTP, run it in a terminal, and connect the client to `http://127.0.0.1:8000/mcp`.
5. Register two MCP servers in the client configuration and list the combined tools.

**Checkpoint:**

- What does a reducer do, and what happens without one?
- How does LangGraph differ from MCP in purpose?
- What must be true before a paused graph can resume?

**Expected result:** a compiled graph with checkpointed memory, a working ReAct agent, and MCP tools discovered over both transports.

---

### Chapter 3: Vectorless RAG

**Notebook:** [3. Vectorless Rag.ipynb](3.%20Vectorless%20Rag.ipynb)

**Goal:** Understand retrieval as evidence gathering, compare vector RAG with tree-based PageIndex-style retrieval, and learn when to combine them.

**Prerequisites:** Chapters 1 and 2. No model key is required for the conceptual walkthrough.

**Install:** no new packages are required. The PageIndex SDK is optional and should be installed only after checking its current documentation.

**Environment:** optional `PAGEINDEX_API_KEY` if you connect a real PageIndex client.

**Read and run, in order:**

1. The one-minute RAG mental model and the traditional vector pipeline.
2. What "vectorless" means: reason over a document tree instead of searching embeddings.
3. Indexing phase vs query phase.
4. The example document tree and the path a question takes through it.
5. Retrieval-by-reasoning pseudocode, hybrid retrieval, and the grounded answer prompt.
6. The integration, evaluation, and production checklists.

**Practice:**

1. Build a small tree for a real PDF you own: title, page ranges, and one-sentence summaries.
2. Write five questions whose correct evidence sits in different branches, then trace the likely path for each.
3. Fill in three `evaluation_case` records and compute page recall.
4. Extend the grounded prompt with a rule about conflicting sources.
5. Decide whether your corpus is a strong or weak candidate for tree retrieval, and write two sentences explaining why.

**Checkpoint:**

- When is a document tree a better index than embeddings?
- What does page recall measure, and what does it miss?
- Why should retrieved text be treated as untrusted data?

**Expected result:** a document tree, a traced retrieval path, and a small evidence evaluation record.

---

### Chapter 4: Deep Agents

**Notebook:** [4. Deepagents.ipynb](4.%20Deepagents.ipynb)

**Goal:** Use a higher-level agent that plans, keeps working files, manages context, and delegates bounded work to subagents.

**Prerequisites:** Chapters 1 and 2. Checkpointing and subagents will make more sense if you have built both by hand.

**Install:**

```powershell
pip install -U deepagents langchain-openai langchain-anthropic
```

**Environment:**

- API track: `OPENAI_API_KEY` or `ANTHROPIC_API_KEY`
- Free track: `langchain-ollama` installed, Ollama running, and model string `ollama:qwen2.5-coder:3b`

Deep Agents is most reliable on the API track. If the free track struggles with multi-step tool use, finish the chapter on a hosted model and return to the local experiment later.

**Read and run, in order:**

1. `create_deep_agent(...)` and the smallest useful agent.
2. What is built in: planning, filesystem tools, context management, delegation.
3. Adding a plain Python tool.
4. Filesystem tools and the backend boundary.
5. Subagents with `name`, `description`, and `system_prompt`.
6. Thread persistence with a checkpointer.
7. Streaming, skills, and project instructions.
8. When to choose a basic agent, Deep Agents, or a custom LangGraph workflow.

**Practice:**

1. Replace `YOUR_MODEL_NAME` and run the smallest agent.
2. Add a second tool of your own and ask a task that needs both planning and the tool.
3. Create a `reviewer` subagent and a `researcher` subagent, then delegate one task to each.
4. Run the same conversation twice with one `thread_id`, then once with a different ID.
5. Write a one-page task prompt with acceptance criteria and ask the agent to produce a file that meets them.

**Checkpoint:**

- What does Deep Agents add beyond a basic tool-calling agent?
- What is the difference between graph state and a filesystem artifact?
- When should you delegate to a subagent instead of adding another tool?

**Expected result:** a planning agent that writes a working file, delegates to two subagents, and remembers a thread.

---

### Chapter 5: Guardrails

**Notebook:** [5. Guardrails.ipynb](5.%20Guardrails.ipynb)

**Goal:** Put controls around an agent instead of trusting a prompt to behave.

**Prerequisites:** Chapters 1, 2, and preferably 4.

**Install:**

```powershell
pip install -U langchain langgraph pydantic langchain-google-genai langchain-openai
```

**Environment:** `GOOGLE_API_KEY`, `OPENAI_API_KEY`, or Ollama, depending on the model you configure.

**Read and run, in order:**

1. The three guardrail positions: before the agent, during the agent, after the agent.
2. Deterministic checks vs model-based checks, and why you need both.
3. `PIIMiddleware` with `redact`, `mask`, `hash`, and `block`.
4. `HumanInTheLoopMiddleware` with a checkpointer.
5. A `before_agent` hook for input policy.
6. An `after_agent` hook for output review.
7. A layered design that gives each layer one job.

**Practice:**

1. Add a blocked term and verify the deterministic check rejects it.
2. Send an email address through each PII strategy and compare the outputs.
3. Put `send_email` behind HITL and run approve, edit, and reject.
4. Add an after-agent check that blocks output containing a fake secret.
5. Draw your own layered design for a customer-support agent, naming the job of each layer.

**Checkpoint:**

- Why is a regex better than an LLM for a required ID format?
- What does PII redaction protect, and what does it not protect?
- Why must tool implementations enforce authorization even when guardrails exist?

**Expected result:** a guarded email agent with input checks, PII handling, approval, and output review.

---

### Chapter 6: LLM evaluation

**Notebook:** [6. LLM Evaluation.ipynb](6.%20LLM%20Evaluation.ipynb)

**Goal:** Replace "it looks good" with a repeatable dataset, target, judges, and experiment results.

**Prerequisites:** Chapters 1 and 5. You should already know what a good answer looks like for your task.

**Install:**

```powershell
pip install -U langsmith langchain-openai openai pydantic
```

**Environment:**

- `LANGSMITH_API_KEY`
- `OPENAI_API_KEY`
- `LANGSMITH_TRACING=true`
- `LANGSMITH_ENDPOINT=https://api.smith.langchain.com`
- `LANGSMITH_PROJECT=agentic-ai-handbook`

The notebook prompts for missing keys with `getpass`, but exporting them first is cleaner and keeps secrets out of cell output.

**Read and run, in order:**

1. Dataset concepts: inputs, reference outputs, metadata.
2. Create and inspect the dataset before spending model calls.
3. Define the target function.
4. Design the correctness and concision rubrics.
5. Run `evaluate(...)` and inspect individual examples in LangSmith.
6. Add a deterministic metric such as `under_80_words`.
7. Improve the evaluation set with edge cases and known failures.

**Practice:**

1. Add five examples: one ambiguous question, one where "I do not know" is correct, one long-answer case, one short-answer case, and one production failure with sensitive data removed.
2. Add a third judge for tone or citation quality.
3. Run the same dataset against two different prompts and compare one variable at a time.
4. Read the judge rationales for three low-scoring examples and decide whether the judge or the target is wrong.
5. Version the dataset name and experiment prefix.

**Checkpoint:**

- Why can a toy dataset make a weak system look strong?
- Why is correctness harder to judge without a reference?
- Why should concision never reward missing information?

**Expected result:** a LangSmith dataset, a target application, two model judges, one deterministic metric, and an experiment you can rerun.

---

### Chapter 7: LLM gateway with LiteLLM

**Notebook:** [7. LLM Gateway.ipynb](7.%20LLM%20Gateway.ipynb)

**Goal:** Put one OpenAI-compatible endpoint in front of multiple providers, keys, and deployments, with caching, retries, fallbacks, and limits.

**Prerequisites:** Chapters 1 and 6. You should understand model choice, cost, and evaluation trade-offs.

**Install:**

```powershell
pip install -U "litellm[proxy]" langchain-openai langchain-core pyyaml openai
```

**Environment:**

- `OPENAI_API_KEY` and/or `ANTHROPIC_API_KEY`
- `GOOGLE_API_KEY` if you route Gemini through LiteLLM
- `LITELLM_MASTER_KEY`
- `LITELLM_SALT_KEY` (a long, stable random value)

**Read and run, in order:**

1. SDK, Router, and Proxy: what each layer does.
2. The Router model list and logical model aliases.
3. Create `config.yaml` from the notebook cell and start the proxy.
4. Test the OpenAI-compatible endpoint.
5. Enable caching and observe a cache hit.
6. Configure retries, fallbacks, and deployment-level RPM/TPM limits.
7. Connect LangChain through `ChatOpenAI` and `base_url`.
8. Review the production architecture notes and failure modes.

**Start the gateway in a separate terminal:**

```powershell
litellm --config config.yaml --port 4000
```

If `litellm` is not on your PATH:

```powershell
python -m litellm --config config.yaml --port 4000
```

**Practice:**

1. Add a second deployment to the same `fast-chat` alias with a different key.
2. Add a fallback from `fast-chat` to `strong-chat` and force the primary to fail.
3. Enable response caching and send the same request twice.
4. Set a low RPM limit and confirm the router respects it.
5. List three metrics you would monitor in production and explain what each tells you.

**Checkpoint:**

- How is a gateway alias different from a provider model ID?
- Why is load balancing not the same as pooling provider quota?
- What can change when a fallback model takes over?

**Expected result:** a running LiteLLM proxy, a config with two model groups, a successful fallback, and a LangChain client pointed at the gateway.

---

## Capstone projects

Choose one after Chapter 7. Each project should include a short README of its own with setup, architecture, and evaluation results.

### 1. Guarded research assistant

Combine Chapters 1, 2, 4, and 5.

- LangGraph agent with two MCP tools: web search and document lookup.
- PII redaction on input and output.
- Human approval before any external write.
- A small evaluation set for task success and citation correctness.

### 2. Cited document analyst

Combine Chapters 3 and 6.

- Build a document tree over a public annual report.
- Answer questions with page citations and an explicit "insufficient evidence" path.
- Evaluate page recall, claim support, and citation correctness.
- Compare against a simple vector baseline.

### 3. Multi-model gateway with a budget

Combine Chapters 6 and 7.

- Route simple requests to a fast model and hard requests to a strong model.
- Configure retries, fallbacks, caching, and per-key limits.
- Evaluate quality and cost on the same dataset.
- Report the quality/cost trade-off in a short table.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `ModuleNotFoundError` | Wrong kernel or environment | Activate `.venv`, reinstall requirements, restart the kernel |
| API key not found | Variable not exported or `.env` not loaded | Add `load_dotenv()` as the first cell, or set the variable before launching Jupyter |
| Ollama connection refused | Server not running | Run `ollama serve` in a terminal |
| Model not found in Ollama | Model not pulled | Run `ollama pull qwen2.5-coder:3b` |
| Model returns text instead of tool calls | Model does not support reliable tool calling | Try a hosted model, or a larger local model |
| `GraphRecursionError` | Agent loop exceeded the step limit | Bound the task, add stopping criteria, or raise the recursion limit |
| HITL pauses but will not resume | Missing checkpointer or changed `thread_id` | Reuse the same `config` with the same `thread_id` |
| LangSmith shows no traces | Tracing disabled or key missing | Set `LANGSMITH_TRACING=true`, check the key, restart the kernel |
| LiteLLM proxy will not start | Missing master key, bad YAML, or port in use | Check `config.yaml`, set `LITELLM_MASTER_KEY`, change the port |
| Fallback model breaks tool calls | Fallback lacks tool or JSON support | Test the fallback path explicitly; restrict fallbacks to compatible models |
| Jupyter kernel missing | `ipykernel` not registered | Run the `ipykernel install` command in setup |
| PowerShell blocks `Activate.ps1` | Execution policy | Run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |

### Version drift

These notebooks use modern LangChain and LangGraph APIs, including `create_agent`, middleware, and LangGraph 1.x-style graphs. Library APIs change quickly. If an import or hook signature differs:

1. Check the installed version with `pip show <package>`.
2. Read the official documentation linked at the end of the chapter.
3. Adapt the example rather than downgrading blindly.

---

## Glossary

| Term | Meaning |
|------|---------|
| **Agent** | A model plus a loop that can call tools, observe results, and continue until done |
| **Tool call** | A structured request from the model to run a function |
| **Agent loop** | Model -> tool -> result -> model -> final answer |
| **Structured output** | A response constrained to a schema, often a Pydantic model |
| **Middleware** | Code that wraps agent execution to add behavior such as approval or summarization |
| **Checkpointer** | Storage that saves graph state so a run can pause and resume |
| **Thread** | A named conversation or execution identified by `thread_id` |
| **Reducer** | A rule for combining old and new state values in LangGraph |
| **HITL** | Human in the loop; a person approves, edits, or rejects an action |
| **HITL action** | Approve, edit, or reject a proposed tool call |
| **RAG** | Retrieval-augmented generation; retrieve evidence, then answer from it |
| **Vector RAG** | Retrieval by embedding similarity |
| **Vectorless RAG** | Retrieval by reasoning over a document tree instead of embeddings |
| **PageIndex** | A tree-based document indexing and retrieval approach |
| **MCP** | Model Context Protocol; a standard for exposing tools and resources to AI applications |
| **MCP server** | A process that exposes tools, resources, or prompts over MCP |
| **MCP client** | The application-side connection that discovers and calls MCP capabilities |
| **FastMCP** | A Python framework for building MCP servers and clients |
| **Subagent** | A narrower agent that receives delegated work and returns a result |
| **Deep Agent** | An agent with built-in planning, working files, context management, and delegation |
| **Guardrail** | A control that checks input, agent activity, or output against a policy |
| **PII** | Personally identifiable information |
| **LLM-as-judge** | Using a model to score another model's output against a rubric |
| **Deterministic metric** | A rule-based score such as word count or exact match |
| **Experiment** | One evaluated run of a target over a dataset |
| **LLM gateway** | A central endpoint that routes, caches, limits, and observes model calls |
| **Router** | A component that chooses among deployments or model groups |
| **Fallback** | A secondary model group used when the primary fails |
| **Virtual key** | A gateway-issued key with its own limits and permissions |

---

## References

Official documentation is the source of truth when an API differs from these notebooks.

- LangChain: <https://docs.langchain.com>
- LangGraph: <https://docs.langchain.com/oss/python/langgraph/overview>
- Deep Agents: <https://docs.langchain.com/oss/python/deepagents/overview>
- LangSmith evaluation: <https://docs.smith.langchain.com/evaluation>
- LiteLLM: <https://docs.litellm.ai/docs/>
- LiteLLM proxy architecture: <https://docs.litellm.ai/docs/proxy/architecture>
- LiteLLM routing: <https://docs.litellm.ai/docs/routing>
- FastMCP: <https://gofastmcp.com>
- Model Context Protocol: <https://modelcontextprotocol.io>

Each notebook also ends with a self-test and its own reference list. Use those when you want to go deeper on a single topic.

---

## A final note

The goal is not to memorize every API. The goal is to recognize the shape of a system:

**model -> tools -> state -> guardrails -> evaluation -> operations.**

Once that shape is clear, the specific frameworks become replaceable details. Build the smallest thing that works, then add one layer at a time.
