# Build knowledge-enhanced AI agents with Foundry IQ

Source: [Build knowledge-enhanced AI agents with Foundry IQ][msft-docs-source]

## Understanding RAG for agents

**Simple AI agent limitations**:

Simple AI agents face significant challenges in enterprise environments. These limitations prevent them from providing the accurate, contextual responses that organizations need for critical business operations:

| Limitation | Impact | Example |
| --- | --- | --- |
| Knowledge cutoff dates | No access to recent information | Can't help with newly released features or updated policies |
| Private data access | Generic responses only | Missing company procedures, support knowledge, product specs |
| Lack of context | Irrelevant advice | Ignores specific security requirements or approval workflows |
| Fabricated responses | Compliance and security risks | Confident-sounding but incorrect information |
| Scalability issues | Duplicated engineering effort | Every team rebuilds the same RAG infrastructure |

**How RAG solves these problems**:

The RAG process works in three coordinated steps:

1. Retrieve: System searches knowledge bases for relevant content related to the query.
1. Augment: Combines retrieved content with the user's question to provide factual context.
1. Generate: Agent creates response using both training data and retrieved information.

Through this process, RAG delivers three critical advantages for enterprise AI:

1. Real-time updates that keep agents current with policies and procedures without requiring retraining.
1. Source transparency that shows users exactly which documents informed each response to build trust and enable verification.
1. Factual grounding that anchors responses in actual organizational content to eliminate fabricated information and ensure compliance.

## Explore Foundry IQ

**What is Foundry IQ?**:

Foundry IQ is a managed knowledge platform for AI agents built on Azure AI Search. It provides the retrieval capabilities you learned about in RAG, but as a shared service that multiple agents can use.

**How knowledge bases organize information**:

Knowledge bases in Foundry IQ organize information by business domain rather than technical storage location. This design reflects how people actually think about information.

Instead of agents searching "SharePoint Site A" or "Blob Container B," they search "Product Documentation" or "HR Policies." Each knowledge base brings together related information regardless of where it's stored.

**Connecting data sources**:

Foundry IQ connects to your existing storage through data source integrations. You point it at your SharePoint sites, Blob containers, or OneLake instances. Foundry IQ handles indexing, embedding generation, and search optimization automatically.

Here's what happens when you add a data source:

1. Discovery: Foundry IQ scans your storage location for documents.
1. Processing: Documents are chunked and embedded for semantic search.
1. Indexing: Content becomes searchable through the knowledge base.
1. Monitoring: Changes to your documents trigger automatic reindexing.

**Code example - Connecting agents to knowledge**:

```python
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import PromptAgentDefinition, MCPTool

project_client = AIProjectClient(endpoint=project_endpoint, credential=credential)

# Connect to the product documentation knowledge base
knowledge_tool = MCPTool(
    server_label="product-docs",
    server_url=f"{search_endpoint}/knowledgebases/product-documentation/mcp"
)

# Create an agent with knowledge access
agent = project_client.agents.create_version(
    agent_name="product-support-agent",
    definition=PromptAgentDefinition(
        model="gpt-4o-mini",
        instructions="Answer product questions using the knowledge base. Always cite your sources.",
        tools=[knowledge_tool]
    )
)
```

## Configure data sources for knowledge bases

Understanding which data source to use depends on where your data lives and how you need to access it. Foundry IQ supports six primary data source types:

| Data Source | Access Type | Best For |
| --- | --- | --- |
| Azure AI Search Index | Indexed | Enterprise search with custom pipelines |
| Azure Blob Storage | Direct | Document files in Azure Storage |
| Web | Real-time | Current, public information via Bing |
| SharePoint (Remote) | Real-time | Live SharePoint content with Microsoft 365 governance |
| SharePoint (Indexed) | Indexed | Advanced search on SharePoint with custom pipelines |
| OneLake | Direct | Unstructured data in Microsoft Fabric |

## Configure retrieval with Foundry IQ

**Writing effective retrieval instructions**:

Effective instructions specify three critical behaviors:

1. **When to retrieve**: Tell the agent to always use the knowledge base, never rely on training data.
1. **How to cite**: Specify the exact format for source attribution.
1. **What to do when unsure**: Define fallback behavior when information isn't found.

```python
retrieval_instructions = """You are a helpful HR assistant.

CRITICAL RULES:
- You must ALWAYS search the knowledge base before answering any question
- You must NEVER answer from your own knowledge or training data
- Every answer must include citations in this format: 【doc_id:search_id†source_name】
- If the knowledge base doesn't contain the answer, respond with "I don't have that information in our current documentation. Please contact HR directly at hr@company.com"

Your role is to provide accurate, verifiable information from company documentation."""

agent = project_client.agents.create_version(
    agent_name="hr-assistant",
    definition=PromptAgentDefinition(
        model="gpt-4o-mini",
        instructions=retrieval_instructions,
        tools=[knowledge_tool]
    )
)
```

[msft-docs-source]: https://learn.microsoft.com/en-us/training/modules/introduction-foundry-iq/
