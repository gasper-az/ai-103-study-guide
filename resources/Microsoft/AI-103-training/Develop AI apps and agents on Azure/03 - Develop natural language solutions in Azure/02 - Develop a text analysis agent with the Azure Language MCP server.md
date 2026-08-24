# Develop a text analysis agent with the Azure Language MCP server

Source: [Develop a text analysis agent with the Azure Language MCP server][src-msft-docs]

Azure Language in Foundry Tools provides a set of natural language processing (NLP) capabilities that you can use to analyze text. These capabilities include language detection, named entity recognition, and personally identifiable information (PII) extraction.

While you can call these capabilities individually through REST APIs or SDKs, you can also make them available to an AI agent through the Azure Language Model Context Protocol (MCP) server. This approach lets the agent dynamically select and call the appropriate language tool based on a user's request, without you needing to write specific code for each capability.

## Understand the Azure Language MCP server

**Azure Language MCP server capabilities**:

| Capability | Description |
| Language Detection | Identifies the language in which text is written. |
| Named Entity Recognition | Identifies and categorizes entities in text, such as people, places, organizations, dates, and quantities. |
| PII Redaction | Detects and redacts personally identifiable information (PII) such as names, addresses, and phone numbers. |
| Text Analytics for Health | Extracts and labels medical entities (such as diagnoses, medications, and symptoms) from clinical text. |

## Connect and use the Language MCP server with an agent

After you understand the capabilities of the Azure Language MCP server, the next step is to connect it to an agent and start using it. This involves creating an agent in Microsoft Foundry, connecting the Language MCP tool, testing it in the agent playground, and optionally building a client application to interact with the agent programmatically.

**Build a client application**:

To build a client application, you use the `azure-ai-projects` and `azure-identity` packages. The general pattern is:

1. Create an `AIProjectClient` using your Foundry project endpoint and `DefaultAzureCredential` (which uses your Azure CLI credentials in development).
1. Get an OpenAI client from the project client by calling `get_openai_client()`.
1. Call `responses.create()` to send a user prompt to the agent.

The key part is how you reference the agent — you specify it by name in the `extra_body` parameter:

```python
response = openai_client.responses.create(
    input=[{"role": "user", "content": user_prompt}],
    extra_body={
        "agent_reference": {
            "name": "Text-Analysis-Agent",
            "type": "agent_reference"
        }
    },
)

print(response.output_text)
```

**Connect the MCP server in code**:

```python
from azure.ai.projects.models import MCPTool

mcp_tool = MCPTool(
    server_label="azure-language",
    server_url="https://{foundry-resource-name}.cognitiveservices.azure.com/language/mcp?api-version=2025-11-15-preview",
    require_approval="always",
)
```

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/develop-text-analysis-agent-language-mcp/
