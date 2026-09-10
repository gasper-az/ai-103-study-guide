# Agentic RAG - Dynamic retrieval for agents

Source: [Unit 6 - Agentic RAG - Dynamic retrieval for agents][src-udemy]

## What is grounding?

Grounding means giving an agent factual information from your own data, so it answers correctly, and not from its training data.

## Static RAG vs Agentic RAG

- **Static RAG**: you search a DB for every user question and inject results into the prompt. The agent never decides to search or skip.
- **Agentic RAG**: you give the agent a search tool. The agent decides if it should search, and if so, what to search.
- **Why agentic is better?**: it searches only when needed. Static RAG waste tokens when the answer is obvious.

## Azure AI Search - Vector search for similarity

- **Vector search**: it converts text into numbers (vectors). Documents with similar vectors have similar meanings.
- **Hybrid search**: Azure AI search supports keyword search (exact words) plus vector search (meaning).

## Embeddings

- **Embedding model**: an embedding model takes text input and outputs a vector of numbers, typically 1536 numbers long.
- **How embeddings enable search?**: your search index stores embedding vectors for every document. The user's question is also converted to an embedding. The search finds documents with the most similar vectors.
- **Embedding distance**: two texts with similar meaning have embedding vectors that are close together mathematically. Unrelated texts have vectors for apart.

## Coding pattern - Complete grounding flow

1. **Embedding**: call embedding model API to convert user question to vector. Store in `user_embedding` variable.
1. **Search**: call Azure AI search with `vectorQueries` containing `user_embedding`. Parse JSON response into `grounding_text` arrays.
1. **Construct prompt**: Build `grounded_prompt = f"Context:\n{grounding_text}\n\nQuestion:\n{user_question}"`.
1. **LLM call**: send `grounded_prompt` to deployed model endpoint. Return reponse to the user.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569039#overview
