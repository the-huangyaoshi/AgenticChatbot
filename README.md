# LangGraph Agentic AI Chatbot

A Streamlit-based application that builds stateful AI graphs using LangGraph. The application leverages Groq as its LLM and Tavily for web search capabilities.

## ✨ Features and Use Cases

The application currently implements three distinct workflows, configurable via the UI:

1. **Basic Chatbot**: A simple single-node graph that directly invokes a Groq LLM with the user's messages.
2. **Chatbot With Web**: An advanced graph that binds the Tavily Search tool to the Groq LLM. It uses LangGraph's conditional edges (`tools_condition`) to route requests to a `ToolNode` when the LLM decides to search the web, allowing it to answer questions using real-time information.
3. **AI News**: A sequential 3-node graph (`fetch_news` -> `summarize_news` -> `save_result`) that fetches AI technology news using Tavily, summarizes the articles into Markdown format using a Groq LLM, and writes the results to an `AINews/` directory.

## 🛠️ Tech Stack

- **Orchestration**: `langgraph` (StateGraph, ToolNode, tools_condition)
- **LLM**: `langchain_groq` (ChatGroq)
- **Search**: `tavily-python` (TavilyClient) and `langchain_community.tools.tavily_search` (TavilySearchResults)
- **UI**: `streamlit`

## 🚀 Getting Started

### Prerequisites

- Python environment
- API Keys:
  - `GROQ_API_KEY`
  - `TAVILY_API_KEY`

### Installation

1. Clone the repository:
```bash
git clone https://github.com/the-huangyaoshi/AgenticChatbot.git
cd AgenticChatbot
```

2. Create and activate a virtual environment:
```bash
python -m venv .venv
# On Windows:
.\.venv\Scripts\activate
# On Mac/Linux:
source .venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

### Configuration

You can provide your API keys directly in the Streamlit UI sidebar, or configure them via environment variables.

## ▶️ Running the App Locally

Start the Streamlit application:

```bash
streamlit run app.py
```

## 🏗️ Project Structure

The project is structured into the following modules:

```
Section20-AgenticChatbot/
├── app.py                              # Entry point that runs load_langgraph_agenticai_app()
├── AINews/                             # Directory where AI News markdown summaries are saved
├── src/
│   └── langgraphagenticai/
│       ├── main.py                     # Orchestrates UI loading, LLM setup, and Graph execution
│       ├── graph/
│       │   └── graph_builder.py        # Contains GraphBuilder which constructs StateGraph for the 3 use cases
│       ├── LLMS/
│       │   └── groqllm.py              # Initializes ChatGroq with the provided API key and model
│       ├── nodes/
│       │   ├── ai_news_node.py         # Defines AINewsNode (fetch_news, summarize_news, save_result)
│       │   ├── basic_chatbot_node.py   # Defines BasicChatbotNode (invokes LLM)
│       │   └── chatbot_with_Tool_node.py # Defines ChatbotWithToolNode (binds Tavily tool to LLM)
│       ├── state/
│       │   └── state.py                # Defines the TypedDict State containing 'messages'
│       ├── tools/
│       │   └── search_tool.py          # Configures TavilySearchResults tool and creates ToolNode
│       └── ui/
│           ├── streamlitui/
│           │   ├── display_result.py   # Handles displaying graph.stream() and graph.invoke() outputs in Streamlit
│           │   └── loadui.py           # Configures the Streamlit sidebar, inputs, and session state
│           ├── uiconfigfile.ini        # Defines page title, LLM options, and Use Case options
│           └── uiconfigfile.py         # Parses uiconfigfile.ini using ConfigParser
```

## 🌐 Deployment on Hugging Face Spaces

To deploy this Streamlit application to Hugging Face Spaces:

1. **Create a New Space**: Go to Hugging Face Spaces, create a new Space, and select **Streamlit** as the SDK.
2. **Upload Files**: Upload the entire repository (including `app.py`, `requirements.txt`, and the `src/` folder), but exclude `.venv` and `__pycache__`.
3. **Configure Secrets**: Go to your Space's Settings > Variables and secrets, and add `GROQ_API_KEY` and `TAVILY_API_KEY`.
4. **Launch**: Hugging Face will automatically install the dependencies in `requirements.txt` and run `app.py`.

## ☁️ Deployment on Google Cloud Platform (GCP)

To deploy this application to GCP, the easiest and most cost-effective method is **Google Cloud Run**. A `Dockerfile` is already included in this repository.

1. **Install and authenticate with Google Cloud SDK**:
   - Install the [gcloud CLI](https://cloud.google.com/sdk/docs/install).
   - Run `gcloud auth login` and `gcloud config set project [YOUR_PROJECT_ID]`.

2. **Enable Required APIs**:
   Ensure Cloud Build and Cloud Run are enabled in your project:
   ```bash
   gcloud services enable cloudbuild.googleapis.com run.googleapis.com
   ```

3. **Deploy to Cloud Run**:
   Run the following command from the root of this repository (where the `Dockerfile` is located):
   ```bash
   gcloud run deploy agentic-chatbot --source . --region us-central1 --allow-unauthenticated --port 8501
   ```
   *(Note: You can change the `--region` to one closest to you.)*

4. **Configure Secrets**:
   Once deployed, go to the **Cloud Run** console in GCP:
   - Select your service (`agentic-chatbot`).
   - Click **Edit & Deploy New Revision**.
   - Under the **Variables & Secrets** tab, add `GROQ_API_KEY` and `TAVILY_API_KEY` as environment variables.
   - Click **Deploy** to apply the keys.

5. **Launch**:
   Once the deployment is complete, `gcloud` will provide a secure HTTPS URL where your Streamlit app is hosted!