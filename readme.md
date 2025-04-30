# RAG based Mental-Health Chatbot 

A mental health Q&A chatbot that uses LangChain, HuggingFace, FAISS, and Mistral LLMs. It reads content from a custom PDF — Emotional Aid — and answers user questions based strictly on that information.

## Project Purpose

This chatbot is designed to help users understand and reflect on mental health concepts by retrieving relevant insights from a structured mental health resource. It does not make up information and only responds with content available in the Emotional Aid PDF.

## System Architecture

```
                        ┌──────────────────────────┐
                        │     Emotional Aid PDF    │
                        └────────────┬─────────────┘
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │  Load & Split into Chunks│
                        │  (LangChain Splitter)    │
                        └────────────┬─────────────┘
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │   Embed with MiniLM L6    │
                        │(HuggingFace Embeddings)   │
                        └────────────┬─────────────┘
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │     Store in FAISS DB    │
                        └────────────┬─────────────┘
                                     │
        ┌────────────────────────────┼────────────────────────────┐
        ▼                            ▼                            ▼
  User Input               Retrieve Top k Relevant Chunks   Prompt Construction
(Streamlit UI)             (LangChain Retriever)            (Custom Prompt Template)
        │                            │                            │
        └─────────────┬──────────────┴──────────────┬────────────┘
                      ▼                             ▼
                ┌────────────────────────────────────────┐
                │      Mistral LLM via HuggingFace       │
                └────────────────────────────────────────┘
                                     │
                                     ▼
                           Return Answer to UI
```

## Features

- PDF-to-chatbot pipeline using LangChain
- Answers only from context, no hallucinations
- Embeddings generated via HuggingFace transformers
- FAISS-powered retrieval for fast similarity search
- Query handling using Mistral-7B-Instruct
- Interactive chat interface using Streamlit

## Tools and Technologies

| Tool / Library | Role |
|----------------|------|
| LangChain | Chaining PDF ingestion, chunking, retrieval, and QA logic |
| HuggingFace | Embedding model and Mistral LLM via endpoint |
| Mistral | LLM for generating answers with context awareness |
| FAISS | Vector similarity search and document retrieval |
| Streamlit | Web interface for chatbot interaction |
| Python | Core programming language |
| VS Code | Development environment |

## How It Works

### PDF Loading & Preprocessing
Loads the Emotional Aid PDF and breaks it into manageable text chunks with overlap using RecursiveCharacterTextSplitter.

### Embeddings & Vector Store
Each chunk is embedded using sentence-transformers/all-MiniLM-L6-v2, and stored in a FAISS index for later retrieval.

### Retrieval-Augmented QA
On user prompt, the top relevant chunks are fetched using FAISS and sent to the Mistral LLM, which generates a focused answer using the given context only.

### User Interface
The user interacts via a Streamlit chat interface which shows both questions and AI-generated responses.


## Setup Instructions

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Add HuggingFace Token

Create a .env file and include:

```env
HF_TOKEN=your_huggingface_token_here
```


### Run the Streamlit App

```bash
streamlit run app.py
```

## Ways to Improve Accuracy

- Upgrading from Mistral-7B to a more powerful model (like LLaMA-2, Claude, or GPT variants) can yield better reasoning and understanding.
- Fine-tuning a language model on mental health–related conversations or medical QA data can improve contextual accuracy.
- You can edit the chain type from stuff: Use conversational memory to handle follow-up questions and maintain dialogue continuity.
- Apply response filtering, human-in-the-loop evaluations, or confidence thresholds to reject hallucinated answers.

  
Developed by Manasvi :)
