# 🤖 AI Agents Lab

> DIY Easy to follow and learn; Hands-on implementations of AI agent architectures — built with real APIs, production-relevant patterns, and heavily annotated for learning.

## What You Will Learn
- Get familiar with the world of experimentation using free tier LLM models, APIs, Google collab etc.
- Understand what happens under the hood and clarify what is "autonomous" about AI agents and how is it different from a static workflow
- Start with a simple web search AI agent simply with python functions, without using any frameworks like Langchain etc.
- Move forward from beginner to advanced AI agents using frameworks like langChain, langGraph

## 📦 Agents

| # | Agent | Core Concept | Stack | Notebook |
|---|-------|-------------|-------|----------|
| 01 | ReAct Research Agent | Reason → Act → Observe loop | Gemini + Tavily | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/mj-labs-sys/ai-agents-lab/blob/main/01_react_agent/react_agent.ipynb) |
| 02 | RAG Agent | Retrieval Augmented Generation | Coming soon | — |
| 03 | Multi-Agent System | Agent orchestration | Coming soon | — |
and more..

---

## 🧠 Concepts Covered

**Agent Fundamentals**
- ReAct pattern — Reason, Act, Observe loop (Yao et al., 2022)
- Agentic loops — autonomous multi-step decision making
- Tool calling — LLM-driven function execution via JSON schemas

**Memory**
- In-context memory — conversation history as short-term memory
- Vector stores — long-term memory via embeddings (used in RAG agent)

**Grounding & Reliability**
- Web search grounding to reduce hallucinations
- Retry logic and error handling in production agents
- Date injection to prevent training-cutoff anchoring

---

## ⚙️ Setup

All notebooks are designed to run on **Google Colab** — no local installation needed.

### Step 1 — Get API Keys

| Key | Where to get it | Free tier |
|-----|----------------|-----------|
| `GEMINI_API_KEY` | [Google AI Studio](https://aistudio.google.com) | ✅ Yes |
| `TAVILY_API_KEY` | [Tavily](https://tavily.com) | ✅ 1000 searches/month |

### Step 2 — Add to Colab Secrets

1. Open any notebook in Colab
2. Click the 🔑 **Secrets** icon in the left sidebar
3. Add each key with the exact name shown above
4. Enable access for the notebook

### Step 3 — Run

Click the **Open in Colab** badge next to any agent above.

### Running Locally (Optional)

```bash
git clone https://github.com/mj-labs-sys/ai-agents-lab.git
cd ai-agents-lab
pip install -r requirements.txt
jupyter notebook
```

Then add your API keys to a `.env` file (never commit this):
```
GEMINI_API_KEY=your_key_here
TAVILY_API_KEY=your_key_here
```

---

## 📁 Repository Structure

```
ai-agents-lab/
│
├── 01_react_agent/
│   ├── react_agent.ipynb      ← fully annotated notebook
│   └── README.md              ← architecture + design decisions
│
├── 02_rag_agent/              ← coming soon
├── 03_multiagent_system/      ← coming soon
│
├── assets/                    ← diagrams and images
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 📚 References

- [ReAct: Synergizing Reasoning and Acting in LMs](https://arxiv.org/abs/2210.11610) — Yao et al., 2022
- [Toolformer: Language Models Can Teach Themselves to Use Tools](https://arxiv.org/abs/2302.04761)
- [Gemini API Documentation](https://ai.google.dev/docs)
- [Tavily Search API](https://docs.tavily.com)

---

## 🤝 Contributing

This is a learning-focused repository. If you spot an error or have a suggestion, open an issue.

---

*Notebooks are heavily commented — every design decision is explained inline. Built for understanding, not just running.*
