# DeepWiki – Local AI Research Assistant

![DeepWiki Screenshot](Screenshot_22.jpg)

DeepWiki is a fully local, AI‑powered research assistant that turns a single question into multi‑step web research and a structured markdown report. It uses an Ollama‑hosted LLM for reasoning, Tavily for web search, and a LangGraph workflow you can inspect and control visually in LangGraph Studio. 

---

## ✨ Features

- **Autonomous research pipeline** – generates sub‑questions, searches the web, reads sources, reflects, and synthesizes a final answer.  
- **Local‑first AI** – all reasoning and writing runs on your machine via Ollama (e.g. `llama3.2:latest`). 
- **Visual workflow** – watch each node in the research graph execute in LangGraph Studio, step by step.  
- **Web‑scale context** – Tavily search pulls fresh sources and passes them to the LLM for summarization and analysis.   

---

## 🧱 Architecture

- **LangGraph server (Docker, port 2024)**  
  Hosts the research workflow and exposes an HTTP API for LangGraph Studio. 

- **Ollama (port 11434)**  
  Serves the local LLM used for query generation, summarization, reflection, and report writing. 

- **Tavily API**  
  Performs web searches and returns URLs + snippets that the LLM then reads and summarizes. 

- **LangGraph Studio**  
  Browser UI where you connect to `http://localhost:2024`, select the `agent` graph, and watch research runs live. 

---

## ✅ Prerequisites

- Node.js **18+** and **Yarn** installed.  
- Docker Desktop running.   
- Ollama installed with at least one model, for example:  
ollama pull llama3.2:latest

- Tavily API key (free tier is enough for testing). 

---

## 🚀 Setup

1. **Clone and install**

git clone <your-fork-url>.git
cd deepwiki
yarn install

2. **Configure environment**

Create a `.env` file:

TAVILY_API_KEY=your_tavily_key_here
LLM_BASE_URL=http://localhost:11434

3. **Point to your model**

In `src/agent/configuration.ts`, ensure:

localLlm: configurable?.localLlm || "llama3.2:latest",
searchApi: configurable?.searchApi || SearchAPI.TAVILY,

---

## ▶️ Running DeepWiki

1. **Start Ollama**

Confirm it’s alive:

curl http://localhost:11434/api/tags

2. **Start the research server**

docker compose up -d

Check it’s up on port 2024:

docker ps

3. **Open LangGraph Studio**

https://smith.langchain.com/studio/?baseUrl=http://localhost:2024
---

## 🧪 Using the Agent

1. In Studio, open the **Graph** view for the `agent` graph. 
2. In the **Input** panel, set `ResearchTopic`, for example:  

> `PDS corruption in India and smart ration card solutions` 

3. Click **Submit** and watch nodes like `generateQuery`, `webResearch`, `summarizeSources`, `reflectOnSummary`, and `finalizeSummary` execute. 
4. Read the final markdown report and source citations in the trace panel on the right.  

---

## 🛠 Customization

- Swap to any Ollama model by changing `localLlm` (e.g. `mistral:7b`, `deepseek-r1:8b`). 
- Tune depth vs speed by adjusting the maximum research loop count in the config.   
- Extend the graph with extra nodes (PDF export, custom ranking, domain‑specific tools) using LangGraph.js. 
