# Microsoft Foundry - The agent development platform

Source: [Unit 02 - Microsoft Foundry - The agent development platform][src-udemy]

## What is it?

It is a single Azure service that provides model deployment, agent hosting, security, and debugging tools.

- **Core purpose**: it eliminates the need to stitch together separate Azure services. One platform handles everything an agent needs to run.
- **What it includes**: model catalog, agent service (runtime environment), foundry trace (debugging), and Entra Agent ID (identity).
- **Foundry vs Building from scratch**: Foundry packages OpenAI, App Service, Cosmos DB, and monitor services (and others).

## The agent workspace

A foundry project is a workspace inside a hub where you build, test, and deploy a specific agent or group of related agents.

- **Project definition**: it contains everything for one agent solution (agent code, system message, tool definitions, memory configuration, and trace logs).
- **Project vs Hub relationship**: a hub can contain many projects. Each project inherits security settings from its hub, but can have its own specific configuration.

## Deploying LLMs

- **Model catalog definition**: it shows all available model from MSFT, OpenAI, and other providers in one place.
- **Deploying a model**: it means reserving a copy of a model exclusively for your agent. You choose the model version and pay for the computing time.
- **Model vs Agent relationship**: an agent uses a deployed model as its reasoninig engine. One agent can use one model, or different agents can use different models.

## What is an endpoint?

- **Definition**: it is an URL provided by Foundry.
- **Why do they matter?**: the application code sends requests to the endpoint URL. Without the endpoint, your code has no way to reach the deployed model or agent.
- **Endpoints contain keys**: each endpoint has an associated API key (a secret password) that proves the code is authorized to call such endpoint.

## Foundry project hierarchy

- **Level 1 - Azure subscription**: billng and account container.
- **Level 2 - Foundry Hub**: contains projects, shared models, and shared security polices. Isolates teams or business units.
- **Level 3 - Foundry project**: contains agent code, tool definitions, memory settings, and trace logs. One project per agent or agent group.
- **Level 4 - Agent service deployment**: the running instance of the agent. Deployed from a project. Has its own endpoint URL and API keys.

## Agent identity blueprints

An agent identity blueprint is a template that defines what permissions and access rules an agent should have when deployed.

- **Blueprint definition**: it is a reusable confiiguration file that specificies which databases, APIs, and storage an agent can access.
- **Why use them?**: instead of configuring permissions for each agent individually, you can create a blueprint once and apply it to many agents of the same type.
- **Blueprint vs Agent ID**: the agent ID is the unique identity. The blueprint is the permission template applied to such identity.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569029#overview
