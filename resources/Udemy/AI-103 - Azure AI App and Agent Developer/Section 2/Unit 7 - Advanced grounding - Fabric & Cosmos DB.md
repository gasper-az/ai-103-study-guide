# Advanced grounding - Fabric & Cosmos DB

Source: [Unit 7 - Advanced grounding - Fabric & Cosmos DB][src-udemy]

## What is MSFT Fabric?

It is a unified data platform that brings together data lakes, warehouses, and analytics into a single product called OneLake.

- **Fabric**: Fabric replaces separate Azure services (Data Lake, Data Warehouse, Synapse) with one integrated platform for all enterprise data.
- **OneLake**: it is the single storage layer in Fabric. Every piece of data in the company lives in OneLake, not scattered across multiple systems.
- **Fabric vs. Traditional storage**: traditional storage copies data between systems, while Fabric stores data once in OneLake, and all tools access the same copy.

## How agents queryu OneLake?

- **Onelake connection**: we create an Azure AI Search index that points to Onelake tables. The index does not copy data; it reads from Fabric live.
- **Query Flow**: Agent calls Search tool -> Search idx. translate query to Fabric SQL -> Fabric returns results -> Search idx. returns results to agent.
- **No data movement**: because the search idx. reads from Onelake live, the grounding data is always current. No sync jobs or data copies needed.

## Cosmos DB vs Azure AI search for grounding

- **Cosmos DB Strengths**: real-time updates (milliseconds latency), transactional consistency, and vector search plus SQL queries in one database.
- **Azure AI Search strengths**: advanced relevance tuning (learning to rank), hybrid search (vector + keywords), and larger document sizes (up to 16MB per document).
- **Decision rule**: if your data changes more than 10 times per day or requires transactions, use Cosmos DB. Otherwise, use Azure AI search.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569041#overview
