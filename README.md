# AI Research Assistant Agent

An agentic AI system that automates research paper understanding — it reads documents, retrieves relevant context, and answers questions or generates structured reports, without manual reading.

Built with **n8n** (workflow automation + decision logic) and **Google Colab** (RAG pipeline + LLM processing with Gemini).

## Problem

Reading and analyzing long research papers (e.g. cybersecurity literature) is slow and manual. This project automates that process: given a paper, the system extracts structure, summarizes it, and can answer follow-up questions grounded in the document content.

## Architecture

**n8n automation layer**
- Workflow trigger → Google Drive download → HTTP request → Set/Code node
- Basic LLM Chain (Gemini) decides whether the input is a paper to summarize or a general Q&A query
- `If` node routes to the correct path based on that decision

**Google Colab / RAG layer**
- Documents are loaded and split into chunks (LangChain `RecursiveCharacterTextSplitter`)
- Chunks are embedded (`sentence-transformers/all-MiniLM-L6-v2`) and stored in a Chroma vector DB
- Relevant chunks are retrieved per query and passed to Gemini for the final answer

## Tech Stack

- **n8n** – workflow orchestration, conditional logic
- **Google Gemini** – LLM for summarization and Q&A
- **LangChain** – document loading, text splitting, retrieval
- **ChromaDB** – vector storage for RAG
- **HuggingFace Sentence Transformers** – embeddings
- **Google Colab** – execution environment

## How It Works

1. User uploads a document or asks a question
2. n8n receives the request and routes it through the workflow
3. If a paper is provided → the system generates a structured report (objective, methodology, findings, limitations, summary)
4. If no paper is provided → the system answers as a general Q&A agent
5. Colab backend handles document processing, embedding, retrieval, and final answer generation via Gemini

## Dataset

4–5 cybersecurity research papers (PDF), used only as a knowledge source for testing retrieval and summarization — not redistributed in this repo.

## Results

- Correctly parsed and understood research paper structure
- Retrieved relevant context via RAG before answering
- Automated end-to-end via n8n with no manual intervention
- Generated accurate, structured summaries and Q&A responses

## Limitations

- Requires an internet connection and active Gemini API access
- Output quality depends on document quality (scanned/poorly formatted PDFs perform worse)
- Free-tier API and Colab usage limits apply

## Files

- `AI_Research_Assistant_Agent.json` – n8n workflow export (import directly into n8n)
- `stage_5_project_new.ipynb` – Colab notebook (RAG pipeline: loading, chunking, embedding, retrieval)
- `AI_Agentic_Final_Project.pptx` – project write-up and architecture slides

## Setup

1. Import `AI_Research_Assistant_Agent.json` into your n8n instance
2. Replace `<YOUR_GOOGLE_DRIVE_FILE_LINK>` placeholders with your own document source, and connect your own Google Drive + Gemini credentials
3. Open `stage_5_project_new.ipynb` in Google Colab
4. Set your `GOOGLE_API_KEY` in Colab Secrets (Gemini API key)
5. Run the notebook cells in order to build the vector DB and test retrieval

## Author

Uzair — built as part of an Agentic AI final project.

