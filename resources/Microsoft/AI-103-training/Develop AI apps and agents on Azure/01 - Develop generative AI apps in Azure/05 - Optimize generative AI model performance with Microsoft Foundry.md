# Optimize generative AI model performance with Microsoft Foundry

Source: [Optimize generative AI model performance with Microsoft Foundry][optimize-gen-AI-model-performance-with-foundry]

## Introduction

There are several complementary strategies you can use to optimize a generative AI model's performance. These strategies range from quick, low-cost adjustments to more involved techniques that require additional time and resources.

![strategies](./images/05%20-%20introduction%20-%20strategies.png)

## Optimize model output with prompt engineering

The most accessible way to optimize a model's performance is through prompt engineering. Prompt engineering is the process of designing and refining prompts to improve the quality, accuracy, and relevance of the responses a language model generates. It requires no additional infrastructure or training data, and you can start experimenting immediately.

**Understand prompt components**:

Prompts for chat completion models typically include the following components:

- **System message**: Instructions that define the model's behavior, role, and constraints.
- **User message**: The question or input from the user.
- **Assistant message**: Previous model responses, used in multi-turn conversations.
- **Examples**: Sample input/output pairs that demonstrate the expected response format.

**Design effective system messages**:

A system message is a set of instructions you provide to the model to guide its responses. System messages typically appear first in the conversation and act as the highest-level set of instructions. You use them to:

- Define the assistant's role and boundaries.
- Set the tone and communication style.
- Specify output formats, such as JSON or bullet points.
- Add safety and quality constraints for your scenario.

When designing a system message, follow this checklist:

1. **Start with the assistant's role**: State the role and the expected outcome for a typical request.
1. **Define boundaries**: List the topics, actions, and content types the assistant should avoid.
1. **Specify the output format**: If you need a specific format, state it plainly and keep it consistent.
1. **Add a "when unsure" policy**: Tell the model what to do when the user's request is ambiguous, out of scope, or when the model lacks information.

**Configure model parameters**:

Beyond the text of your prompts, you can adjust model parameters that control how the model generates responses:

- **Temperature**: Controls the randomness of the output. A higher value (for example, 0.7) produces more creative and varied responses, while a lower value (for example, 0.2) produces more focused and deterministic responses. Use lower values for factual tasks and higher values for creative ones.
- **Top_p**: Also controls randomness, but in a different way. It limits the model to a subset of the most probable next tokens. For example, a top_p of 0.9 means the model considers only the top 90% of probable tokens.

**When prompt engineering is enough**:

Prompt engineering is the right starting point for any model optimization effort. It's effective when you need to:

- Guide the model's tone, format, and behavior.
- Provide specific instructions for a task.
- Quickly iterate on results without infrastructure changes.
- Keep costs low, as no additional training or data storage is required.

## Ground your model with Retrieval Augmented Generation

**Understand grounding**:

When you use a language model without grounding, the only information it has comes from its training data. The result might be grammatically correct and logically structured, but it can be inaccurate or include fabricated details.

When you ground a prompt, you provide relevant data from a trusted source along with the user's question. The model then generates a response based on that data, producing more accurate and contextually relevant answers.

**How RAG works?**:

![rag](./images/05%20-%20rag%20-%20how%20rag%20works.png)

RAG is a pattern that retrieves relevant information from a data source and includes it in the prompt before the model generates a response. The process follows three steps:

1. **Retrieve**: Search a data source for information that is relevant to the user's question.
1. **Augment**: Add the retrieved information to the prompt as context.
1. **Generate**: Send the augmented prompt to the language model to generate a grounded response.

**Create embeddings for search**:

A critical component of RAG is the ability to efficiently find the most relevant information in your data source. This is where embeddings and vector search come in.

An embedding is a mathematical representation of text as a vector — a list of floating-point numbers that captures the meaning of words, sentences, or documents. You create embeddings by sending your content to an embedding model, such as an Azure OpenAI embedding model available in Microsoft Foundry.

![embeddings](./images/05%20-%20rag%20-%20create%20embeddings%20for%20search.png)

> [!NOTE]
> **Cosine similarity** measures how close two vectors are by calculating the angle between them. A value near 1 means the vectors are very similar. This mathematical approach enables you to find relevant documents even when the exact words don't match.

**Use Azure AI Search for retrieval**:

Azure AI Search provides the retrieval component for RAG solutions in Microsoft Foundry. It allows you to bring your own data, create a searchable index, and query it to retrieve relevant information.

![az-ai-search](./images/05%20-%20rag%20-%20Azure%20AI%20search.png)

To use Azure AI Search with RAG, you:

1. **Add your data** to Microsoft Foundry from sources like Azure Blob Storage, Azure Data Lake Storage Gen2, or Microsoft OneLake. You can also upload files directly.
1. **Create an index** using an embedding model to generate vector representations of your content. The index is stored in Azure AI Search.
1. **Query the index** when a user asks a question. The system converts the question to an embedding, searches for the most similar content, and returns the relevant results.

Azure AI Search supports several search techniques:

- **Keyword search**: Matches exact terms in the query to text in the index.
- **Semantic search**: Uses semantic models to match the meaning of the query rather than exact keywords.
- **Vector search**: Uses embeddings to find semantically similar content.
- **Hybrid search**: Combines keyword, semantic, and vector search for the most accurate results. Hybrid search is recommended for generative AI applications.

**Code example**:

