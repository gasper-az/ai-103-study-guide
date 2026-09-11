# Multi-agent orchestration - MSFT Agent Framework

Source: [Unit 9 - Multi-agent orchestration - MSFT Agent Framework][src-udemy]

## What is MSFT agent framework (MAF)?

It is the official successor to Semantic Kernel and AutoGen, a unified SDK for building multi-agent systems in Python and C#.

## The three core orchestration patterns

- **Magentic (Manager) pattern**: one central manager agent receives user requests and delegates subtaks to specialized sub-agents. The manager controls the flow.
- **Handoff pattern**: agents explicitely transfer conversation control to another agent, passing all context and state.
- **Group chat pattern**: multiple agents share a conversation space. A speaker selection algorithm decides which agent speaks next based on the conversation.

## Magentic pattern

- **Manager responsibilities**: the manager agent has system instructions that say "you coordinate specialists. Do not answer user questions directly. Delegate to the correct sub-agent".
- **Sub-agent specialization**: Each sub-agent handles one domain: RefundAgent, TechnicalSupportAgent, AccountAgent. Sub-agents have no awareness of each other.

## Handoff pattern

- **State transfer**: when handing off, Agent A passes the conversation history, user information, and any partial work to Agent B. The user sees no interruption.

## Group chat pattern

In group chat pattern, multiple agents participate in a shared conversation space, taking turns speaking based on conversation context.

- **Shared space definition**: all agents see every message in the conversation. No single manager controls who speaks. Agents respond when relevant to their expertise.
- **Speaker selection algorithm**: The framework runs an algorithm that evaluates each agent's system instructions and the conversation to decide which agent speaks next.

## Orchestrator - The runtime coordinator

The orchestrator is the runtime component that receives user messages, routes them to the correct agent, and manages conversation state.

- **Responsibilities**: It holds the list of registered agents, maintains conversation history, runs speaker selection (for Group chat), and routes messages.
- **Stating a conversation**: Call `orchestrator.start_conversation(user_id, initial_message)`. The orchestrator selects the first agent based on the message content.
- **Conversation ID**: each conversation gets an unique ID. The orchestrator uses it to retrieve conversation history and route subsequent messages to the correct agent chain.

## Monitoring multi-agent with foundry trace

- **Trace span per agent**: each agent action creates a span (a logged operation) with agent name, operation type (message, tool call, handoff), and duration.
- **Visualizing handoffs**: Foundry trace shows handoffs as connecting lines between agent spans. You can see exactly which agent transferred to which agent and why.
- **Debugging reasoning loops**: if agents hand off back and forth without progress, Foundry traces shows the cycle. Identify which agent made the wrong handoff decision.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569049#overview
