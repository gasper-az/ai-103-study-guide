# What is an AI Agent?

Source: [Unit 01 - What is an AI Agent?][src-udemy]

## Three core capabilities

1. **Reasoning**: an agent can break a goal into steps and decide what to do net based on the current situation.
1. **Tool use**: taking action. AN agent can call external services to perform actions or to get information.
1. **Memory**: remembering context. An agent can recall previous conversations, user preferences, or past decisions.

## Simple API Call vs Agent

- **Simple API call flow**: No memory, no tools.
- **Agent flow with tool use**: An agent receives a question. Then it decides to *call an API tool* to get an aswer and decides.
- **Stateful vs Stateless**: simple call are stateless. Agents are statefull.

## What is an LLM?

- **What is an LLM?**: It is a statistical model trained on billions on text examples to predict the next word in a sequence.
- **LLMs role in an agent**: they are the brain, as they read the user's requests and the agent's memory, then decides what action to take next.
- **LLMs can not act alone**: It only produce text. It can't call APIs, search the web, or remember users without additional code wrapping around it.

## What is a token?

It is a small piece of text that LLMs use to process language.

- **Token definition**: when a text is sent to an LLM, the model breaks it into tokens. It can be a word, part of a word, or a punctuation mark.
- **Why token matter?**: LLM charge by the token.
- **Tokens in agent conversations**: agents track token usage across multi-step conversations. Long histories cost more tokens and may exceed model limits.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569027#overview
