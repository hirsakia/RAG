# Local RAG Pipeline Evolution

**Production-grade local-first RAG system** built with LangChain, LangGraph, Ollama (Llama 3.2), ChromaDB, BM25, FlashRank, and PostgreSQL.

## Overview
Progressive versions of a fully local conversational RAG:
- **V1**: Basic retrieval + conversational memory
- **V2**: Hybrid retrieval (BM25 + vector), query expansion, FlashRank reranking, retry loop
- **V3**: LangGraph orchestration + intelligent SQL vs. Docs routing

## Features
- Hybrid retrieval (keyword + semantic)
- Query contextualization & expansion
- FlashRank cross-encoder reranking
- Automatic SQL routing (PostgreSQL)
- Quality-check retry loop
- Multi-turn chat history
- Strict source citations

## Tech Stack
- **LLM**: Ollama (llama3.2)
- **Vector DB**: ChromaDB
- **Keyword**: BM25Retriever
- **Reranker**: FlashRank (ms-marco-MultiBERT-L-12)
- **Orchestration**: LangGraph
- **DB**: PostgreSQL + SQLDatabase
- **Embeddings**: all-MiniLM-L6-v2

## Quick Start
```bash
pip install -r requirements.txt
ollama pull llama3.2
jupyter notebook RAG-V3.ipynb
