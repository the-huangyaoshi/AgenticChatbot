# 🤖 LangGraph Agentic AI Chatbot

An intelligent, multi-agent chatbot built with LangGraph, leveraging Groq for LLM inference, FAISS for vector storage, and LangSmith for observability.

## Features

- **Multi-Agent Architecture**: Uses a research team workflow with a manager and two researchers.
- **LangChain Integration**: Built on top of the LangChain ecosystem.
- **RAG Support**: Integrated with FAISS vector store for retrieval-augmented generation.
- **Real-Time Search**: Powered by the Tavily API for up-to-date information.
- **LangSmith Observability**: Track all agent runs, graphs, and tools in the LangSmith dashboard.
- **Web Interface**: A simple Streamlit UI to interact with the chatbot.

## 🛠️ Tech Stack

- **Orchestration**: [LangGraph](https://langchain.com/langgraph)
- **LLM**: [Groq](https://groq.com)
- **Vector Store**: [FAISS](https://faiss.ai)
- **Search**: [Tavily API](https://tavily.com)
- **UI**: [Streamlit](https://streamlit.io)
- **Observability**: [LangSmith](https://smith.langchain.com)

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- API Keys:
  - `GROQ_API_KEY`
  - `TAVILY_API_KEY`
  - `LANGSMITH_API_KEY`

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Section20-AgenticChatbot
```

2. Create and activate a virtual environment:
```bash
python -m venv .venv
.\.venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

### Configuration

Create a `.env` file in the root directory with your API keys:
```env
OPENAI_API_KEY=your-openai-key
GROQ_API_KEY=your-groq-key
TAVILY_API_KEY=your-tavily-key
LANGSMITH_API_KEY=your-langsmith-key
LANGSMITH_TRACING=true
LANGSMITH_PROJECT=agentic-chatbot
```

## ▶️ Running the App

Start the Streamlit application:

```bash
streamlit run app.py
```

The application will open in your browser (usually at `http://localhost:8501`).

## 🏗️ Project Structure

```
Section20-AgenticChatbot/
├── src/
│   └── langgraphagenticai/
│       ├── config/
│       │   └── settings.py         # Application settings
│       ├── tools/
│       │   ├── research_tools.py     # Tavily search tools
│       │   └── database_tools.py     # FAISS database operations
│       ├── agents/
│       │   ├── research_agents.py    # Researcher and Manager agents
│       │   └── research_graph.py     # LangGraph state and workflow
│       └── main.py                 # Main app entry point
├── data/                             # Vector store files
├── .env                              # Environment variables
├── requirements.txt                  # Project dependencies
└── app.py                            # Streamlit UI
```

## 📝 Workflow

1. **User Query**: The user asks a question via the Streamlit UI.
2. **Manager Agent**: The Manager agent receives the query and decides if it needs external research or database lookup.
3. **Researcher Agents**: 
   - If research is needed, one researcher searches the web using Tavily.
   - If domain knowledge is needed, another researcher queries the FAISS database.
4. **Synthesis**: The Manager agent combines the results and generates a final answer.
5. **Observability**: Every step is logged to LangSmith, allowing you to trace the execution flow and debug issues.

## 🔍 LangSmith Observability

To view the traces, visit [smith.langchain.com](https://smith.langchain.com). You should see a new project named `agentic-chatbot` (or as configured in `.env`).

You can inspect:
- **Graph Runs**: Visual representation of the workflow.
- **Agent States**: See inputs and outputs of each agent.
- **Tools**: Track which tools were called and their results.
- **Latency**: Monitor the response time of each step.

## 🗄️ Data & RAG

The system uses a FAISS index for RAG. You can pre-populate it by running:
```bash
python src/langgraphagenticai/tools/database_tools.py --embed
```
This will embed documents in the `data` folder and create the FAISS index.

## 🧩 Extending the System

- **Add New Tools**: Create a new file in `src/langgraphagenticai/tools/` and register it in `ResearchAgents`.
- **Modify Agents**: Edit the prompts and configurations in `src/langgraphagenticai/agents/research_agents.py`.
- **Change LLM**: Update `src/langgraphagenticai/config/settings.py` to use a different model.
- **New Workflow**: Define a new graph in `src/langgraphagenticai/agents/` and update `main.py` to expose it.