# n8n RAG Pipeline with Supabase Vector Store and PDF Documents

## Project Overview
This repository contains an end-to-end Retrieval-Augmented Generation (RAG)
implementation built using **n8n**, **Supabase**, and an **AI Agent**.
The system enables natural language question answering over PDF documents
by combining document retrieval with large language models.

The workflow is designed to ingest PDF files, generate embeddings, store them
in a vector database, and retrieve relevant context at query time to produce
accurate, grounded answers.

---

## High-Level Architecture

The project consists of two main pipelines:

1. **Document Ingestion & Indexing Pipeline**
2. **Question Answering (RAG) Pipeline**

Both pipelines are orchestrated entirely within **n8n**.

---

## 1. Document Ingestion & Indexing Pipeline

This pipeline is responsible for transforming PDF documents into searchable
vector embeddings.

### Flow Description

1. **Manual Trigger**
   - The workflow is triggered manually using the `Execute Workflow` node.

2. **Google Drive – Download File**
   - A PDF file is downloaded from a configured Google Drive location.
   - This allows flexible document management without manual uploads.

3. **Default Data Loader**
   - The PDF file content is extracted.
   - The document is split into smaller semantic chunks to improve retrieval accuracy.

4. **Embedding Generation**
   - Each chunk is converted into a vector embedding using **Google Gemini Embeddings**.
   - Embeddings capture the semantic meaning of the text.

5. **Supabase Vector Store**
   - The generated embeddings are stored in **Supabase (PostgreSQL + vector extension)**.
   - Each chunk is persisted with its associated metadata.

This pipeline runs once per document and prepares the knowledge base for querying.

---

## 2. Question Answering (RAG) Pipeline

This pipeline handles user questions and produces answers grounded in the indexed PDFs.

### Flow Description

1. **Chat Trigger**
   - Activated when a user sends a message via the chat interface.

2. **AI Agent**
   - Acts as the central orchestrator.
   - Receives the user question and decides how to retrieve context.

3. **Supabase Vector Store (Retrieval)**
   - The user query is embedded.
   - A similarity search is performed against stored document embeddings.
   - The most relevant document chunks are retrieved.

4. **Chat Memory (PostgreSQL)**
   - Conversation history is stored to maintain context across turns.

5. **LLM Response Generation**
   - The retrieved document chunks are injected into the prompt.
   - The AI model generates an answer strictly based on the retrieved content.

This ensures responses are **context-aware**, **document-grounded**, and
**hallucination-resistant**.

---

## Technologies Used

- **n8n** – Workflow orchestration and automation
- **Supabase** – PostgreSQL database with vector search
- **Google Drive** – PDF document storage
- **Google Gemini** – Embedding and chat model
- **RAG (Retrieval-Augmented Generation)** architecture

---

## Repository Contents

| File | Description |
|----|----|
| `RAG.json` | Exported n8n workflow containing both ingestion and RAG pipelines |
| `README.md` | Project documentation |

---

## How to Use This Project

1. Import `RAG.json` into your n8n instance
2. Configure credentials:
   - Google Drive
   - Supabase
   - Google Gemini (or compatible LLM)
3. Upload a PDF document to Google Drive
4. Execute the ingestion workflow to index the document
5. Open the chat interface
6. Ask natural language questions about the document

---

## Example Use Cases

- Company policy Q&A systems
- Internal knowledge assistants
- Document-aware chatbots
- RAG experimentation with low-code tools
- AI agents with persistent memory and retrieval

---

## Notes

- The PDF documents used in this project represent **fictional company data**
  created specifically for RAG testing.
- The system is designed to be modular and easily extendable
  to additional data sources or LLM providers.

---

## Author
Built and configured by **Aslınur**  
Focus: RAG systems, workflow automation, and applied AI architectures
