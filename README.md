# 🔬 Multi-Agent Research System

A multi-agent AI research system built with **LangChain** that automates the research process from web search to report generation and quality evaluation.

The system uses specialized agents and chains to search for information, extract relevant content from web pages, generate a structured research report, and evaluate the final output.

## 🌟 Features

- 🔎 **Search Agent** — Searches the web using the Tavily API to find relevant sources.
- 📖 **Reader Agent** — Fetches and parses web pages using Requests and BeautifulSoup4 to extract useful content.
- ✍️ **Writer Chain** — Synthesizes the collected research into a structured Markdown report.
- 🧐 **Critic Chain** — Reviews the generated report, evaluates its quality, identifies strengths, and suggests improvements.
- 🔗 **Multi-Agent Pipeline** — Connects all stages into a complete research workflow.
- 🧠 **Shared State** — A central Python dictionary passes research results between different stages of the pipeline.
- 🖥️ **Streamlit Interface** — Provides an interactive web interface for submitting research topics and viewing results.
- 🧩 **Modular Architecture** — Agents, tools, chains, and pipeline logic are separated for easier extension.

## 🏗️ Architecture

```text
                         ┌──────────────────────────┐
                         │       Streamlit UI       │
                         │          app.py          │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │     Research Pipeline    │
                         │       pipeline.py        │
                         │                          │
                         │    Shared State (dict)   │
                         └────────────┬─────────────┘
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                         ▼                         ▼
                ┌─────────────────┐       ┌─────────────────┐
                │   Search Agent  │       │   Reader Agent  │
                │                 │       │                 │
                │    Tavily API   │       │ Requests + BS4  │
                └────────┬────────┘       └────────┬────────┘
                         │                         │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │      Writer Chain        │
                         │                          │
                         │ ChatPromptTemplate       │
                         │        + LLM             │
                         │        + LCEL             │
                         │   + StrOutputParser      │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │      Critic Chain        │
                         │                          │
                         │ Reviews generated report │
                         │ Scores & suggests fixes  │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │    Final Research Report │
                         └──────────────────────────┘
```

## 🔄 Research Workflow

The system follows a multi-stage research pipeline:

```text
Research Topic
      │
      ▼
┌───────────────┐
│ Search Agent  │
└───────┬───────┘
        │
        │ Search results & URLs
        ▼
┌───────────────┐
│ Reader Agent  │
└───────┬───────┘
        │
        │ Extracted web content
        ▼
┌───────────────┐
│ Writer Chain  │
└───────┬───────┘
        │
        │ Draft report
        ▼
┌───────────────┐
│ Critic Chain  │
└───────┬───────┘
        │
        │ Evaluation & feedback
        ▼
 Final Research Report
```

## 🤖 Agents & Components

### 🔎 Search Agent

The Search Agent uses **Tavily** to search the web for information related to the user's research topic.

It retrieves relevant search results, URLs, and snippets that are passed to the next stage of the pipeline.

### 📖 Reader Agent

The Reader Agent uses **Requests** and **BeautifulSoup4** to retrieve and process web pages.

The scraping process removes unnecessary HTML elements such as:

- `<script>`
- `<style>`
- `<nav>`
- `<footer>`

The remaining content is converted into readable text that can be used by the Writer Chain.

### ✍️ Writer Chain

The Writer Chain uses **LangChain Expression Language (LCEL)** to connect:

```text
ChatPromptTemplate
       │
       ▼
      LLM
       │
       ▼
StrOutputParser
```

The chain combines the collected research and generates a structured Markdown research report containing relevant findings, analysis, and citations.

### 🧐 Critic Chain

The Critic Chain evaluates the generated report.

It analyzes aspects such as:

- Accuracy
- Relevance
- Completeness
- Structure
- Clarity
- Quality of research

The critic provides a score and identifies strengths and areas that could be improved.

## 🧠 Shared State

The research pipeline uses a Python dictionary as shared state between the different stages.

