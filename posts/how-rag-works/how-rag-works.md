---
title: How RAG (Retrieval-Augmented Generation) Works
description: An overview of the process flow of Retrieval-Augmented Generation.
slug: how-rag-works
published: '2026-10-1'
category: AI
---

<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
</script>

<Image src="how-rag-works.webp" alt="RAG process flow diagram" />

RAG connects an LLM to an external knowledge base so it can answer using information beyond what it was trained on. There are two phases: an **indexing phase** (done ahead of time) and a **query phase** (happens at runtime, per user query).

## 1. Indexing Phase

### Documents → Chunked → Chunks
Source documents (PDFs, docs, web pages, etc.) get split into smaller chunks — typically a few hundred tokens each. Chunking matters because embedding a whole document loses granularity; you want retrieval to return focused, relevant passages, not entire files.

### Chunks → Embedding Model → Embeddings → Vector Store
Each chunk is passed through an embedding model, which converts the text into a numeric vector that captures its semantic meaning. Those vectors (plus a reference to the original text) are stored in a vector store/database, indexed for fast similarity search.

## 2. Query Phase

### User Query → Embedding Model
When a user asks a question, that query is also run through the same embedding model, turning it into a vector in the same space as the stored chunks.

### Query Embedding → Vector Store → Retrieved Chunks
The vector store compares the query vector against stored chunk vectors (using cosine similarity or similar) and returns the top-k most relevant chunks.

### Retrieved Chunks + Query → Augmented Prompt
The retrieved chunks are inserted into the prompt alongside the user's original query. This combined prompt (query + retrieved context) is what gets sent to the LLM.

### LLM Generates Response
The LLM reads the augmented prompt and generates an answer grounded in the retrieved content, rather than relying solely on its training data.

## Flow Diagram

<Image src="how-rag-works-flow.webp" alt="Another RAG process flow diagram" />

## Key Insight

The embedding model is used **twice** — once to index the documents, once to embed the incoming query — and it's the **vector store's retrieval result** (not the embedding model itself) that augments what the LLM sees.

## Next Steps

Topics worth exploring further:
- Chunking strategies (fixed-size, semantic, recursive)
- Embedding model choice (OpenAI, Cohere, open-source options)
- Retrieval methods (hybrid search, re-ranking, metadata filtering)