```python
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

project = AIProjectClient(
    endpoint=os.environ["PROJECT_ENDPOINT"],
    credential=DefaultAzureCredential(),
)

client = project.get_openai_client()

response = client.responses.create(
    model="gpt-4o",
    input=[
        {"role": "system", "content": "You are a helpful travel advisor. "
         "Use the following hotel data to answer: " + retrieved_context},
        {"role": "user", "content": "Which hotels do you offer in Paris?"},
    ],
)

print(response.output_text)
```

> [!IMPORTANT]
> `retrieved_context` represents the documents returned from your Azure AI Search index.

**When to use RAG?**:

- **The model needs domain-specific knowledge**: Your organization has private data that the model wasn't trained on, like a product catalog, policy documents, or internal knowledge base.
- **Information changes frequently**: Your data is updated regularly, such as inventory, pricing, or news. RAG retrieves current data at query time without retraining.
- **Factual accuracy is critical**: You need responses grounded in real data rather than the model's general knowledge.
- **The base model's training data has a cutoff**: Events or information that occurred after the model's training cutoff date need to be accessible.

## Fine-tune a model for consistent behavior

**Fine-tuning** is the process of taking a pretrained language model and further training it on a smaller, task-specific dataset. This adjusts the model's internal weights so that it produces responses that are consistent with the patterns in your training data.

**Understand fine-tuning**:

Fine-tuning uses **LoRA (Low-Rank Adaptation)**, a technique that approximates weight changes with a lower-rank representation. Instead of retraining all of the model's parameters, LoRA updates only a smaller subset of important parameters. This makes training faster and more cost-effective while maintaining model quality.

The key benefit of fine-tuning over training a model from scratch is efficiency. You need less time, fewer computing resources, and significantly less data to customize a model's behavior.

**Know when to fine-tune**:

Fine-tuning is suited for scenarios where prompt engineering alone doesn't achieve the consistency you need. Common use cases include:

- **Consistent style and tone**: Your organization has a specific brand voice, and the model needs to follow it reliably across all interactions. For example, the travel agency wants every response to use a warm, encouraging tone with short paragraphs.
- **Specific output formats**: You need the model to reliably produce structured output, like JSON responses following a defined schema, and few-shot examples alone aren't sufficient.
- **Reducing prompt length**: Long system messages with many examples consume tokens and increase latency. Fine-tuning embeds those patterns into the model, reducing the prompt size needed for each request.
- **Distillation**: You want to transfer the capabilities of a large, expensive model to a smaller, more efficient one. For example, you can collect outputs from a high-performing model and use them to fine-tune a smaller model that achieves similar quality at lower cost and latency.
- **Enhancing tool usage**: When your application uses tool calling, fine-tuning with tool examples can improve the accuracy of tool selection and parameter generation.

**Explore types of fine-tuning**:

Microsoft Foundry offers several fine-tuning techniques:

- **Supervised fine-tuning (SFT)**: Train the model on a labeled dataset of prompt-and-response pairs. The model learns to produce outputs that match the patterns in your training data. This technique works best when there are clear, well-defined ways to approach a task.
- **Reinforcement fine-tuning (RFT)**: Optimize the model's behavior through iterative feedback, using a grader to reward better responses incrementally. RFT works well for complex or dynamic tasks where there are many possible solutions and you want to improve the model's reasoning quality.
- **Direct Preference Optimization (DPO)**: Align the model based on human preferences by providing preferred and non-preferred response pairs. DPO is computationally lighter than traditional reinforcement learning approaches while being equally effective at alignment.

## Compare and combine optimization strategies

**Understand the optimization spectrum**:

![strategies](./images/05%20-%20introduction%20-%20strategies.png)

- **Optimize for context**: When the model lacks d*omain-specific knowledge* and you want to *maximize the accuracy of responses*. *RAG* addresses this by retrieving relevant data from external sources.
- **Optimize the model**: When you want to improve the response *format*, *style*, or *tone* by *maximizing the consistency of behavior*. *Fine-tuning* addresses this by training the model on examples that demonstrate the desired output.

**Compare strategies**:

| Strategy | Time to implement | Complexity | Cost | Best for |
| --- | --- | --- | --- | --- |
| Prompt engineering | Low | Low | Low (per-token only) | Guiding tone, format, and behavior; quick iteration; providing instructions and examples |
| RAG | Medium | Medium | Medium (search infrastructure + storage + per-token) | Factual accuracy, domain-specific knowledge, dynamic or frequently changing data |
| Fine-tuning | High | High | High (training compute + model hosting + per-token) | Behavioral consistency, style enforcement, reducing prompt length, model distillation |

**Apply a decision framework**:

When deciding which strategies to use, start simple and add complexity only when needed:

1. **Start with prompt engineering**: Test system messages, few-shot examples, and parameter tuning. Evaluate whether the results meet your requirements.
1. **Add RAG if accuracy matters**: If the model needs access to specific, current, or private data to answer correctly, implement RAG with Azure AI Search.
1. **Add fine-tuning if consistency matters**: If the model doesn't reliably maintain the desired style, tone, or format despite detailed prompts, fine-tune the model with representative examples.
1. **Combine as needed**: Layer strategies based on your application's specific requirements. Not every application needs all three.

[optimize-gen-AI-model-performance-with-foundry]: https://learn.microsoft.com/en-us/training/modules/optimize-generative-ai-model-performance/
