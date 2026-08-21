# Develop an AI agent with Microsoft Agent Framework

Source: [Develop an AI agent with Microsoft Agent Framework][msft-docs-source]

## Understand Microsoft Agent Framework AI agents

The Microsoft Agent Framework is the next generation of both Semantic Kernel and AutoGen, built by the same engineering teams. It combines AutoGen's intuitive agent abstractions with Semantic Kernel's enterprise-grade features—including session-based state management, type safety, execution filters, and telemetry.

**What agents can do**:

Because all agents share the same Agent base class, you get a consistent set of capabilities regardless of which provider powers your agent. This means you can focus on your application logic rather than adapting to provider-specific APIs.

Out of the box, every agent in the framework supports:

- *Function calling*: automatically invoke registered tools to interact with external APIs and services.
- *Multi-turn conversations*: maintain chat history either locally or via service-provided history management.
- *Structured outputs*: generate type-safe, schema-validated responses.
- *Streaming responses*: receive results incrementally as they're generated.
- *Service-provided tools*: use built-in capabilities such as code execution, file search, and web search where supported by the provider.

## Add tools to Azure AI agent

**Service-provided tools**:

When using the Foundry provider, a range of hosted tools are available without any extra implementation.

| Tool - What it does |
| --- | --- |
| Code Interpreter - Executes Python code in a sandboxed environment for calculations and data analysis |
| File Search - Searches through and retrieves information from uploaded documents |
| Web Search - Retrieves up-to-date information from the internet |
| Hosted MCP Tools - MCP (Model Context Protocol) servers invoked directly by the provider runtime |
| Azure AI Search - Queries an Azure AI Search index through a Foundry connection |
| Foundry Toolboxes - Named, versioned bundles of hosted tool configurations managed in a Foundry project |

[msft-docs-source]: https://learn.microsoft.com/en-us/training/modules/develop-ai-agent-with-semantic-kernel/
