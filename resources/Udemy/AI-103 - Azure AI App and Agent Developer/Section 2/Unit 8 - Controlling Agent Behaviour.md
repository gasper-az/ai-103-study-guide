# Controlling Agent Behaviour

Source: [Unit 8 - Controlling Agent Behaviour][src-udemy]

## What is a system instruction?

- **System instruction**: the system message is separate from user messages. It tells the LLM *who you are* and *how to behave* for the entire conversation.
- **Persistence across turns**: unlike user messages, the system instruction is included in every API call. The agent never forgets its core rules.

## Boundaries

- **Hard boundaries**: these are absolute prohibitions. Ex: "never delete user data; never share PII".
- **Soft boundaries**: these allow exceptions with conditions. EX: "only share refund amounts if the user has verified their order number".

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569043#overview
