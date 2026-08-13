# Develop AI agents with Microsoft Foundry and Visual Studio Code

Source: [Develop AI agents with Microsoft Foundry and Visual Studio Code][msft-doc-source]

## Understand AI agents and Microsoft Foundry Agent Service

An AI agent is a software service that uses generative AI to understand and perform tasks on behalf of users or other programs. Unlike traditional applications that follow predetermined rules, AI agents can operate independently by understanding context, making decisions, and taking actions to achieve specific goals.

**Why AI agents are useful**:

AI agents provide significant value across multiple dimensions:

- Automation of routine tasks
- Enhanced decision-making
- Scalability
- 24/7 availability

**Security considerations for AI agents**:

| Risk Area | Description | Example Impact |
| --- | --- | --- |
| Data leakage and privacy exposure | Agents often access sensitive business or user data. Without proper controls, they can unintentionally expose confidential information. | An agent summarizing internal files accidentally includes private data in customer-facing responses. |
| Prompt injection and manipulation attacks | Malicious users craft inputs that override an agent's intended behavior, tricking it into revealing data or performing unauthorized actions. | Hidden instructions in a message cause the agent to leak system credentials. |
| Unauthorized access and privilege escalation | Weak authentication or access controls let agents, or bad actors controlling them, access systems they shouldn't. | An agent connected to a CRM tool performs admin-level actions like exporting or deleting records. |
| Data poisoning | Attackers corrupt training or contextual data, causing agents to make biased, incorrect, or unsafe decisions. | A poisoned dataset causes a customer support agent to recommend harmful content. |
| Supply chain vulnerabilities | Agents rely on external APIs, plugins, or model endpoints, expanding the attack surface. | A compromised third-party plugin injects malicious code into the agent's workflow. |
| Over-reliance on autonomous actions | Highly autonomous agents may execute unintended actions if not carefully constrained or validated. | An agent mistakenly sends payments or publishes unverified content. |
| Inadequate auditability and logging | Without detailed logging, it's difficult to trace actions or detect malicious behavior early. | Security teams can't identify data misuse due to missing activity logs. |
| Model inversion and output leakage | Attackers might exploit model outputs to infer sensitive data used during training or prompting. | Repeated queries extract private information from a fine-tuning dataset. |

**Mitigation strategies**:

To reduce these risks, adopt a security-by-design approach that includes:

- Enforcing *role-based access controls (RBAC)* and least privilege permissions.
- Adding *prompt filtering and validation layers* to prevent injection attacks.
- Sandboxing or gating sensitive operations behind *human-in-the-loop approvals*.
- Maintaining comprehensive *logging and traceability* for all agent actions.
- *Auditing* third-party dependencies and integrations regularly.
- *Continuously retraining and validating models* to detect data drift or poisoning attempts.

## Configure and manage agents in Visual Studio Code

- **Version control your YAML files**: Commit agent configurations to Git alongside your application code. This enables rollback, code review, and change tracking.
- **Use descriptive names and tags**: Clear naming and tagging make it easy to find and identify agents as your collection grows.
- **Document complex instructions**: Include comments in your YAML files explaining why you chose specific instruction patterns or configurations.
- **Test after every change**: Use the integrated playground to verify behavior after modifying configuration. Small changes can have unexpected effects.
- **Start simple, then iterate**: Begin with basic instructions and add complexity based on testing results. Overly complex initial instructions are harder to debug.
- **Keep instructions focused**: Each agent should have a clear, specific purpose. Agents trying to do too many things perform inconsistently.

## Extend agent capabilities with tools

**Understanding agent tools**:

Tools are programmatic functions that agents can invoke to complete tasks. When an agent determines that a tool is needed to respond to a user request, it automatically calls the appropriate tool, processes the results, and incorporates them into its response. This capability enables agents to work with real-time data, execute code, search knowledge bases, and interact with external services.

The tool-calling lifecycle happens automatically:

1. User sends a message to the agent.
1. Agent analyzes the request and determines which tools (if any) are needed.
1. Agent invokes the appropriate tools with relevant parameters.
1. Tools execute and return results.
1. Agent incorporates results into a natural language response.
1. Response is returned to the user.

## Test, deploy, and integrate agents

**Testing strategies for agents**:

Thorough testing ensures your agents behave reliably across diverse scenarios before reaching users. Both the Foundry portal and Visual Studio Code extension provide playgrounds for interactive testing.

Using the playground effectively:

- **Happy path testing**: Verify the agent handles common, expected requests correctly.
- **Edge case testing**: Try ambiguous inputs, incomplete information, and unusual requests to reveal how agents handle uncertainty.
- **Boundary testing**: Confirm the agent respects boundaries defined in its instructions by testing out-of-scope requests.
- **Multi-turn conversation testing**: Verify the agent maintains context across multiple exchanges and builds on previous responses.
- **Tool invocation testing**: Verify agents call the right tools at the right times and incorporate results correctly.

[msft-doc-source]: https://learn.microsoft.com/en-us/training/modules/develop-ai-agents-azure-vs-code/
