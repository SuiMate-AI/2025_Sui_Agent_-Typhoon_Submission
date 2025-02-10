# Place we used Atoma

## RAG

We implement a RAG (Retrieval-Augmented Generation) Mechanism with streaming pipeline to generate multi-level reasoning.

1. RAGManager Initialization

We utilize Atoma's API to function as a Large Language Model (LLM) engine.

- [RAGManager.openai](https://github.com/SuiMate-AI/eliza_aideniyi/blob/aideniyi/src/clients/twitter/ragManager.ts#L10C19-L10C31)

2. Knowledge Points Generation

```typescript
   streamCallback("<knowledge>");
   let knowledgePoints = await this.streamKnowledgePoints(text, streamCallback);
   streamCallback("</knowledge>");
```

RAGManager leverages Atoma's `meta-llama/Llama-3.3-70B-Instruct` model to analyze the question, extract relevant Sui blockchain concepts, and stream a response highlighting the key knowledge points required to answer the question.

- [RAGManager.streamKnowledgePoints](https://github.com/SuiMate-AI/eliza_aideniyi/blob/aideniyi/src/clients/twitter/ragManager.ts#L64)

3. Knowledge Embedding and Search Results Retrieval

The process converts knowledge points into embeddings using the `intfloat/multilingual-e5-large-instruct` model by Atoma, performs a semantic search in the Qdrant vector database, retrieves the top N most relevant documents, and streams the search results— including URLs, titles, and scores—wrapped in <searchResults> tags.

```typescript
    const searchResults = await this.getSearchResults(knowledgePoints);
    streamCallback(
      "<searchResults>" +
        JSON.stringify(
          searchResults.map((result) => ({
            url: result.url,
            title: result.title,
            score: result.score,
          }))
        ) +
        "</searchResults>"
    );
```

- [RAGManager.getEmbedding](https://github.com/SuiMate-AI/eliza_aideniyi/blob/aideniyi/src/clients/twitter/ragManager.ts#L17)
- [RAGManager.getSearchResults](https://github.com/SuiMate-AI/eliza_aideniyi/blob/aideniyi/src/clients/twitter/ragManager.ts#L92)


4. Final Refined Response Generation


RAGManager utilizes the `deepseek-ai/DeepSeek-R1` model provided by Atoma to produce the final response, refining the original question based on the retrieved search results and formatting the output as a Twitter-friendly message following specific formatting rules.

```typescript
    await this.streamFinalResponse(text, searchResults, streamCallback);
```

- [RAGManager.streamFinalResponse](https://github.com/SuiMate-AI/eliza_aideniyi/blob/aideniyi/src/clients/twitter/ragManager.ts#L128)
