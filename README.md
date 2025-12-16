DeepWiki – Local AI Research Assistant
![DeepWiki Screenshot]( is a fully local, AI‑powered research assistant that turns a single query into multi‑step web research and a structured markdown report. It uses an Ollama‑hosted LLM for reasoning, Tavily for web search, and a LangGraph workflow to orchestrate the whole research process through an interactive graph UI in LangGraph Studio.​​

Features
Multi‑step autonomous research: generates sub‑questions, searches the web, reads sources, reflects, and synthesizes a final answer.

Runs on your machine using Docker, Node.js/TypeScript, and LangGraph.js.​

Uses a local Ollama model (e.g. llama3.2:latest) for all reasoning and writing, with Tavily providing fresh web results.​​

Architecture
LangGraph server (Docker, port 2024): Hosts the research workflow and exposes an HTTP API for LangGraph Studio.​

Ollama (port 11434): Serves the local LLM used for query generation, summarization, reflection, and report writing.​​

Tavily API: Performs web searches, returning URLs and snippets that the LLM then reads and summarizes.​

LangGraph Studio: Browser UI where you connect to http://localhost:2024, run the agent graph, and watch each node execute in real time.​

Prerequisites
Node.js 18+ and Yarn installed.​

Docker Desktop running.​

Ollama installed with a model pulled, e.g.:

bash
ollama pull llama3.2:latest
Tavily API key (free tier is enough for testing).​

Setup
Clone the repository and install dependencies:

bash
git clone <your-fork-url>.git
cd deepwiki
yarn install
Create a .env file:

text
TAVILY_API_KEY=your_tavily_key_here
LLM_BASE_URL=http://localhost:11434
In the configuration file (for example src/agent/configuration.ts), set:

ts
localLlm: configurable?.localLlm || "llama3.2:latest",
searchApi: configurable?.searchApi || SearchAPI.TAVILY,
Running
Start Ollama (ensure http://localhost:11434/api/tags responds).​​

Start the research server with Docker:

bash
docker compose up -d
Confirm the container is running on port 2024:

bash
docker ps
Using DeepWiki
Open LangGraph Studio in your browser:

text
https://smith.langchain.com/studio/?baseUrl=http://localhost:2024
Select the agent graph and go to the Graph view.​

In the Input panel, set ResearchTopic to something like:
PDS corruption in India and smart ration card solutions.

Click Submit and watch nodes like generateQuery, webResearch, summarizeSources, reflectOnSummary, and finalizeSummary execute.

Read the final markdown report and source citations in the right‑hand trace panel.

Customization
Swap models by changing localLlm to any installed Ollama model name (for example mistral:7b or deepseek-r1:8b).​​

Adjust depth vs speed by tuning the max research loop count in the configuration.​

Extend the graph with extra nodes (e.g. PDF export, custom ranking) using LangGraph.js.​
