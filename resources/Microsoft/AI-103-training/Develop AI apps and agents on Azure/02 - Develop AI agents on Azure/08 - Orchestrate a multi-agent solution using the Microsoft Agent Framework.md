# Orchestrate a multi-agent solution using the Microsoft Agent Framework

Source: [Orchestrate a multi-agent solution using the Microsoft Agent Framework][source-msft-docs]

## Understand agent orchestration

### Core Components of a Workflow

**Executors**:

Executors are the main workers in a workflow. They receive input messages, perform specific actions, and produce outputs that move the workflow toward completing its goal.
Executors can represent AI agents or custom logic components.

**Edges**:

Edges define how messages flow between executors, determining the logic and order of execution. The Microsoft Agent Framework supports several types of edges:

- **Direct Edges**: Connect one executor directly to another in sequence.
  - Example: After an AI agent gathers user input, the next executor processes the booking.*
- **Conditional Edges**: Trigger only when certain conditions are met.
  - Example: If hotel rooms are unavailable, the workflow branches to an executor that suggests alternative dates or locations.*
- **Switch-Case Edges**: Route messages to different executors based on predefined conditions.
  - Example: VIP customers might be routed to a premium service executor, while others follow the standard process.*
- **Fan-Out Edges**: Send a single message to multiple executors simultaneously.
  - Example: One request could be sent to several agents — one checking flights, another checking hotels.*
- **Fan-In Edges**: Combine multiple messages from different executors into one for a final step.
  - Example: After gathering hotel and flight results, a summary executor compiles them into a single travel itinerary.*

**Events**:

The Microsoft Agent Framework includes built-in events to improve observability and debugging during workflow execution. These events help developers monitor progress, track errors, and analyze system performance.

| Event Name | Description |
| --- | --- |
| **WorkflowStartedEvent** | Triggered when workflow execution begins. |
| **WorkflowOutputEvent** | Emitted when the workflow produces an output. |
| **WorkflowErrorEvent** | Occurs when an error is encountered. |
| **ExecutorInvokeEvent** | Fired when an executor starts processing a task. |
| **ExecutorCompleteEvent** | Fired when an executor finishes its work. |
| **RequestInfoEvent** | Logged when an external request is issued. |

**Supported orchestration patterns**:

Microsoft Agent Framework provides several orchestration patterns directly in the SDK, each offering a different approach to coordinating agents. These patterns are designed to be technology-agnostic so you can adapt them to your own domain and integrate them into your existing systems.

- *Concurrent orchestration*: Broadcast the same task to multiple agents at once and collect their results independently. Useful for parallel analysis, independent subtasks, or ensemble decision making.
- *Sequential orchestration*: Pass the output from one agent to the next in a fixed order. Ideal for step-by-step workflows, pipelines, and progressive refinement.
- *Handoff orchestration*: Dynamically transfer control between agents based on context or rules. Great for escalation, fallback, and expert routing where one agent works at a time.
- *Group chat orchestration*: Coordinate a shared conversation among multiple agents (and optionally a human), managed by a chat manager that chooses who speaks next. Best for brainstorming, collaborative problem solving, and building consensus.
- *Magentic orchestration*: A manager-driven approach that plans, delegates, and adapts across specialized agents. Suited to complex, open-ended problems where the solution path evolves.

[source-msft-docs]: https://learn.microsoft.com/en-us/training/modules/orchestrate-semantic-kernel-multi-agent-solution/
