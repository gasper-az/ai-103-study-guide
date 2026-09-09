# Model Selecton - LLMs vs SMLs for agent tasks

Source: [Unit 05 - Model Selecton - LLMs vs SMLs for agent tasks][src-udemy]

## What is a LLM?

- **LLM size definition**: an LLM has over 10 billion parameters. GPT-5 has over 1 trillion parameters.
- **LLM capabilities**: LLMs excel at complex reasoning, following nuanced instructions, understanding context, and generating creative responses.
- **LLM cost and speed**: LLMs are expensive to run (higher cost per token) and slower to respond (higher latency in milliseconds) than smaller models.

## What is a SLM?

- **SLM size definition**: an SLM tipically has 1 billion to 7 billion parameters. MSFT Phi-3 mini has 3.8 billion parameters.
- **SLM capabilities**: SLMs excel at specific task like classification, summarization, entity extraction, and simple question answering.
- **SLM cost and speed**: SLMs are cheap to run (lower cost per token) and very fast (low latency in milliseconds), making them ideal for high-volume tasks.

## GPT-5: The new standard for frontier intelligence

- **GPT-5.6 capabilities**: GPT-5.6 can process text, images, and audio simultaneously. It excels at multi-step reasoning, tool calling, and following complex system instructions.
- **GPT-5.6 token limit**: GPT-5.6 supports 1M tokens of context. This allows the agent to remember very long conversations or process large documents.
- **When to use GPT-5.6**: Use GPT-5.6 as the manager agent in multi-agent systems, for complex planning tasks, or when you need the highest accuracy.

## Phi-3 and Phi-4 - MSFT SLM family

Phi-3 and Phi-4 are MSFT SML models, optimized for fast, cheap inference on specific agent tasks like summarization and classification.

- **Phi model sizes**: Phi-3 comes in mini (3.8B parameters), small (7B), and medium (14B). Phi-4 adds improved reasoning in a compact 14B size.
- **Phi training data**: Phi models are trained on high-quality synthetic data and filtered web content, not raw internet dumps. This makes them more efficient.
- **When to use Phi models**: Use Phi models for high-volume classification, sentiment analysis, entity extraction, or summarization where GPT-5 is overkill.

## Deploying a model in Foundry model catalog

- **Deployment steps**: In foundry model catalog, select a model (GPT-5.6, Phi-3). Choose a deployment name, capacity (tokens per minute), and region.
- **Deployment capacity**: Capacity determines how many tokens per minute your agent can process. Higher capacity costs more but handles higher traffic.
- **Deployment lifecycle**: Deployed models can be updated (new version), scaled (increase capacity), or deleted (stop paying). Each deployment has its own endpoint.

## Model capacity and quotas

- **Tokens per minute (TPM) definition**: it is the maximum number of tokens the agent can send and receive in one minute. 10K TPM handles moderate traffic.
- **Regional quotas**: each AZ region has a global quota for each model. You may need to request a quota increase from MSFT for higher-volume agents.
- **Monitoring Usage**: Foundry shows your TPM usage in metrics. If you exceed quota, the model returns a 429 error (too many requests) until the next minute.

## Multi-model agents

- **Routing pattern**: user input first goes to Phi-3 for intent classification. If intent is asimple, Phi-3 responds directly.
- **Escalation pattern**: if intent is complex (e.g.: "plan a dispute resolution strategy"), the agent calls GPT-5.6 for deeper reasoning.
- **Cost savings**: 90% of requests route to cheap Phi-3. Only 10% escalate to expensive GPT-5.6. Total cost drops dramatically.

## System instructions - Model-specific considerations

- **LLM System instructions**: GPT-5.6 handles long, complex instructions with conditional logic.
- **SLM System instructions**: Phi-3 works best with short, direct instructions.
- **Testing required**: Always test system instructions with your chosen model. An instruction that works in GPT-5.6 may fail in Phi-3 due to its smaller size.

## Coding pattern

- **SDK**: Use `from azure.ai.inference import ChatCompletionClient`. Create a client with endpoint and key. Call `client.complete()` with messages list.
- **Response handling**: both sdk and rest return a JSON response. Extract the assistant's message from `choices[0].message.content`.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569037#overview
