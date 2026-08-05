# Plan and prepare to develop AI solutions on Azure

Source: [MSFT - Prepare Azure AI Development][MSFT-Prepare-Azure-AI-development].

## Introduction

This module explores some of the key considerations for planning an AI development project, and introduces Microsoft Foundry; a comprehensive platform for AI development on Microsoft Azure.

## What is AI?

The term "Artificial Intelligence" (AI) covers a wide range of software capabilities that enable applications to exhibit human-like behavior.

In today's technological landscape, AI solutions are built on machine learning models that encapsulate semantic relationships found in huge quantities of data; enabling applications to appear to interpret input in various formats, reason over the input data, and generate appropriate responses and predictions.

Common AI capabilities that developers can integrate into a software application include:

| **Capability** | Description |
| --- | --- |
| *Generative AI and agents* | Generative AI is based on large language models (LLMs) with the ability to generate original responses to natural language prompts. <br> Increasingly, generative AI is used as the foundation for agentic AI solutions in which AI agents combine LLMs with focused instructions that define task responsibilities, and tools that the agent can use to find relevant knowledge and automate the tasks for which it is responsible. |
| *Natural language processing* | Modern LLMs evolved from a well-established area within AI called natural language processing (NLP). <br> NLP makes use of statistical and semantic models to make sense of language in documents, emails, social media messages, and other sources of text. <br> While many common NLP tasks can now be performed by generative AI LLMs, there are some specialized uses of NLP - particularly within the realm of text analysis that can benefit from statistical NLP techniques built on term-frequency algorithms, and task-specific models for text classification, sentiment analysis, and summarization. |
| *Computer Speech* | The ability to recognize and synthesize speech enables AI apps and agents to engage more naturally with users through voice input and spoken responses. <br> Computer speech is another well-established area of AI, and recent advances enable it to handle complex conversational interactions; handling background noise, interruptions, and multiple languages and accents. <br> Beyond interactive conversational solutions, computer speech is an integral component of AI solutions for transcription and analysis of live or recorded speech, and the synthesis of speech from text for simultaneous translation or "read aloud" interfaces. |
| *Computer vision* | Computer vision refers to the ability of AI applications and agents to accept, interpret, and process visual input from images, videos, and live camera streams. <br> Increasingly, generative AI models are multimodal, and can not only process visual input, but also generate visual output in the form of images and videos. |
| *Information extraction* | The ability to combine generative AI models for language reasoning, natural language techniques for document understanding, and computer vision and speech for media analysis enables the development of AI solutions that can extract key information from documents, forms, images, recordings, and other kinds of content. |

## Microsoft Foundry

It is a platform for AI development on MSFT Azure. It is useful for project organization, resource management, and for its AI development capabilities.

In Foundry, the resource connectionsa, data, code, and other elements of the AI solution are managed through a *project*. Each project belongs to a single MSFT Foundry *resource* in Azure, which provides compute, data storage, AI tools, and other services.

A Foundry *resource* can have one or more projects, with one of them being the *default* one.

Projects allow to manage assets needed for an AI solution, including:

- **Models**: LLM deployments based on models available in MSFT Foundry. We can interact with them by using the project *endpoint* (using Foundry-specific APIs and SDKs) and the Azure OpenAI endpoint (using OpenAI APIs and SDKs).
- **Agents**: Named AI configurations that encapsulate an LLM, intructions, and tools to define an autonomous AI entity that can automate tasks and collaborate with users and other agents. Agents in Foundry are developed and consumed using the *Microsoft Foundry Agent service* through the project endpoint.
- **Tools**: The tools used by agents can be based on built-in functionality, such as web search or a code interpreter, or connections to custom and third-party tools through Model Context Protocol (MCP) connections. Additionally, Microsoft Foundry Tools includes a suite of AI services for common tasks such as text analysis, speech recognition and synthesis, translation, and content understanding that you can use in your Foundry-based AI solutions. Foundry Tools are hosted in the Foundry resource associated with your project(s).
- **Knowledge**: Agents can use tools to connect to knowledge stores, and use the data they contain to contextualize prompts. To simplify integration with multiple sources of knowledge, you can use *Foundry IQ* in a project to create a single, central MCP-based knowledge connection.

## Foundry Tools

Microsoft Foundry includes *Foundry Tools*; a set of *out-of-the-box* prebuilt APIs and models that can be integrated into different applications. With these tools is possible to create a more cost-effective and predictable solution than relying on generative AI based agents alone.

