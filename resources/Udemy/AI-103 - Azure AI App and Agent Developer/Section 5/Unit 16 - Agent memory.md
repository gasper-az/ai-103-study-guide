# Agent memory

Source: [Unit 16 - Agent memory - Short-term and Long-term state][src-udemy]

## Short-term memory - Session context

- **Short-term**: the agent remembers what was said 5 messages ago within the same conversation. Information is stored in memory, not re-sent with each prompt.
- **Session expiration**: A session typically expires after 1 hour of inactivity or when the user explicitly ends the conversation. Memory is then cleared.
- **Storage location**: short-term memory lives in the agent's runtime memory (RAM) or in a fast cache like Redis. Not written to long-term database.

## Long-term memory - User preferences across sessions

- **Long-term**: The agent remembers user's preferences, no matter when was the last conversation with them.
- **Persistence layer**: It is written to a database (Cosmos DB) or key-value store (redis with persistence). Data survives agent restarts.
- **User identification**: Long-term memory requires a stable user ID (from Entra ID or session cookie) to retrieve the correct user's data on each conversation.

## Sliding window - Managing token limits

A sliding window keeps only the most recent `N` messages in short-term memory, discarding older messages when the conversation becomes too long.

- **Sliding window**: set maximum messages to 20. When message 21 arrives, discard 1 message. Window always contains most recent 20 messages.
- **Token-based window**: Instead of message count, track token count. Keep adding messages until token limit, then remove oldest message.
- **What is lost**: When you discard old messages, the agent loses that context. The user may need to repeat information. Inform user: "restarting conversation to save memory".

## Summarization as Short-Term memory compression

Instead of discarding old messages, you can summarize them into a shorter form, preserving key information while reducing token count.

- **Summarization flow**: after every 10 messages, call an LLM with prompt : "Summarize this conversation so far, keeping key facts and user intent".
- **Replace with summary**: replace the original 10 messages with the 200-token summary. Continue appending new messages. Total token count stays within limits.
- **Lossy compression**: summaries lose some details. The agent may forget minor points but retains major facts and user preferences.

## Azure Cosmos DB for Long-term memory

- **Why Cosmos DB?**: Cosmos DB stores JSON directly. Agent memory (user preferences, conversation history) is already JSON. No conversion needed.
- **Partition key for users**: use `user_id` as partition key. All memories for a user are stored together, enabling fast retrieval.
- **Time-to-live (TTL)**: set TTL on memory documents. Cosmos DB automatically deletes expired memories. E.g.: "user preferences expire after 90 days of no interaction".

## SDK pattern - Storing memory in Cosmos DB

- **SDK pattern**: `from azure.cosmos import CosmosClient`. Also use `container.upsert_item({"id": conversation_id, "user_id": user_id, "memory": memory_json, "ttl": 7776000 })`.
- **Reading memory**: Use `container.read_item(item=conversation_id, partition_key=user_id)`. Handle `CosmosResourceNotFoundError` for new users.

## Azure Managed Redis for Short-term memory

- **Why Redis for Short-term**: Redis stores data in RAM, not disk. Retrieval is much faster than Cosmos DB (1ms vs 50ms). Perfect for session memory.
- **Expiration Built-in**: Set redis key to expire after 1 hour: `SETEX session:user123`.
- **Redis vs CosmosDB**: Redis for short-term (session memory, conversation history), and Cosmos DB for long-term (user preferences, cross-session data).

## Memory structure: What to store?

- **Required fields**: `conversation_id`, `user_id`, `messages` (list of recent messages), `preferences` (dictionary, e.g. `{"temperature_unit": "C"}`).
- **Sessions State Fields**: `state` (e.g.: "awaiting_order_number", "approval_pending"), `pending_action` (tool call awaiting result), `last_activity_timestamp`.
- **Size limits**: Keep memory under 10k tokens for CosmosDB (2MB document limit) and under 1MB for Redis to maintain performace.

## Serializing and Deserializing Memory JSON

- **Serialization**: `memory_json = json.dumps({"user_id": "123", "preferences": {"unit": "C"}, "messages": message_list })`.
- **Deserialization**: `memory = json.loads(memory_json)`. Access fields: `user_id = memory["user_id"]`.
- **Handling missing fields**: Use `memory.get("preferences", {})` to provide a default empty dictionary if field does not exist in older stored memories.

## Authentication

- **Managed identity for Cosmos DB**: `Cosmos DB Built-in Data Contributor` role.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57635837#overview
