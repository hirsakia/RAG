# Local Agentic RAG Evolution
This repository contains a progressive series of Jupyter Notebooks demonstrating the evolution of a fully local, privacy-preserving Retrieval-Augmented Generation (RAG) system. Starting from a basic LangChain pipeline, the system evolves into a complex, multi-agent LangGraph architecture featuring hybrid retrieval, contextual chunking, SQL database routing, and Self-RAG hallucination mitigation.

All processing, including embeddings, vector storage, and Large Language Model (LLM) generation, happens locally.

## 🚀 The Evolution
### V1: The Baseline Local Conversational RAG

The foundational ETL (Extract, Transform, Load) pipeline and an LCEL-based (LangChain Expression Language) conversational chain.

Ingestion: Parses unstructured text (.txt) and digital PDFs using LangChain's DirectoryLoader.

Chunking: RecursiveCharacterTextSplitter ensures logical semantic breaks.

Vector Store: In-memory ChromaDB using lightweight HuggingFace embeddings (all-MiniLM-L6-v2).

Memory: Implements a chat history buffer to maintain context across multi-turn conversations (e.g., resolving pronouns).

Grounding: Strict prompt engineering ensures the LLM cites source filenames.

### V2: Advanced Retrieval & Agentic Routing

Introduces LangGraph to transform the static chain into a dynamic state machine, significantly improving retrieval accuracy.

Hybrid Search: Combines dense semantic search (ChromaDB) with sparse keyword search (BM25) to catch exact matches and conceptual overlaps.

Query Transformations: * Contextualization: Rewrites follow-up questions into standalone queries before searching.

Expansion: Generates 3 related search queries to broaden recall.

Reranking: Utilizes the FlashRank cross-encoder (ms-marco-MultiBERT-L-12) to select the top 4 most relevant chunks from the expanded search pool.

### V3: Multi-modal Routing (SQL + Docs)

Adds structured data querying by integrating a PostgreSQL database, introducing complex routing challenges.

LLM Routing: The graph dynamically decides whether to query the vector database (unstructured) or write SQL (structured) based on the user's prompt.

Text-to-SQL: Automatically generates and executes PostgreSQL queries against a local database.

Note: This version highlights a common vulnerability in LLM-based routing where topic overlap (e.g., "Deep Learning" as a course name vs. a research paper topic) causes incorrect pathing.

### V4: Self-RAG & Contextual Retrieval (State-of-the-Art)

A robust, production-ready architecture fixing V3's routing bugs and implementing advanced Anthropic/Self-RAG techniques.

Contextual Chunking (Anthropic Technique): Prepend an LLM-generated summary of the parent document to each chunk before embedding. This provides global context and reduces "lost in the middle" retrieval failures.

Upgraded Embeddings: Swapped to BAAI/bge-base-en-v1.5 and added persistent ChromaDB storage to skip rebuilding on subsequent runs.

Semantic Router: Replaced the unreliable LLM router with an embedding-based cosine-similarity router, completely fixing V3's routing bug.

Self-RAG (Quality Control):

Doc Grader: Filters out irrelevant retrieved chunks before reranking.

Answer Grader: Checks the final output for hallucinations (e.g., if the LLM admits ignorance). If retrieval fails, the graph loops back and automatically retries.

## 🛠️ Technology Stack
Orchestration: LangChain, LangGraph

Local LLM: Ollama (llama3.2 for text generation, llama3.1:8b for SQL generation)

Embeddings: HuggingFace (BAAI/bge-base-en-v1.5, all-MiniLM-L6-v2)

Vector Database: ChromaDB

Relational Database: PostgreSQL (psycopg2)

Reranker: FlashRank

## ⚙️ Setup & Installation
1. Clone the repository and set up a virtual environment:

```Bash
git clone <your-repo-url>
cd <your-repo-name>
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```
2. Install dependencies:
```
Bash
pip install langchain langchain-ollama langchain-huggingface langchain-community chromadb flashrank langgraph psycopg2-binary sentence-transformers jupyter
```
3. Install and run Ollama:
Download Ollama from ollama.com. Once installed, pull the required models:
```
Bash
ollama pull llama3.2
ollama pull llama3.1:8b
```
4. Set up the Data:

Create a folder named RAG_data in the root directory.

Place your .txt and .pdf files inside this folder.

(For V3/V4) Ensure PostgreSQL is running locally and create a database named rag_db.

5. Run the notebooks:
```
Bash
jupyter notebook
```
