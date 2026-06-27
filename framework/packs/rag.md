# RAG Pack

Activates the RAG Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions involving Retrieval-Augmented Generation systems: the combination of a vector search or retrieval layer with a language model to produce grounded responses.

## Activate When

The decision involves designing or modifying a RAG pipeline, selecting a vector database, chunking and embedding strategy, retrieval ranking approach, or the integration between a retrieval system and an LLM.

## Added Concerns

**Architectural concerns:**
- Is the retrieval layer and the generation layer independently testable and replaceable? A design that couples the embedding model to the generation model makes it difficult to evaluate or swap components.
- Is the document ingestion pipeline (chunking, embedding, indexing) designed to be re-runnable without data loss or duplication? Ingestion pipelines need idempotency when documents are updated or re-indexed.
- Is the retrieved context window sized appropriately for the generation model? Too little context produces incomplete answers; too much context increases cost and can degrade coherence.

**Quality concerns:**
- Is retrieval precision evaluated independently from generation quality? A RAG system can fail at retrieval (retrieving irrelevant documents) or at generation (ignoring relevant retrieved content). These failures require different mitigations.
- Is the chunking strategy aligned with the semantic units of the source documents? Chunk boundaries that split a logical unit (a paragraph, a section) across two chunks degrade retrieval relevance.
- Is there a citation or grounding mechanism that allows users or auditors to trace a generated claim to its source document?

**Security concerns:**
- Are the documents indexed in the vector store authorized for the querying user? Retrieval must respect access control; a vector search that returns documents the querying user is not allowed to read is an authorization bypass.
- Is the ingestion pipeline protected against document injection? A malicious document designed to bias retrieval results toward attacker-controlled content is a prompt injection variant.

**Observability concerns:**
- Are retrieval recall, precision, and latency measured independently from end-to-end response quality?
- Are queries that produce low-confidence or no-retrieval results logged for review?

## Council Interactions

The RAG Specialist defers to the Security Architect on access control in retrieval and prompt injection risk, to the AI Specialist on generation model behavior and output quality, and to the Performance Engineer on indexing throughput and query latency.
