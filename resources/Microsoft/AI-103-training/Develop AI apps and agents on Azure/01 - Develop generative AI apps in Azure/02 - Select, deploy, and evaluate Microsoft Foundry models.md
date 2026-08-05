# Select, deploy, and evaluate Microsoft Foundry models

Source: [MSFT - Select, deploy, and evaluate Microsoft Foundry models][MSFT-Select-deploy-evaluate-MSFT-Foundry-models].

## Introduction

Building effective generative AI applications requires selecting the right foundation model for your specific use case. With thousands of models available, you need a structured approach to discover, compare, deploy, and validate that a model meets your requirements.

## Explore the model catalog

The Foundry Models catalog serves as your central hub for discovering and comparing AI models.

The model catalog includes two broad categories of model:

- **Foundry Models sold directly by Azure**
  - These models are billed directly through your Azure subscription, and include Azure OpenAI models as well as models from Microsoft and other providers.
- **Foundry Models from partners and community**
  - These models are provided by trusted partners and the community; each with their own licensing and pricing.

**Finding models in the model catalog**:

The model catalog user interface in the Foundry Portal provides an easy way to search for the right model for your needs. Each model has a model card showing its key information; including the provider, capabilities, benchmark metrics, responsible AI considerations, and deployment options.

It is possible to search for models by keyword, and use filters based on the following attributes:

- **Collection**: Models are organized into collections, such as models that are provided directly in Azure, or models in the Hugging Face repository.
- **Capabilities**: Specific model abilities, including reasoning (complex problem-solving), tool calling (API and function integration), or multimodal processing (text, images, audio).
- **Source**: The model provider, including Azure OpenAI, Microsoft, Cohere, Mistral, Meta, Anthropic, and others.
- **Inference tasks**: Specific tasks like text generation, summarization, translation, image-generation, speech synthesis, or other common AI tasks.
- **Fine-tuning methods**: Supported techniques for fine-tuning a model.
- **Industry**: Models trained on industry-specific datasets. These specialized models often outperform general-purpose models in their respective domains.

**Chat completion and reasoning models**:

Most language models in the catalog are *chat completion* models designed to generate coherent, contextually appropriate text responses. These models power conversational interfaces and content generation applications.

For scenarios requiring higher performance in complex tasks like mathematics, coding, science, strategy, and logistics, *reasoning models* like Claude Opus 4.6 provide enhanced problem-solving capabilities. These models can break down complex problems and show their reasoning process.

**Specialized models**:

The catalog also includes task-specific models:

- **Embedding models** like Ada and Cohere convert text into numerical representations. These models enable semantic search, recommendation systems, and Retrieval Augmented Generation (RAG) scenarios where you need to find relevant information based on meaning rather than exact keyword matches.
- **Image generation models** like GPT-image-1 create images from text descriptions. Use these for generating marketing materials, illustrations, or design mockups.
- **Video generation models** like Sora 2 create video content from text descriptions.
- **Image analysis models** like GPT-4.1 can accept multimodal input, including text and images; and generate natural language output based on prompts that include images for analysis.
- **Text to speech models** like GPT-4o-tts can convert text-based input to synthesized speech.
- **Speech to text models** like GPT-4o-transcribe can convert audio data containing speech into text transcriptions.

**Regional and domain-specific models**:

Some models are optimized for specific languages, regions, or industries. When you need specialized performance in a particular domain or language, these models often outperform general-purpose alternatives. Examples include models trained on medical literature, legal documents, or specific language corpora.

## Select models using benchmarks

**Quality benchmarks**:

Quality benchmarks assess how well a model generates accurate, coherent, and contextually appropriate responses. These metrics use public datasets and standardized evaluation methods to ensure consistency.

The Quality index provides a high-level overview by averaging accuracy scores across multiple benchmark datasets that measure reasoning, knowledge, question answering, mathematical capabilities, and coding skills. Higher quality index values indicate stronger overall performance across general-purpose language tasks.

Quality benchmarks use datasets such as:

