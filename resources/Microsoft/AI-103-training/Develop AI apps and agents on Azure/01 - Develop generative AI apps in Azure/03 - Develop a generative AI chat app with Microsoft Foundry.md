# Develop a generative AI chat app with Microsoft Foundry

Source: [MSFT - Develop a generative AI chat app with Microsoft Foundry][MSFT-develop-gen-ai-chat-with-foundry]

## Explore with the model playground

The Model playground in the Foundry portal provides an interactive environment for testing models before you write any code. You can access it by selecting Model playground from the left navigation.

The playground lets you:

- Send prompts to deployed models and see responses in real time.
- Adjust settings like *temperature* and *max tokens*.
- Add system messages to customize model behavior.
- Experiment with different models and configurations.

**Generating code samples**:

One of the most useful features of the Model playground is the Code button in the chat pane. At any point during your experimentation, you can select this button to see code samples to reproduce a chat session in your app.

The generated code samples include choices for:

- *API*: Using Responses API, or another API like ChatCompletions
- *Language*: Select your preferred programming language
- *SDK*: Choose which SDK you want to see a sample of.

## Choose an endpoint and SDK

Microsoft Foundry provides flexibility for developing generative AI chat applications.

- **Endpoints**: Microsoft Foundry projects provide two endpoints that you can use to connect to and consume project assets, such as model deployments, from client applications. Each project has both a *Project endpoint* and an *Azure OpenAI endpoint*.
- **Client SDK**: Depending on the endpoint you select, you can choose to use the Microsoft Foundry SDK or the OpenAI SDK to develop a generative AI chat application. Both SDKs support an OpenAI API compatible client object that can submit prompts to models, but there are some differences in the specific functionality available in each SDK.
- **Authentication**: Depending on the endpoint and SDK you choose to use, there are multiple ways a client application can be authenticated by Foundry in order to be granted access to assets. In general, production applications should use Microsoft Entra ID authentication, which requires the application to be running in the context of a specific identity; but in some scenarios you can also use key-based or token-based authentication.
- **Chat API**: The OpenAI client API supports two chat APIs: *ChatCompletions* and *Responses*. While the Responses API is recommended for most new development projects, the ChatCompletions API is well-established and compatible across many generative AI models and platforms.

**Using the Foundry SDK with the project endpoint**:

- [Azure AI Projects for Python][MSFT-azure-ai-projects-python]

**Installing the SDK**:

> pip install azure-ai-projects azure-identity openai

**Connecting to the project endpoint**:

Each Foundry project has a unique endpoint that you can find on the project's Overview page in the Foundry portal at https://ai.azure.com.

The project endpoint follows this format:

> https://{resource-name}.services.ai.azure.com/api/projects/<project-name>

**Using the OpenAI SDK with the Azure OpenAI endpoint**:

1. Install the SDK

> pip install openai azure-identity

2. The Azure OpenAI endpoint follows this format:

> https://{resource-name}.openai.azure.com/openai/v1

**Choosing between the Foundry SDK and OpenAI SDK**:

Use the Foundry SDK when your application needs Foundry-specific capabilities:

- **Foundry Agent Service** for building and managing AI agents.
- **Tool invocation and approval** workflows.
- **Cloud evaluations** for testing and validating AI responses.
- **Tracing and observability** for monitoring application behavior.
- **Foundry direct models** (non-Azure OpenAI models available through the model catalog).
- **Project metadata, connections, and governance** features.

Use the OpenAI SDK when you need maximum compatibility with the OpenAI API:

- **Full OpenAI API compatibility** for existing code and tooling.
- **Portability** between OpenAI and Azure OpenAI deployments.
- **Chat Completions, Responses, and Images** APIs.
- **Minimal dependency** on Foundry-specific concepts.

## Generate responses with the Responses API

The OpenAI Responses API brings together capabilities from two previously separate APIs (ChatCompletions and Assistants) in a unified experience. It provides stateful, multi-turn response generation, making it ideal for conversational AI applications.

**Understanding the Responses API**:

The Responses API offers several advantages over traditional chat completions:

- **Stateful conversations**: Maintains conversation context across multiple turns.
- **Unified experience**: Combines chat completions and Assistants API patterns.
- **Foundry direct models**: Works with models hosted directly in Microsoft Foundry, not just Azure OpenAI models.
- **Simple integration**: Access through the OpenAI-compatible client.

**Understanding response structure**:

A response object contains several useful properties:

- **output_text**: The generated text response.
- **id**: Unique identifier for this response.
- **status**: Response status (for example, "completed").
- **usage**: Token usage information (input, output, and total tokens).
- **model**: The model used to generate the response.

**Adding instructions**:

In addition to the user input, you can provide *instructions* (often referred to as a *system prompt*) to guide the model's behavior.

**Controlling response generation**:

You can control response generation with additional parameters:

- **temperature**: Controls randomness (0.0-2.0). Higher values make output more creative and varied.
- **max_output_tokens**: Limits the maximum number of tokens in the response.
- **top_p**: Alternative to temperature for controlling randomness.

## Generate responses with the ChatCompletions API

The OpenAI *ChatCompletions API* is commonly used across generative AI models and platforms. Although the Responses API is recommended for new project development, it's likely that you'll encounter scenarios where the ChatCompletions API is useful for code maintenance or cross-platform compatibility.

**Submitting a prompt**:

The *ChatCompletions API* uses collections of message objects in JSON format to encapsulate prompts.

**Retaining conversational context**:

Unlike the Responses API, the *ChatCompletions API doesn't provide a stateful response tracking feature*. To retain conversational context, you must write code to manually track previous prompts and responses.

[MSFT-develop-gen-ai-chat-with-foundry]: https://learn.microsoft.com/en-us/training/modules/foundry-sdk/
[MSFT-azure-ai-projects-python]: https://pypi.org/project/azure-ai-projects/
