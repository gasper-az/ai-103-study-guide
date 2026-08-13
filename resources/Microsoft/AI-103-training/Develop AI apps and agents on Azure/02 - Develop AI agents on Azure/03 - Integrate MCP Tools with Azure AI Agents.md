# Integrate MCP Tools with Azure AI Agents

Source: [Integrate MCP Tools with Azure AI Agents][MSFT-docs-source]

AI agents are capable of performing a wide range of tasks, but many tasks still require them to interact with tools outside the large language model. Agents may need to access APIs, databases, or internal services. Manually integrating and maintaining these tools can quickly become complex, especially as your system grows, or changes frequently.

Model Context Protocol (MCP) servers can help solve this problem by integrating with AI agents. Connecting an Azure AI Agent to a Model Context Protocol (MCP) server can provide your agent with a catalog of tools accessible on demand.

## Understand MCP tool discovery

**Advantages of the Model Context Protocol for AI agents**:

The Model Context Protocol (MCP) provides several benefits that enhance the capabilities and flexibility of AI agents:

- **Dynamic Tool Discovery**: AI agents can automatically receive a list of available tools from a server, along with descriptions of their functions. Unlike traditional APIs, which often require manual coding for each integration and updates whenever the API changes, MCP enables an *integrate once* approach that improves adaptability and reduces maintenance.
- **Interoperability Across LLMs**: MCP works seamlessly with different large language models (LLMs), allowing developers to switch or evaluate core models for improved performance without reworking integrations.
- **Standardized Security**: MCP provides a consistent authentication method, simplifying secure access across multiple MCP servers. This eliminates the need to manage separate keys or authentication protocols for each API, making it easier to scale AI agent deployments.

**What is dynamic tool discovery?**:

It is a mechanism that allows an AI agent to discover available external tools without needing hardcoded knowledge of each one. Instead of manually adding or updating every tool your agent can use, the agent queries a centralized Model Context Protocol (MCP) server. This server acts as a live catalog, exposing tools that the agent can understand and call.

This approach means:

- Tools can be *added, updated, or removed* centrally without modifying the agent code.
- Agents can always use the *latest version of a tool*, improving accuracy and reliability.
- The *complexity of managing tools shifts away* from the agent and into a dedicated service.

**How does MCP enable dynamic tool discovery?**:

An MCP server hosts a set of *functions* that are *exposed as tools* using the `@mcp.tool decorator`.

*Tools* are a primitive type in the MCP that enables servers to *expose executable functionality to clients*.

A *client* can *connect* to the server and *fetch* these tools *dynamically*.

The client then *generates function wrappers* that are added to the *Azure AI Agent's tool definitions*.

This setup creates a flexible pipeline:

- The MCP server hosts available tools.
- The MCP client dynamically discovers the tools.
- The Azure AI Agent uses the available tools to respond to user requests.

**Why use dynamic tool discovery with MCP?**:

This approach provides several benefits:

- **Scalability**: Easily add new tools or update existing ones without redeploying agents.
- **Modularity**: Agents can remain simple, focusing on delegation rather than managing tool details.
- **Maintainability**: Centralized tool management reduces duplication and errors.
- **Flexibility**: Supports diverse tool types and complex workflows by aggregating capabilities.

## Integrate agent tools using an MCP server and client

**What is the MCP Server?**:

The MCP server acts as a registry for tools your agent can use. You can initialize your MCP server using `FastMCP("server-name")`. The *FastMCP* class uses Python type hints and document strings to automatically generate tool definitions, making it easy to create and maintain MCP tools. These definitions are then served over HTTP when requested by the client.

**What is the MCP Client?**:

A standard MCP client acts as a *bridge* between your MCP server and the Azure AI Agent Service. The client **initializes** an *MCP client session* and connects to the server. Afterwards, it performs three key tasks:

- *Discovers* available tools from the MCP server using `session.list_tools()`.
- *Generates* Python function *stubs* that wrap the tools.
- *Registers* those functions with your agent.

**Register tools with an Azure AI Agent**:

When an MCP client session is initialized, the client can dynamically pull in tools from the MCP server. An MCP tool can be invoked using session.call_tool(tool_name, tool_args). The tools should each be wrapped in an async function so that the agent is able to invoke them. Finally, those functions are bundled together and become part of the agent's toolset and are available during runtime for any user request.

Overview of MCP agent tool integration

- The **MCP server** hosts tool definitions decorated with `@mcp.tool`.
- The **MCP client** initializes an MCP client connection to the server.
- The **MCP client** fetches the available tool definitions `with session.list_tools()`.
- Each tool is *wrapped in an async function* that invokes `session.call_tool`.
- The tool functions are bundled into `FunctionTool` that makes them usable by the agent.
- The `FunctionTool` is registered to the agent's toolset.

## Use Azure AI agents with MCP servers

**Integrating remote MCP servers**:

To connect to an MCP server, you need:

- A remote MCP server endpoint (for example, https://api.githubcopilot.com/mcp/).
- A Microsoft Foundry agent configured to use the MCP tool.

You can connect to multiple MCP servers by adding them to your agent as separate tools. Each `MCPTool` can include the following parameters:

- `server_label`: A unique identifier for the MCP server (e.g., GitHub).
- `server_url`: The MCP server’s URL.
- `allowed_tools` (optional): A list of specific tools the agent is allowed to access.
- `require_approval` (optional): A boolean that determines whether tool invocations require *human approval.* If set to true, the agent will pause and wait for approval before invoking any tools on the MCP server.

The MCP tool also supports custom headers, which let you pass:

- Authentication keys (API keys, OAuth tokens).
- Other required headers for the MCP server.

**Invoking tools**:

When using the Azure MCP Tool object, you don't need to wrap function tools or invoke `session.call_tool`. Instead, the tools are automatically invoked when necessary during an agent run. To automatically invoke MCP tools:

- Create the `MCPTool` object with the server label and url.
- Use `update_headers` to apply any headers required by the server.
- Use the `require_approval` parameter to determine whether approval is required. Supported values are:
  - **always**: A developer needs to provide approval for every call. If you don't provide a value, this one is the *default*.
  - **never**: No approval is required.
- Create an agent and add the `MCPTool` object to its tools list
- Invoke a prompt on the agent, you should see the results of any invoked tools in the response.

If the model tries to invoke a tool in your MCP server with approval required, you get an `mcp_approval_request` in the agent response. This includes information about which tool is being invoked, and you can use this information to decide whether to approve the request. To approve, you send a follow-up message with the `mcp_approval_response` object, which includes an `approval_request_id` value and an `approve` boolean.

MCP integration is a key step toward creating richer, more context-aware AI agents. As the MCP ecosystem grows, you’ll have even more opportunities to bring specialized tools into your workflows and deliver smarter, more dynamic solutions.

[MSFT-docs-source]: https://learn.microsoft.com/en-us/training/modules/connect-agent-to-mcp-tools/