- **Arena-Hard**: adversarial question answering
- **BIG-Bench Hard**: reasoning capabilities
- **GPQA**: graduate-level multi-discipline questions
- **HumanEval+ and MBPP+**: code generation tasks
- **MATH**: mathematical reasoning
- **MMLU-Pro**: general knowledge assessment
- **IFEval**: instruction following

## Safety benchmarks

Safety metrics ensure models don't generate harmful, biased, or inappropriate content.

**Harmful behavior detection** uses the *HarmBench* benchmark to measure how well models *resist generating unsafe content*. The evaluation calculates Attack Success Rate (ASR), where *lower values indicate safer, more robust models*. HarmBench tests three functional areas:

- **Standard harmful behaviors**: cybercrime, illegal activities, general harm.
- **Contextually harmful behaviors**: misinformation, harassment, bullying
- **Copyright violations**: reproducing copyrighted material

**Toxic content detection** uses the *ToxiGen* dataset to measure how well models identify adversarial and implicit hate speech. *Higher F1 scores indicate better detection performance* across references to minority groups.

**Sensitive domain knowledge** uses the *WMDP* (Weapons of Mass Destruction Proxy) benchmark to measure model knowledge in biosecurity, cybersecurity, and chemical security. *Higher WMDP scores indicate more knowledge of potentially dangerous capabilities*.

## Cost benchmarks

Cost benchmarks in Microsoft Foundry display pricing for serverless API deployments and Azure OpenAI models.

**Cost per input tokens** shows the price for processing 1 million input tokens (the text you send to the model).

**Cost per output tokens** indicates the price for generating 1 million output tokens (the text the model produces).

**Estimated cost** combines input and output costs using a typical *3:1 ratio* (three input tokens for every output token), giving you a single number for comparison. Lower values indicate more cost-effective models.

## Performance benchmarks

Performance metrics measure how quickly and efficiently models respond to requests. These benchmarks matter for real-time applications where user experience depends on responsiveness.

**Latency** measurements include:

- Latency mean - average time in seconds to process a request
- Latency P50 (median) - 50% of requests complete faster than this time
- Latency P90 - 90% of requests complete faster than this time
- Latency P95 - 95% of requests complete faster than this time
- Latency P99 - 99% of requests complete faster than this time
- Time to first token (TTFT) - time until the first token arrives when using streaming

**Throughput** measurements include:

- Generated tokens per second (GTPS) - output tokens generated per second
- Total tokens per second (TTPS) - combined input and output tokens processed per second
- Time between tokens - interval between receiving consecutive tokens

The leaderboard summarizes performance using *mean time to first token* (lower is better) and mean generated tokens per second (higher is better).

**High-throughput**, **low-latency** models provide better user experiences in interactive applications. For batch processing jobs where speed matters less than cost, you can prioritize other factors.

## Deploy models to endpoints

**Understand deployment types**:

Microsoft Foundry supports several deployment types, each offering different characteristics for data residency, scaling, and billing:

- **Global Standard** model deployments can use any Azure region on a pay-per-token basis. They're best for general workloads, and provide the highest quota.
- **Global Provisioned** deployments can use any Azure region, and their use is based on a reserved provision throughput units(PTU) basis to provide predictable high-throughput.
- **Global Batch** deployments can use any Azure region at a 50% discount for large asynchronous jobs within 24-hours.
- **Data Zone Standard** deployments ensure data stays within a specific data zone on a pay-per-token basis. They're best for scenarios where EU/US data zone compliance is required.
- **Data Zone Provisioned** deployments provide predictable throughput based on reserved PTUs within a data zone.
- **Data Zone Batch** deployments are designed for large asynchronous batch jobs within a data zone/
- **Standard deployments** are deployed within a single region on a pay-per-token basis. They're great when you need regional data residency compliance or for low-volume scenarios.
- **Regional Provisioned deployments** provide reserved PTUs within a single region.
- **Developer** Developer deployments use any Azure region on a pay-per-token basis and are for fine-tuned model evaluation only.

