# AI/ML Engineer Interview Guide: Deep Dive

> A comprehensive guide to mastering AI/ML system design interviews from a Head of AI/ML perspective. Covers Cost & Latency, System Design, Real-World Scenarios, LLM Fundamentals, Prompting, and Vector DBs.

---

## Table of Contents

- [1. Cost & Latency Tradeoffs](#1-cost--latency-tradeoffs)
  - [1.1 How do you reduce token usage in production?](#11-how-do-you-reduce-token-usage-in-production)
  - [1.2 When should you quantize models and what are the tradeoffs?](#12-when-should-you-quantize-models-and-what-are-the-tradeoffs)
  - [1.3 Caching Strategies for LLM Applications](#13-caching-strategies-for-llm-applications)
- [2. System Design Thinking](#2-system-design-thinking)
  - [2.1 Design a production RAG system](#21-design-a-production-rag-system)
  - [2.2 Design a multi-tenant LLM platform](#22-design-a-multi-tenant-llm-platform)
- [3. Real-World Scenarios](#3-real-world-scenarios)
  - [3.1 How do you handle LLM hallucinations in production?](#31-how-do-you-handle-llm-hallucinations-in-production)
  - [3.2 Handling model failures and fallbacks](#32-handling-model-failures-and-fallbacks)
- [4. LLM Fundamentals](#4-llm-fundamentals)
  - [4.1 How does tokenization affect LLM performance and cost?](#41-how-does-tokenization-affect-llm-performance-and-cost)
  - [4.2 Explain attention mechanisms and their computational complexity](#42-explain-attention-mechanisms-and-their-computational-complexity)
- [5. Prompting & Context Engineering](#5-prompting--context-engineering)
  - [5.1 How do you design effective system prompts?](#51-how-do-you-design-effective-system-prompts)
  - [5.2 Context window management strategies](#52-context-window-management-strategies)
- [6. Vector Databases & RAG](#6-vector-databases--rag)
  - [6.1 How do you choose a vector database?](#61-how-do-you-choose-a-vector-database)
  - [6.2 Chunking strategies for RAG](#62-chunking-strategies-for-rag)

---

## 1. Cost & Latency Tradeoffs

### 1.1 How do you reduce token usage in production?

#### Deep Understanding
Token usage directly impacts cost (input + output tokens billed) and latency (more tokens = longer generation time). The optimization happens at multiple levels:
```
┌─────────────────────────────────────────────────────────────────┐
│                    TOKEN OPTIMIZATION LAYERS                    │
├─────────────────────────────────────────────────────────────────┤
│  Layer 1: Input Optimization (Pre-processing)                   │
│  ├── Prompt compression (summarize context before passing)      │
│  ├── Dynamic context selection (retrieve only what's needed)    │
│  ├── System prompt minimization (remove redundant instructions) │
│  └── Conversation pruning (summarize old turns)                 │
├─────────────────────────────────────────────────────────────────┤
│  Layer 2: Model-Level Optimization                              │
│  ├── Smaller models for simple tasks (routing)                  │
│  ├── Fine-tuned models (more efficient than few-shot)           │
│  └── Specialized models vs general-purpose                      │
├─────────────────────────────────────────────────────────────────┤
│  Layer 3: Output Optimization (Post-processing)                 │
│  ├── Structured outputs (JSON mode reduces verbosity)           │
│  ├── Max tokens limitation                                      │
│  ├── Early stopping / stop sequences                            │
│  └── Output compression for downstream tasks                    │
└─────────────────────────────────────────────────────────────────┘

```
#### Practical Example

```python
# BAD: Naive approach - sending everything
def naive_qa(question, full_document, chat_history):
    prompt = f"""
    You are a helpful assistant. Here is the full document:
    {full_document}  # Could be 50,000 tokens
    
    Here is our chat history:
    {format_history(chat_history)}  # Could be 10,000 tokens
    
    Question: {question}
    """
    return call_llm(prompt)

# GOOD: Optimized approach - multi-layer optimization
def optimized_qa(question, document_store, chat_history, summary_cache):
    # Layer 1: Summarize history instead of full history
    history_summary = summary_cache.get_or_compute(
        chat_history, 
        model="gpt-3.5-turbo"  # Cheaper model for summarization
    )
    
    # Layer 2: Retrieve only relevant chunks
    relevant_chunks = document_store.retrieve(
        question, 
        top_k=3,  # Instead of sending full document
        max_tokens=1500  # Limit retrieved content
    )
    
    # Layer 3: Minimal system prompt
    prompt = f"Answer based on context. Context: {relevant_chunks}\nQ: {question}"
    
    # Layer 4: Use JSON mode for structured output
    response = call_llm(
        prompt,
        response_format={"type": "json_object"},
        max_tokens=200  # Limit output length
    )
    
    return json.loads(response)
```

#### Interview Answer Structure
> "Token optimization is a multi-layered problem. First, I look at **input optimization** - using RAG to retrieve only relevant context instead of passing full documents, which typically saves 70-90% of tokens. Second, for chat applications, I implement **conversation summarization** where older turns are compressed using a cheaper model. Third, I use **model routing** - simple queries go to smaller models, complex reasoning to larger ones. For example, at my previous company, we implemented a classifier that routed 60% of queries to a fine-tuned Llama-7B, reducing costs by 4x with only 5% quality degradation on our edge cases."

---

### 1.2 When should you quantize models and what are the tradeoffs?

#### Deep Understanding
Quantization reduces model precision (FP32 → FP16 → INT8 → INT4) to decrease memory usage and increase inference speed, at the cost of potential quality degradation.

| Precision | Memory/Param | Memory Savings | Speedup | Quality Loss |
|-----------|--------------|----------------|---------|--------------|
| FP32 (Full) | 4 bytes | 1x (Baseline) | 1x | None |
| FP16 (Half) | 2 bytes | 2x | ~1x | ~0% |
| INT8 (8-bit) | 1 byte | 4x | 1.2-2x | 0.1-1% |
| INT4 (4-bit) | 0.5 byte | 8x | 1.5-3x | 0.5-3% |
| INT3 (Extreme) | 0.375 byte | ~10x | 2-4x | 2-10% |

#### When to Quantize?
*   ✅ **Yes:** Memory-constrained environments, high-throughput serving, edge deployment, cost-sensitive apps.
*   ❌ **No:** Precision-critical tasks (math, coding), research/benchmarking, very small models (already fit in memory).

#### Interview Answer Structure
> "Quantization decisions depend on three factors: **memory constraints**, **latency requirements**, and **quality tolerance**. For most production LLM deployments, I recommend starting with **INT8 GPTQ** - it gives 4x memory reduction with typically <1% quality degradation. For edge devices or when memory is extremely constrained, **INT4 AWQ** is my go-to because it's activation-aware and preserves instruction-following better than GPTQ. However, I'd avoid quantization for math-heavy tasks or code generation where precision matters more. At [Company], we quantized our 70B model to INT4 for our mobile app, going from 140GB to 35GB VRAM requirement, enabling single-GPU deployment."

---

### 1.3 Caching Strategies for LLM Applications

#### Deep Understanding
Caching operates at three primary levels in LLM systems:
1.  **Semantic Cache (Response-level):** Cache complete responses for semantically similar queries (Hit rate: 10-30% for support bots).
2.  **KV Cache (Token-level):** Cache intermediate computations in transformer layers. Critical for long context (Implemented via vLLM PagedAttention).
3.  **Prompt Cache (Prefix-level):** Cache embeddings for repeated system prompts/few-shot examples (Implemented via NVIDIA TensorRT-LLM).

#### Interview Answer Structure
> "Caching in LLM systems operates at three levels. **Semantic caching** stores complete responses for similar queries - we implemented this with GPTCache and saw a 25% hit rate for our customer support chatbot, saving $15K/month. **KV caching** stores intermediate transformer computations, which vLLM implements via PagedAttention - this is essential for any multi-turn application. **Prompt caching** reuses prefix embeddings for repeated system prompts. The key insight is that cache hit rate depends heavily on **query normalization** and **threshold tuning** - too strict and you miss hits, too loose and you return incorrect answers."

---

## 2. System Design Thinking

### 2.1 Design a production RAG system

#### Deep Understanding
A production RAG system is split into two distinct paths: **Ingestion** and **Querying**.

*   **Ingestion Pipeline:** Source Data → Scheduling/Error Handling → Processing (Cleaning, Chunking, Embedding) → Vector Store.
*   **Query Path:** User Query → Query Processing (Rewrite/Classify) → Hybrid Retrieval (Dense + Sparse) → Reranking (Cross-encoder) → Context Assembly → LLM Generation → Post-processing/Validation.

#### Key Production Components
*   **Chunking:** Use semantic chunking over fixed-size to maintain context coherence.
*   **Retrieval:** Use Hybrid Search (Dense + Sparse/BM25) combined with Reciprocal Rank Fusion (RRF).
*   **Reranking:** Use a cross-encoder to re-score the top 20 chunks down to the top 5. (Yields ~15% precision improvement).
*   **Query Classification:** Route factual vs. analytical vs. comparison queries to optimized retrieval strategies.

#### Interview Answer Structure
> "For a production RAG system, I focus on five key areas. **First, the ingestion pipeline** needs intelligent chunking - I prefer semantic chunking because it maintains context coherence. **Second, retrieval** should use hybrid search combining dense and sparse vectors with Reciprocal Rank Fusion. **Third, reranking** with a cross-encoder significantly improves precision. **Fourth, query classification** routes different query types to optimized strategies. **Fifth, observability** - tracking retrieval latency, context relevance scores, and generation quality. The biggest mistake I see is teams not investing in chunking strategy - poor chunks can't be fixed by better retrieval algorithms."

---

### 2.2 Design a multi-tenant LLM platform

#### Deep Understanding
Multi-tenant platforms require isolation at three levels:
1.  **Data Isolation:** Separate namespaces in vector stores (e.g., `tenant_{id}`), prefixed Redis cache keys.
2.  **Compute Isolation:** Shared vs. dedicated model pools based on tier (Free/Pro/Enterprise).
3.  **Configuration Isolation:** Per-tenant feature flags, rate limits, and allowed model lists.

#### Interview Answer Structure
> "Multi-tenant LLM platforms require isolation at three levels: **data isolation** (separate namespaces in vector stores, prefixed cache keys), **compute isolation** (shared vs dedicated model pools based on tier), and **configuration isolation** (per-tenant feature flags, rate limits, model access). For enterprise tenants, we provision dedicated infrastructure to guarantee performance and prevent noisy neighbor problems. For free/pro tiers, we use shared pools with careful load balancing. The key architectural decision is **where to draw the isolation boundary** - too coarse and you waste resources, too fine and you lose efficiency."

---

## 3. Real-World Scenarios

### 3.1 How do you handle LLM hallucinations in production?

#### Deep Understanding
Mitigation requires a multi-layer approach:
*   **Prevention:** High-quality retrieval, constrained decoding (JSON mode), low temperature, explicit uncertainty instructions in system prompts.
*   **Detection:** Claim extraction + verification against source material, self-consistency checking (multiple generations).
*   **Correction:** Retrieval-augmented verification, self-correction prompts, graceful fallbacks to "I don't know".
*   **Monitoring:** User feedback, automated evaluation on sampled responses.

#### Risk-Based Mitigation
*   **Medical/Legal:** 0% tolerance. Reject unverified claims, require human review.
*   **Financial:** <5% tolerance. Conservative correction, strong disclaimers.
*   **Customer Support:** <20% tolerance. Add uncertainty markers like "[I couldn't verify this]".

#### Interview Answer Structure
> "Hallucination mitigation is a multi-layer problem. **Prevention** starts with high-quality retrieval and constrained decoding. **Detection** involves extracting individual claims and verifying each against source material. **Correction** depends on risk tolerance - for medical/legal, we reject unverified claims entirely; for customer support, we add uncertainty markers. The key insight is that **not all hallucinations are equal** - a contradicted fact is much worse than an unsupported claim. The biggest win is improving retrieval quality - better context reduced hallucinations by 40% before we even added detection."

---

### 3.2 Handling model failures and fallbacks

#### Deep Understanding
Resilience requires a structured failover strategy:
1.  **Circuit Breaker:** Tracks failures per model. Opens after N consecutive failures, rejects requests, and periodically half-opens to test recovery.
2.  **Failover Tiers:**
    *   Primary: Best model (e.g., GPT-4)
    *   Secondary: Different vendor (e.g., Claude) to avoid correlated API outages.
    *   Tertiary: Self-hosted model (e.g., Llama) for when vendor APIs are entirely down.
3.  **Degraded Response:** Cached answers or rule-based responses when all LLMs fail.

#### Interview Answer Structure
> "For model resilience, I implement a three-tier failover strategy. **Primary** is our best model, **secondary** is a different vendor to avoid correlated failures, and **tertiary** is a self-hosted model. Each model has a **circuit breaker** that opens after 5 consecutive failures. The key insight is that **failures are correlated by vendor** - if OpenAI has an outage, all OpenAI models fail. So we ensure fallback models are from different providers. We also implement **graceful degradation** where users see 'limited functionality' rather than hard errors."

---

## 4. LLM Fundamentals

### 4.1 How does tokenization affect LLM performance and cost?

#### Deep Understanding
Tokenization impacts four major areas:
1.  **Cost:** Direct billing metric.
2.  **Latency:** Linear scaling with token count.
3.  **Context Window:** Inefficient tokenization = less actual content fits.
4.  **Quality:** Important concepts split across tokens lose semantic meaning (e.g., "UN" → ["U", "N"]).

*Language Efficiency Example:* English tokenizes at ~4.5 chars/token, while Chinese tokenizes at ~1.5 chars/token. This means Chinese users effectively get 3x less actual content in the same 128k context window.

#### Interview Answer Structure
> "Tokenization has four major impacts. **Cost** is direct. **Latency** scales linearly. **Context window utilization** is critical - Chinese text typically tokenizes at ~1.5 characters per token versus ~4.5 for English, meaning Chinese users get 3x less actual content in the same context window. **Quality** can degrade when important concepts get split across tokens. In practice, I've seen companies save 20-30% on costs simply by normalizing text before sending to the API - collapsing whitespace, using contractions, and removing redundant formatting."

---

### 4.2 Explain attention mechanisms and their computational complexity

#### Deep Understanding
*   **Standard Attention:** Computes pairwise interactions. Complexity is **O(n² · d)** where *n* is sequence length and *d* is dimension. The *n²* comes from the Q·Kᵀ matrix multiplication.
*   **Memory Complexity:** O(n²) for the attention matrix. A 128K context needs ~32GB just for the attention matrix in FP16.

#### Efficient Attention Variants
*   **FlashAttention:** Exact computation, but reduces memory to **O(n)** via clever tiling and recomputation. 2-4x faster. (Standard in all modern frameworks).
*   **Grouped Query Attention (GQA):** Used in Llama 2/3. Doesn't reduce compute, but reduces KV cache memory by sharing key/value heads across query heads.
*   **Sliding Window Attention:** Used in Mistral. Limits attention to a local window, achieving **O(n · w · d)** complexity.

#### Interview Answer Structure
> "Attention computes pairwise interactions between all positions, giving O(n²·d) complexity. The n² term comes from the Q·Kᵀ matrix multiplication. This quadratic scaling is the fundamental bottleneck for long contexts. Modern approaches address this differently: **FlashAttention** keeps exact computation but reduces memory to O(n) through clever tiling - it's now standard everywhere. **Grouped Query Attention** reduces KV cache memory. **Sliding window attention** limits attention to a local window. The key insight is that FlashAttention is a pure implementation optimization with zero quality loss, while architectural changes like sliding window involve quality-compute tradeoffs."

---

## 5. Prompting & Context Engineering

### 5.1 How do you design effective system prompts?

#### Deep Understanding
Effective system prompts have 5 components:
1.  **Role Assignment:** Activates relevant knowledge patterns ("You are a senior financial analyst...").
2.  **Task Specification:** Clear scope prevents scope creep.
3.  **Constraints:** Critical for safety ("Only use provided context" reduces hallucinations).
4.  **Output Format:** JSON mode makes downstream processing reliable.
5.  **Few-Shot Examples:** Most powerful tool to demonstrate exact expectations.

#### Interview Answer Structure
> "Effective system prompts have five components. **Role assignment** activates relevant knowledge patterns. **Task specification** with clear scope prevents going off-topic. **Constraints** are critical for safety - explicitly stating 'only use provided context' dramatically reduces hallucinations. **Output format** specification makes downstream processing reliable. **Examples** demonstrate exactly what you want. For production, I always version control prompts and A/B test changes. We once improved first-contact resolution by 12% just by adding explicit handling instructions for edge cases. The key insight is that prompts are code - they should be tested and versioned."

---

### 5.2 Context window management strategies

#### Deep Understanding
Context budget allocation (e.g., for a 128k window):
*   **System Prompt:** ~500 tokens (Fixed)
*   **Retrieved Context:** ~20k tokens (Dynamic)
*   **Chat History:** ~100k tokens (Managed)
*   **Response Space:** ~7.5k tokens (Reserved)

#### History Management Strategies
*   **Truncation:** Drop oldest turns (Simple, loses early context).
*   **Summarization:** Summarize old turns using a cheaper model, keep recent verbatim.
*   **Hybrid:** Summarize very old turns, keep recent 3-4 turns verbatim. (Recommended)

#### Interview Answer Structure
> "Context window management is about budget allocation. I divide the window into four zones: system prompt (fixed), retrieved context (dynamic), chat history (managed), and response space (reserved). For history management, I use a **hybrid strategy**: summarize the oldest turns with a cheaper model, keep the most recent 3-4 turns verbatim. The key insight is that **not all history is equal** - a user stating their name in turn 1 is more important than a generic acknowledgment in turn 15. We implemented this and reduced context-related quality drops by 40%."

---

## 6. Vector Databases & RAG

### 6.1 How do you choose a vector database?

#### Deep Understanding
Selection depends on three dimensions:
1.  **Deployment Model:** Managed (Pinecone, Zilliz Cloud) vs. Self-Hosted (Milvus, Qdrant) vs. Embedded (Chroma, LanceDB).
2.  **Scale:** Small (<1M), Medium (1M-100M), Large (>100M).
3.  **Features:** Hybrid search, filtering, multi-tenancy, built-in reranking.

#### Quick Selection Guide
*   **Prototyping:** Chroma or LanceDB (Embedded, zero infra).
*   **Medium Scale / Managed:** Qdrant Cloud (Best price/performance) or Pinecone (Best DX).
*   **Large Scale / Managed:** Pinecone Dedicated or Zilliz Cloud.
*   **Data Sovereignty / Self-Hosted:** Qdrant (Low resource req) or Milvus (Best for >100M vectors).

#### Interview Answer Structure
> "Vector database selection depends on **deployment model**, **scale**, and **features**. For prototyping, I'd use Chroma or LanceDB embedded. For production at medium scale with managed services, Qdrant Cloud offers the best price-performance, while Pinecone has the best developer experience. For large scale, Pinecone dedicated or self-hosted Milvus are the options. A mistake I see often is teams choosing Pinecone for 10K vectors - that's overkill. Start simple, migrate when you have evidence you need to."

---

### 6.2 Chunking strategies for RAG

#### Deep Understanding
*   **Fixed-Size:** Simple, predictable, but breaks semantic boundaries.
*   **Paragraph/Section:** Better coherence, but variable sizes (some too small/large).
*   **Semantic Chunking:** Uses embeddings to find natural topic boundaries. Best coherence, but computationally expensive.
*   **Structural/Hierarchical:** Parses document structure (headers, lists). Preserves hierarchy for retrieval.

#### Interview Answer Structure
> "Chunking strategy makes or breaks a RAG system. Fixed-size chunking is the most common mistake—it splits sentences and concepts in half. I strongly prefer **semantic chunking** or **structural chunking**. Structural chunking parses Markdown/HTML headers to keep related paragraphs together. If compute allows, semantic chunking uses embedding similarities to find natural topic boundaries. The goal is that any single chunk should be self-contained enough to answer a question without requiring adjacent chunks."

---

## License & Contribution
Feel free to use this guide for interview prep or internal team training. Pull requests with additional questions/deep dives are welcome!
```
