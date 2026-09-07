# Entra Agent ID - Agent Identity deep dive

Source: [Unit 03 - Entra Agent ID - Agent Identity deep dive][src-udemy]

## What is a Service Principal

It is the formal Azure term for an identity that represents an application or agent, not a human user.

- **Definition**: It proves the agent exists and has permissions to act.
- **Human vs Service Principal**: A human logs in with an username and a password. A Service Principal logs in with a client ID and a secret or certificate.
- **Agent as Service Principal**: when you register an agent with Entra Agent ID, you are creating a service principal for such agent. The agent now has its own identity.

## Authentication flows for agents

- **Client credentials flow**: the agent sends its client ID and secret (or certifcate) to Entra ID. Entra D verifies and returns an access token. This is the primary flow for agents.
- **Token definition**: a token is a time-limited digital pass that proves the agent has authenticated. Token typically expire after one hour for security.
- **Using the token**: the agent includes the token in the `Authorization` header of every API request: `Authorization: Bearer`.

## DefaultAzureCredential for Agents

The class tries: environment variables (first), then managed Identity (if on Azure), then Visual Studio logn, then Azure CLI login.

## Agent Identity Blueprints - Permission templates

- **Blueprint structure**: a blueprint contians a list of role assignments.
- **Applying the blueprint**: after creating a blueprint, your assign it ton an agent's Service Principal. The agent immediately receives all permissions listed in the Blueprint.
- **Blueprint versioning**: you can upate a blueprint. All agents using that Blueprint automatically receve the updated permissions. No need to reconfigure each agent.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569031#overview