## Access models programmatically

When you're ready to integrate the model into your application, you need three key pieces of information from the deployment details:

- **Endpoint URL**: The API endpoint where your application sends requests. Microsoft Foundry supports project endpoints for Foundry-specific functionality, and OpenAI v1 endpoints for broad compatibility with OpenAI model APIs.

- **Authentication key**: The secret key or token your application presents to authenticate requests. Alternatively, you can use Microsoft Entra ID authentication and have your application present an authentication token based on is identity. Entra ID authentication is recommended for production scenarios.

- **Deployment name**: The name you specified during deployment, used in the model parameter of API requests to route to your specific deployment.

## Evaluate model performance

**Why evaluate models**:

Evaluation serves several critical purposes in generative AI application development:

- **Quality assurance** identifies issues and ensures your model provides accurate, relevant responses. Discovering problems during evaluation rather than production protects your users and your organization's reputation.
- **User satisfaction** improves when models consistently deliver helpful, appropriate responses. Evaluation helps you understand how users experience your application and where improvements make the biggest impact.
- **Continuous improvement** comes from analyzing evaluation results to identify enhancement opportunities. Regular evaluation as you update prompts, add features, or retrain models ensures ongoing quality.
- **Compliance and safety verification** confirms your model adheres to policies, avoids generating harmful content, and respects user privacy and data protection requirements.

**Automated evaluation metrics**:

Automated evaluation uses standard metrics to assess your model's outputs automatically. These evaluations scale efficiently and provide consistent, objective measurements.

The Microsoft Foundry portal supports several categories of evaluation metrics, including:

**Generation quality metrics** evaluate overall response quality:

- **Groundedness**: Determines whether responses are based on provided context rather than speculation. Groundedness Pro offers binary assessment (grounded or not grounded) useful for factual accuracy requirements.
- **Relevance**: Measures whether responses address the user's question or request appropriately.
- **Coherence**: Assesses whether responses flow logically and maintain consistent ideas.
**- Fluency: Evaluates linguistic correctness and natural language quality.
**
**Risk and safety metrics** identify potential harmful content:

- **Self-harm content**: Detects responses discussing or encouraging self-harm
- **Hateful and unfair content**: Identifies bias, discrimination, or hateful statements
- **Violent content**: Flags responses containing or promoting violence
- **Sexual content**: Detects inappropriate sexual content
- **Protected material**: Identifies potential copyright or proprietary content reproduction
- **Indirect attack (jailbreak)**: Assesses vulnerability to manipulation attempts

For content harm metrics, results aggregate as defect rate—the percentage of responses exceeding a severity threshold (typically Medium). For protected material and indirect attack, defect rate calculates as `(true instances / total instances) × 100`.

**Natural language processing metrics**:

NLP metrics provide mathematical-based evaluation without requiring an evaluator model. These metrics often need ground truth data—expected or correct responses for comparison.

- **F1-score** measures the ratio of shared words between generated and ground truth answers, balancing precision (avoiding incorrect words) and recall (including important words). F1-score is valuable for tasks like text classification and information retrieval.
- **BLEU** (Bilingual Evaluation Understudy) compares n-grams (word sequences) between generated and reference texts, commonly used for machine translation evaluation.
- **METEOR** (Metric for Evaluation of Translation with Explicit Ordering) extends BLEU by accounting for synonyms, stemming, and paraphrasing, providing more flexible comparison.
- **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation) emphasizes *recall over precision*, making it particularly useful for summarization tasks where covering key points matters more than avoiding extra words.
- **GLEU** (Google-BLEU) is a variant of BLEU designed for sentence-level evaluation.

NLP metrics work well when you have definitive correct answers or reference texts. They're less suitable for open-ended generation where many valid responses exist.

<!-- List of resources-->
[MSFT-Select-deploy-evaluate-MSFT-Foundry-models]:https://learn.microsoft.com/en-us/training/modules/model-catalog-evaluate/
