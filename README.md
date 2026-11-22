# Knowledge Graph RAG — AWS Bedrock, Chroma & Neptune Concepts

A notebook-based exploration of **Retrieval-Augmented Generation (RAG), knowledge graphs, and multi-hop retrieval**. The notebook compares a local vector-retrieval baseline with an Amazon Bedrock Knowledge Base retrieval flow and explains how graph relationships can support GraphRAG-style questions.

## What the notebook covers

1. Load the sample PDF collection from `data/`.
2. Split documents into overlapping text chunks.
3. Generate embeddings with `BAAI/bge-small-en-v1.5`.
4. Build a local Chroma vector store and retrieve the top five similar chunks.
5. Send retrieved context to Amazon Nova Lite through the Bedrock Runtime API.
6. Explore Amazon Bedrock Knowledge Bases, Amazon Neptune, Neptune Analytics, entities, relationships, and multi-hop retrieval concepts.
7. Use the Bedrock Agent Runtime `retrieve` API to obtain Knowledge Base results.

The saved notebook output shows a sample run that loaded **10 PDFs (133 pages)** and created **250 chunks**.

## Architecture in this repository

- **Local RAG baseline:** LangChain document loaders and splitters, Hugging Face embeddings, Chroma
- **Answer generation:** Amazon Bedrock Runtime with the `amazon.nova-lite-v1:0` model
- **Managed retrieval:** Amazon Bedrock Agent Runtime Knowledge Base retrieval
- **Graph concepts:** Amazon Neptune and Neptune Analytics are covered in the architecture walkthrough

**Implementation boundary:** the notebook demonstrates Chroma retrieval and calls the Bedrock Knowledge Base retrieval API. It discusses Neptune and GraphRAG architecture, but the current notebook does not contain explicit code to provision a Neptune graph or issue Gremlin/openCypher traversals. There are no formal latency or retrieval-quality benchmark results in this repository yet.

## Repository layout

- `Knowledge Graph RAG.ipynb` — end-to-end learning notebook
- `data/` — sample PDF documents used by the notebook
- `images/` — workflow and architecture illustrations
- `chroma_db/` — local Chroma persistence created by the notebook
- `notes.excalidraw` — architecture notes

## Requirements

Python 3.10+ and Jupyter are recommended.

```bash
python -m venv .venv

# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate

pip install langchain langchain-community langchain-huggingface langchain-core \
  langchain-text-splitters langchain-chroma chromadb pypdf \
  sentence-transformers boto3 jupyter
jupyter lab
```

The notebook uses `uv pip install` in its setup cells; the equivalent package list above can be installed with pip.

## AWS setup

Configure AWS credentials outside the notebook (for example, with an IAM role or the AWS CLI). The notebook uses the `us-east-1` region and calls Bedrock Runtime and Bedrock Agent Runtime. Ensure your identity has the required Bedrock permissions and access to the selected model and Knowledge Base.

Replace the Knowledge Base identifier in the notebook with the identifier from your own AWS account. Never commit AWS access keys, secret keys, session tokens, or other credentials.

## Data and privacy

Before sharing or deploying this notebook, verify that every PDF in `data/` is synthetic or otherwise approved for publication. Do not use confidential employee, customer, HR, or internal business records in a public repository.

## Current status

This repository is an educational GraphRAG/RAG exploration. Treat it as a prototype notebook, not a production GraphRAG service or a validated performance comparison.