```text
Search Agent
     │
     ▼
  state
     │
     ├── Search Results
     │
     ├── Extracted Content
     │
     ├── Generated Report
     │
     └── Critic Feedback
```

This allows each stage of the pipeline to access the information generated by previous stages.

## 🛠️ Tech Stack

### Core Framework

- **Python**
- **LangChain**
- **LangChain Agents**
- **LCEL**
- **ChatPromptTemplate**
- **StrOutputParser**

### AI / LLM

- **OpenAI API**
- **ChatOpenAI**
- **GPT-4o-mini**

The LLM layer can also be adapted to other supported models/providers such as Google Gemini, Mistral, or Groq.

### Search & Web Processing

- **Tavily**
- **Requests**
- **BeautifulSoup4**

### Frontend

- **Streamlit**

The Streamlit application provides the user interface for submitting research topics, displaying the progress of the research pipeline, viewing the generated report, and downloading the output.

### Environment & Utilities

- **python-dotenv** — Loads API keys from `.env`
- **uv** — Python environment and package management
- **rich** — Formatted terminal output
- **VS Code** — Development environment

## 📋 Prerequisites

Before running the project, make sure you have:

- Python 3.10+
- OpenAI API key
- Tavily API key
- Internet connection
- `uv` or `pip`

## 🚀 Quick Start

### 1. Download the Repository

Download the repository and extract it.

Then open the project directory:

```bash
cd Multi-agent-research-system
```

### 2. Create a Virtual Environment

Using `uv`:

```bash
uv venv
```

Activate the environment:

**macOS / Linux**

```bash
source .venv/bin/activate
```

**Windows**

```bash
.venv\Scripts\activate
```

### 3. Install Dependencies

Using `uv`:

```bash
uv pip install -r requirements.txt
```

Or using pip:

```bash
pip install -r requirements.txt
```

### 4. Configure API Keys

Create a `.env` file in the project root:

```env
OPENAI_API_KEY="your_openai_api_key"
TAVILY_API_KEY="your_tavily_api_key"
```

> ⚠️ Never commit your `.env` file or expose your API keys publicly.

### 5. Run the Application

Start the Streamlit application:

```bash
streamlit run app.py
```

The application will open in your browser.

## 💬 Example

Enter a research topic such as:

```text
What are the latest developments in generative AI?
```

The system then:

```text
1. Searches for relevant sources
        ↓
2. Reads and extracts webpage content
        ↓
3. Generates a structured research report
        ↓
4. Critically evaluates the report
        ↓
5. Displays the final result
```

## 📁 Project Structure

```text
Multi-agent-research-system/
│
├── agents.py              # Agent and chain definitions
│
├── app.py                 # Streamlit application
│
├── pipeline.py            # Research pipeline and shared state
│
├── tools.py               # Search and web scraping tools
│
├── requirements.txt       # Python dependencies
│
├── .gitignore             # Git ignored files
│
└── README.md              # Project documentation
```

## 🔧 Key Technologies Explained

| Technology | Purpose |
|---|---|
| **LangChain** | Agent and LLM orchestration |
| **OpenAI** | LLM used for research generation |
| **Tavily** | Live web search |
| **BeautifulSoup4** | HTML parsing and text extraction |
| **Requests** | Fetching web pages |
| **Streamlit** | Interactive web interface |
| **LCEL** | Building runnable LangChain pipelines |
| **python-dotenv** | Secure environment variable loading |
| **uv** | Python environment and dependency management |
| **rich** | Formatted terminal output |

## 🔮 Future Improvements

Potential improvements for the system include:

- Parallel research across multiple sources
- Automatic source credibility scoring
- Better citation management
- Research history and persistent memory
- Improved critic feedback loops
- Automatic report export to PDF
- Additional specialized research agents
- Support for multiple LLM providers
- Asynchronous research execution
- Improved error handling and retry mechanisms

## 👨‍💻 Author

**Vamsi Kanithi**

A project exploring **multi-agent AI systems, LLM orchestration, automated web research, and agentic workflows**.
