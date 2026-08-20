# Integrate your agent with Microsoft 365

Source: [Integrate your agent with Microsoft 365][msft-docs-src]

## Understand Foundry agent publishing options

**Understanding agent applications**:

When you publish an agent, Microsoft Foundry creates an Agent Application resource with:

- **Dedicated invocation URL**: A stable endpoint that remains consistent as you update agent versions.
- **Agent identity**: A distinct Microsoft Entra identity separate from your development project.
- **User data isolation**: Inputs and interactions from one user aren't available to other users.

**Publish scopes**:

When publishing to Microsoft 365, you choose between two distribution scopes:

| Scope | Description | Best for |
| --- | --- | --- |
| Shared | Available immediately without admin approval. Appears under Your agents in Teams. | Personal testing, small team pilots |
| Organization | Available to everyone in your tenant under Built by your org. Requires admin approval. | Production deployments |

**Agent identity and permissions**:

When you publish an agent, the system creates a distinct agent identity. This matters because:

- The agent authenticates to Azure resources using its own identity.
- Development-time permissions on your project identity don't transfer automatically.
- Tools that access Azure services need permissions reconfigured after publishing.

If your agent uses tools that connect to services like Azure AI Search, grant the published agent's identity appropriate permissions.

**Prerequisites for publishing**:

Before publishing an agent to Microsoft 365, ensure you have:

- Azure AI Project Manager role on your Foundry project.
- Azure AI User role on the agent application scope.
- An Azure subscription where you can create Azure Bot Service resources.
- Permissions to register applications in Microsoft Entra ID.
- A Microsoft 365 tenant that allows custom apps and bots.

## Publish an agent from Foundry portal to Teams

**Verify your permissions**:

Confirm you have the required role assignments:

- Azure AI Project Manager role on your Foundry project to publish agents.
- Azure AI User role to invoke or chat with published agents.
- Permissions to create resources in your Azure subscription.
- Permissions to register applications in Microsoft Entra ID.

**Prepare metadata**:

Gather the following information before starting:

- A display name for your agent (appears in the Teams agent store).
- A brief description of what your agent does.
- Small (32x32 pixels) and large (192x192 pixels) icons in PNG format.
- Your organization's name and contact details.
- URLs for your privacy policy and terms of use.

## Advanced - Use Microsoft 365 Agents Toolkit

For most scenarios, publishing directly from the Foundry portal is the simplest path to get your agents into Microsoft Teams and Microsoft 365 Copilot. However, some complex enterprise scenarios require additional control over the integration layer. The Microsoft 365 Agents Toolkit provides an alternative approach for these situations.

Consider using it when your agent requires custom single sign-on (SSO) configuration beyond the default Entra ID setup, or when you need to add middleware logic for custom processing, logging, or transformation between Teams and your Foundry agent.

## Access Microsoft 365 data with Work IQ

When building agents that help users with workplace tasks, access to organizational data can dramatically improve the agent's usefulness. Microsoft Work IQ provides a way to connect AI agents to Microsoft 365 data, including emails, meetings, documents, Teams messages, and people information.

**What is Work IQ?**:

Microsoft Work IQ is a command-line interface (CLI) and server that connects AI assistants to your Microsoft 365 Copilot data. It enables agents to query workplace information using natural language, providing rich context that helps agents give more relevant and accurate responses.

Work IQ accesses data across multiple Microsoft 365 services:

| Data type | Example capabilities |
| --- | --- |
| Emails | Search messages, find communications from specific people |
| Meetings | Check calendar, retrieve meeting notes and decisions |
| Documents | Find files in SharePoint and OneDrive, search content |
| Teams messages | Summarize channel discussions, find specific conversations |
| People | Identify team members, find collaborators on projects |

**Understanding MCP servers**:

Work IQ is built on the Model Context Protocol (MCP), an open protocol that enables AI assistants to connect to external data sources and tools. Understanding MCP helps explain how Work IQ functions.

An MCP server exposes capabilities that AI agents can use. These capabilities might include:

- Tools: Actions the agent can take, like searching for documents or sending messages.
- Resources: Data sources the agent can query.
- Prompts: Predefined templates for common queries.

**Security and data access**:

Work IQ inherits the security model of Microsoft 365 Copilot:

- *Permission-based access*: Work IQ can only access data you already have permission to view.
- *No data storage*: Work IQ doesn't store your Microsoft 365 data; it retrieves information on-demand.
- *Enterprise security*: All data access follows your organization's security policies.
- *Admin visibility*: Administrators can monitor and control Work IQ usage.

When you query Work IQ, it accesses data through Microsoft Graph with your authenticated identity. This means:

- You can't access documents you don't have permission to view.
- Queries are auditable by your organization.
- Data protection policies apply to Work IQ queries.

> The CLI is useful for quick, ad-hoc queries during development. Run the workiq ask command directly from your terminal.
> When Work IQ runs as an MCP server, your AI assistant can access the same Microsoft 365 data automatically. Instead of running CLI commands, you interact naturally with your AI assistant, which calls Work IQ tools behind the scenes.

## Test and iterate your integrated agent

### Common troubleshooting scenarios

**Agent doesn't respond in Teams**:

Possible causes:

- Azure Bot Service isn't running
- Bot Service configuration is incorrect
- Network issues between Teams and your agent

Resolution:

- Verify the Bot Service resource exists in the Azure portal.
- Check Bot Service logs for errors.
- Confirm the agent is published and the package was uploaded correctly.

**Tools work in Foundry but fail in Teams**:

Possible cause: The published agent identity doesn't have the required permissions.

Resolution:

- Find the published agent's identity in the Foundry portal.
- In the Azure portal, locate the resources your tools access.
- Assign appropriate RBAC roles to the published agent identity.

**Users can't find the agent**:

Possible causes:

- Wrong publish scope selected
- Admin approval pending (for organization scope)
- Tenant policies block custom apps

Resolution:

- For shared scope: Share the direct link with users.
- For organization scope: Verify admin approval in the Microsoft 365 admin center.
- Check tenant settings for custom app permissions.

**Slow response times**:

Possible causes:

- Complex agent instructions requiring extended processing
- Tools that query large data sets
- Network latency

Resolution:

- Simplify agent instructions where possible.
- Optimize tool configurations.
- Test from different network locations to isolate network issues.

[msft-docs-src]: https://learn.microsoft.com/en-us/training/modules/integrate-foundry-agent-with-m365/
