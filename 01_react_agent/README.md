# 01 — ReAct Research Agent

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/ai-agents-lab/blob/main/01_react_agent/react_agent.ipynb)

> An autonomous research agent that searches the web, reads results, and decides when it has enough information to write a structured report — without any hardcoded steps.

---

## What You Will Learn 

1. How to make a simple web search AI agent simply with python functions, without using any frameworks like Langchain etc. 
2. Understand what happens under the hood and clarify what is "autonomous" about AI agents and how is it different from a workflow
3. Get familiar with the world of experimentation using free tier LLM models, APIs, Google collab etc.

## What This Agent Does

You give it any research question. It autonomously:

1. Decides what to search for (and how many searches are needed)
2. Calls a web search tool via Tavily
3. Reads and interprets the results
4. Decides if it needs more information
5. Repeats until confident, then writes a structured report

No scripted pipeline. The LLM drives every decision.

---

## The ReAct Pattern

Based on [Yao et al., 2022](https://arxiv.org/abs/2210.11610):

```
REASON  →  What do I need to know? What gaps exist?
ACT     →  Call search_web(query)
OBSERVE →  Read and interpret results
DECIDE  →  Enough info? YES → write report. NO → reason again.
```

This loop runs autonomously until the LLM decides it's done, with a `max_iterations` safety cap to prevent runaway agents.

---

## Architecture

```
User Query
    │
    ▼
┌─────────────────────────────────┐
│         run_agent()             │  ← the loop
│                                 │
│  ┌─────────────────────────┐    │
│  │      call_llm()         │    │  ← REASON
│  │  (full history sent)    │    │
│  └────────────┬────────────┘    │
│               │                 │
│      tool_call? ──── no ──────────────→ Final Answer
│               │ yes             │
│               ▼                 │
│  ┌─────────────────────────┐    │
│  │  execute_tool_call()    │    │  ← ACT
│  │  → TOOL_REGISTRY lookup │    │
│  │  → search_web()         │    │
│  │  → Tavily API           │    │
│  └────────────┬────────────┘    │
│               │                 │
│               ▼                 │
│      append result to history   │  ← OBSERVE
│               │                 │
│               └── loop back to call llm() ───┘
└─────────────────────────────────┘
```

---

## Key Design Decisions

| Decision | Why |
|----------|-----|
| Full history passed on every LLM call | In-context memory — agent remembers all prior searches |
| `temperature=0.2` | Low = deterministic, reliable tool calling |
| `max_output_tokens=4096` | Prevents response truncation on long reports |
| `max_iterations=10` | Safety cap — prevents infinite loops |
| Results truncated to 500 chars | Context budget management |
| Today's date injected into system prompt | Prevents LLM anchoring to training cutoff |
| Retry logic in `search_web()` | Handles transient network failures gracefully |
| `TOOL_REGISTRY` dict | Clean separation — adding new tools requires zero changes to agent loop |

---

## Multiple Tool Calling

When the LLM identifies multiple independent sub-questions, it emits several `function_call` parts in a single response. The agent loop executes all of them before the next LLM call:

```
Iteration 1:
  LLM emits 3 × function_call (independent queries)
  Loop executes all 3 → results appended together
  
Iteration 2:
  LLM reads all 3 results → writes final report
  
Total: 2 iterations instead of 4
```

---

## Notebook Structure

| Section | What it covers |
|---------|---------------|
| 1–3 | Imports, API setup, client initialization |
| 4 | `search_web()` — Tavily wrapper with retry logic |
| 5 | Tool registry |
| 6 | Tool schema — the JSON contract sent to the LLM |
| 7 | System prompt — agent identity and behavior |
| 8 | `call_llm()` — the think step |
| 9 | `execute_tool_call()` — the act step |
| 10 | `run_agent()` — the full ReAct loop |
| 11 | Run + experiments |

---

## APIs Required

| Secret Name | Where to get it |
|-------------|----------------|
| `GEMINI_API_KEY` | [Google AI Studio](https://aistudio.google.com) |
| `TAVILY_API_KEY` | [Tavily](https://tavily.com) |

Both have free tiers sufficient to run all experiments in this notebook.

---

## Sample Output

```
🚀 AGENT STARTED
📋 Query: What is MoE architecture in LLMs?

🔄 ITERATION 1
  → search_web('MoE architecture LLMs explained')       [3 results]
  → search_web('importance of MoE in LLMs')             [3 results]  
  → search_web('major LLMs using MoE architecture 2025')[5 results]

🔄 ITERATION 2
  ✅ Final answer delivered

✔  Completed in 2 iteration(s)
```
