# Build agent-driven workflows using Microsoft Foundry

Source: [Build agent-driven workflows using Microsoft Foundry][source-msft-docs]

Modern AI solutions often rely on multiple agents working together to analyze inputs, make decisions, and take action. In Microsoft Foundry, agent workflows provide a way to orchestrate these interactions using a combination of agents, control flow, and runtime safeguards.

## Understand Workflows

A workflow consists of connected nodes, where each node performs a specific function. Some nodes invoke agents, while others evaluate conditions, manage data, or communicate with users. Together, these nodes form an execution path that determines how requests move through the system. By arranging and configuring nodes, you control how information flows and how decisions are made.

One of the key advantages of workflows is their ability to coordinate multiple agents. Single-agent solutions often struggle with complex or ambiguous tasks, but workflows allow you to combine agents with different responsibilities—such as classification, decision-making, and resolution—into a cohesive process. This orchestration enables more robust and scalable automation.

## Identify Workflow Patterns

A **sequential** workflow follows a fixed, step-by-step path. Each node executes in order, passing its output to the next step in the workflow. This pattern works well for pipelines and multi-stage processes, such as validating input, enriching data, and generating a final response. Sequential workflows are predictable and easy to reason about, making them a good starting point when you're learning how workflows operate.

A **human-in-the-loop** workflow introduces pauses where user input or approval is required before the workflow can continue. In this pattern, the workflow explicitly asks a question, waits for a response, and then resumes execution based on that input. Human-in-the-loop workflows are useful when automation must be balanced with oversight—such as approvals, confirmations, or situations where missing context needs to be provided by a person.

A **group chat** workflow enables more dynamic orchestration across multiple agents. Instead of following a fixed path, control can shift between agents based on context, rules, or intermediate results. This pattern is useful for scenarios where multiple specialized agents collaborate to handle complex requests, such as customer support or multi-domain question answering. Group chat workflows allow for flexible interactions, where agents can build on each other's outputs and adapt to changing inputs.

## Create workflows in Microsoft Foundry

The main node types in the workflow builder are:

- *Invoke*: Invokes an AI agent from your project or creates a new one. Agent nodes can return free-text responses or structured outputs (like JSON) that other nodes can use. They're used for classification, reasoning, recommendations, or any AI-driven task.
- *Flow*: Controls the workflow's execution path. Flow nodes let your workflow adapt dynamically to different inputs or situations. Flow nodes include:
  - If/Else: Branches execution based on conditions.
  - Go To: Jumps to another node in the workflow.
  - For Each: Loops over a list of items, performing the same actions for each one.
- *Data transformation*: Manipulates data and manages variables. Data transformation nodes ensure that information is correctly passed to subsequent steps. Data transformation nodes include:
  - Set Variable: Assigns a value to a variable for later use.
  - Reset Variable: Clears or reinitializes a variable.
  - Parse value: Extracts specific data from structured outputs or converts values to different formats.
- *Basic chat*: Sends messages to the user or asks questions to collect input. These nodes are often paired with variables to capture responses, which can then influence logic or agent decisions later in the workflow.
- *End*: Marks the conclusion of a workflow. The End node can optionally return a final result or status.

## Apply Power Fx in Workflows

Power Fx is the low-code, Excel-like language that acts as the glue of a workflow. It allows you to manipulate data, evaluate conditions, and control the flow of execution without writing complex code.

[source-msft-docs]: https://learn.microsoft.com/en-us/training/modules/build-agent-workflows-microsoft-foundry/