| Tool | Description |
| --- | --- |
| **Azure Language** | It provides models and APIs that can be used to analyze natural language text and perform tasks such as entity extraction, sentiment analysis, and summarization. It also provides functionality to help building conversational language models and question answering solutions. |
| **Azure Speech** | It provides APIs that can be used to implement text to speech and speech to text transformation, as well as real-time live speech for conversational apps and agents. |
| **Azure Translator** | It uses state-of-the-art language models to translate text between a large number of languages. |
| **Azure Document Intelligence** | It allows using pre-built or custom models to extract fields from complex documents such as invoices, receipts, and forms. |
| **Azure Content Understanding** | It provides multi-modal content analysis capabilities that enables to build models to extract data from forms and documents, images, videos, and audio streams. |

To use Foundry Tools, it is necessary to create a client applications that connect to the *tool-specific endpoint* in the Microsoft Foundry resource, specifying the *project authentication key or using token-based authentication*. We can then use the tool-specific APIs and SDKs to use the provided functionality.

Some tools provide a user interface for configuration and test in the Foundry portal.

## Developer tools and SDKs

**Programming languages, APIs, and SDKs**:

- [MSFT Foundry SDK][MSFT-Foundry-SDK]: enables to write code to connect to Microsoft Foundry projects and access Foundry-specific assets, like agents and Foundry IQ knowledge stores.
- [OpenAI API][OpenAI-API], which enables to use OpenAI SDKs to build chat applications based on Foundry models that support OpenAI syntax.
- [Foundry Tools SDKs][Foundry-tools-SDK], which are AI service-specific libraries for multiple programming languages and frameworks that enable to consume Foundry Tools resources in a specific subscription. We can also use Foundry Tools through their REST APIs.

## Responsible AI

- **Fairness**
  - AI systems should treat all people fairly.
  - Fairness of machine learned systems is a highly active area of ongoing research, and some software solutions exist for evaluating, quantifying, and mitigating unfairness in machine learned models.
  - However, tooling alone isn't sufficient to ensure fairness. Consider fairness from the beginning of the application development process; carefully reviewing training data to ensure it's representative of all potentially affected subjects, and evaluating predictive performance for subsections of your user population throughout the development lifecycle.
- **Reliability and safety**
  - AI systems should perform reliably and safely.
  - AI-based software application development must be subjected to rigorous testing and deployment management processes to ensure that they work as expected before release.
  - Additionally, software engineers need to take into account the probabilistic nature of machine learning models, and apply appropriate thresholds when evaluating confidence scores for predictions.
- **Privacy and security**
  - AI systems should be secure and respect privacy.
  - The machine learning models on which AI systems are based rely on large volumes of data, which may contain personal details that must be kept private.
  - Even after models are trained and the system is in production, they use new data to make predictions or take action that may be subject to privacy or security concerns; so appropriate safeguards to protect data and customer content must be implemented.
- **Inclusiveness**
  - AI systems should empower everyone and engage people.
  - AI should bring benefits to all parts of society, regardless of physical ability, gender, sexual orientation, ethnicity, or other factors.
  - One way to optimize for inclusiveness is to ensure that the design, development, and testing of your application includes input from as diverse a group of people as possible.
- **Transparency**
  - AI systems should be understandable.
  - Users should be made fully aware of the purpose of the system, how it works, and what limitations may be expected.
  - When an AI application relies on personal data, such as a facial recognition system that takes images of people to recognize them; you should make it clear to the user how their data is used and retained, and who has access to it.
- **Accountability**
  - People should be accountable for AI systems.
  - Although many AI systems seem to operate autonomously, ultimately it's the responsibility of the developers who trained and validated the models they use, and defined the logic that bases decisions on model predictions to ensure that the overall system meets responsibility requirements.
  - To help meet this goal, designers and developers of AI-based solution should work within a framework of governance and organizational principles that ensure the solution meets responsible and legal standards that are clearly defined.

<!-- List of resources-->
[MSFT-Prepare-Azure-AI-development]: https://learn.microsoft.com/en-us/training/modules/prepare-azure-ai-development/
[MSFT-Foundry-SDK]: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
[OpenAI-API]: https://learn.microsoft.com/en-us/azure/foundry/openai/latest
[Foundry-tools-SDK]: https://learn.microsoft.com/en-us/azure/ai-services/reference/sdk-package-resources
