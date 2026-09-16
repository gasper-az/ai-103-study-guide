# MCP

Source: [Unit 11 - MCP][src-udemy]

## What is the Model Context Protocol?

- **MCP**: it is a protocol that any agent and any tool can implement in order to communicate without custom code.

## MCP handshake

It is a two-step process: the agent discovers what tools a server offers, then connects and calls them using standard JSON messages.

- **Step 1 - discovery**: agent sends `list_tools` request to MCP server. Server returns JSON list of available tools, their descriptions, and parameter schemas.
- **Step 2 - connection**: agent sends `call_tool` request with tool name and parameters. Server executes the tool and returns a standard JSON response.
- **No custom code**: the agent does not need to know the tool's API details in advance. The tool describes itself during discovery.

## MCP Server - The tool provider

It is a service that exposes one or more tools to agents using the MCP protocol, acting as a bridge between agents and enterprise systems.

- **MCP server**: The server runs in your infrastructure, connected to your tools (SharePoin, ddbb, internal APIs). It translates MCP calls to actual API calls.
- **Server responsibilities**: the server authenticates to downstream tools, handles rate limits, transforms responses to MCP format, and logs all tool calls.
- **Who runs the server?**: your team deploys and maintains MCP servers. MSFT does not provide them. Each enterprise deploys its own servers for its internal tools.

## MCP client - The agent side

It is the component inside your agent that speaks the MCP protocol, discovering tools and sending call requests to MCP servers.

- **MCP client in Agent Framework**: MSFT Agent Framework includes a built-in MCP client. Your agent uses it without writing protocol code.
- **Client discovery flow**: The client sends `list_tools` to the server URL you configure. The client caches the tool list for the session.
- **Client call flow**: When the agent decides to call a tool, the client sends `call_tool` with the tool name and parameters. The client returns the response to the agent.

## MCP tool discovery - The list_tools response

When an agent sends a `list_tools` request, the MCP server returns a JSON array describing each available tool, its description, and parameter schema.

- **Response structure**: `{ "tools": [{"name": "search_sharepoint", "description": "Search...", "input_schema": { "query": "string", "site_id": "string" }}]}`.
- **Agent uses description**: the agent reads the `description` field to understand what the tool does. The LLM uses this description to decide when to call the tool.
- **Schema for parameters**: The `input_schema` tells the agent what parameters the tool expects and their data types (string, number, boolean, array).

## MCP tool call - The call_tool request and response

To call a tool, the agent sends a `call_tool` JSON message with the tool name and parameters. The server executes the tool and returns a standardized response.

- **Request structure**: `{"method": "call_tool", "params": {"name": "search_sharepoint", "arguments": {"query": "budget report", "site_id": "finance" }}}`.
- **Response structure - Success**: `{"content": [{"type": "text", "text": "Found 3 documents: ..."}]}`.
- **Response structure - Error**: `{"isError": true, "content": [{ "type": "text", "text": "Sharepoint authentication Failed"}]}`. Agent sees error and can retry or escalate.

## Configuring MCP servers in foundry

In Foundry, you configure MCP servers endpoints per agent or per project, providing the URLs and authentication credentials for each server.

- **Server configuration in Foundry**: in agent settings, add an MCP server entry with `url`, `auth_type` (bearer token, API key), and `credentials`.
- **Multiple Servers per agent**: An agent can connect to multiple MCP servers. The agent discovers tools from all servers and can call any discovered tool.
- **Environment-specific urls**: configure different server URLs for development (`dev-mcp.company.com`), staging, and production. Use environment variables to switch.

## MCP authentication - How agents prove identity

MCP servers authenticate agents using bearer tokens (JWT) or API keys, which are configured in Foundry and injected automatically by the MCP client.

- **Bearer token flow**: Agent sends `Authorization: Bearer <token>` header with every MCP request. Foundry obtains the token from Entra ID using the agent's managed identity.
- **API key flow**: Configure a static API key in Foundry. The MCP client adds `X-API-Key: <key>` header to all requests to that server.
- **Entra Agent ID integration**: MCP servers can validate the bearer token aagainst Entra ID, ensuring the agent has permission to call the tool based on its Agent ID.

## MCP vs. custom tool code - When to use each?

- **Use MCP when**: you have 10+ tools, tools are added or removed frequently, or tools are provided by different teams. MCP eliminates per-tool code changes.
- **Use custom code when**: you have one or two tools, tools never change, or you need deep optimization (e.g., streaming responses, custom retry logic).
- **Hybrid approach**: Use MCP for standard enterprise tools (SharePoint, Salesforce, ddbb). Write custom tools for agent-specific logic (validation, aggregation).

## MCP observability with Foundry trace

Foundry trace records all MCP operations: discovery requests, tool calls, responses, and errors. They are recorded as spans in the agent's trace.

- **Discovery span**: When agent calls `list_tools`, Foundry Trace creates a span with attributes: `mcp_server_url`, `tools_count` (number discovered), `duration_ms`.
- **Tool call span**: Each `call_tool` creates a span with `tool_name`, `parameters_json`, `response_length`, `status` (success/error), and `duration_ms`.
- **Debugging failures**: If a tool call fails, the span includes `error_type` and `error_message`. Filter traces by `mcp_server_url` to find all failures from a specific server.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57591191#overview
