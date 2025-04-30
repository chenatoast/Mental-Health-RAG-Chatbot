# Emotional Aid Chatbot

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

## File Structure

```
project-root/
│
├── data/                    # Contains Emotional Aid PDF
├── vectorstore/             # FAISS vector store files
├── app.py                   # Streamlit UI and app logic
├── load_pdf.py              # PDF loading and chunking
├── vector_store_builder.py  # Embedding and FAISS creation
├── query_runner.py          # LLM, prompt, and retrieval logic
├── .env                     # Contains HuggingFace API key
└── README.md                # Project documentation
```

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

### Run Vector Store Builder (One-Time)

```bash
python vector_store_builder.py
```

### Run the Streamlit App

```bash
streamlit run app.py
```

## Example Query

```
Question: What are the recommended coping techniques for anxiety?
```
Answer will be extracted directly from the Emotional Aid PDF, and not invented.

## Limitations

- The chatbot only answers based on the content of the provided PDF.
- It does not substitute professional mental health guidance.
- Accuracy depends on clarity and completeness of the source document.

## Future Improvements

- Support for multiple PDFs and topics
- Better memory and summarization over longer conversations
- Sentiment-aware responses for user emotional support