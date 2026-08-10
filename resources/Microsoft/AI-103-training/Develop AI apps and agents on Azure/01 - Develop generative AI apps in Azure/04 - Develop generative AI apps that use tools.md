# Develop generative AI apps that use tools

Source: [Develop generative AI apps that use tools][develop-generative-AI-apps-that-use-tools]

## Why tools matter?

Tools bridge the gap between AI reasoning and real-world actions. They enable your generative AI applications to:

- **Access real-time information**: Fetch current data, weather, stock prices, or API responses that weren't in the model's training data.
- **Take actions**: perform tasks like sending emails, creating database records, or triggering workflows based on AI decisions.
- **Ground responses in facts**: Retrieve specific, authoritative information to reduce incorrect information and improve accuracy.
- **Extend functionality**: Connect to your existing systems, databases, and business logic seamlessly.
- **Build intelligent workflows**: Chain multiple operations together so AI coordinates complex, multi-step processes.

## What are tools?

Some of the commonly used tools available in the Responses API, include:

- **code_interpreter**: A Python environment in which the model can generate and run code.
- **web_search**: A tool that enables the model to find general information on the Internet, which allows it to base responses on more current data than it was trained on.
- **file_search**: A tool that enables the model to search specific files that you upload to a dedicated vector search index - enabling it to ground responses in specific knowledge.
- **function**: A tool that enables the model to call custom functions in your application code.

## Use the code_interpreter tool

**What is the code_interpreter tool?**:

It enables generative AI models to write and run Python code dynamically during a conversation.
The model can test its logic, process data, and return actual results from code.

Key features include:

- **Dynamic Python Execution**: The model writes and runs Python code in a sandboxed environment.
- **File Handling**: Upload, process, and download files (CSV, JSON, images, and so on).
- **Data Analysis**: Perform calculations, statistical analysis, and data transformations on the fly.
- **Real-time Feedback**: The model sees code execution results and can iterate or fix errors.
- **Complex Problem Solving**: Tackle math problems, simulations, and logic puzzles through executable code.

**Common use cases**:

| Use case | Example |
| --- | --- |
| **Data Analysis** | Parse a CSV file and generate summary statistics |
| **Math & Physics** | Solve differential equations or simulate physics scenarios |
| **File conversion** | Convert betyween data formats (JSON <-> CSV, and so on) |
| **Prototyping** | Test algorithms and ideas before formal implementation |

**Code example**:

```python
from openai import OpenAI

client = OpenAI(
    base_url={openai_endpoint},
    api_key={auth_key_or_token}
)

# Get response using the code_interpreter tool
response = client.responses.create(
    model={model_deployment},
    instructions="You are an AI assistant that provides information. Use the python tool to run code for math problems.",
    input="What is the square root of 16?",
    tools=[{"type": "code_interpreter",
            "container": {"type": "auto"}}]
)
print(response.output_text)
```

**How the code_interpreter tool works?**:

The general process for using the code_interpreter tool is:

1. **You send a request**: Include code_interpreter in your tools array.
1. **Model analyzes the task**: The model determines if code execution is needed.
1. **Model generates code**: The model writes Python code to accomplish the task.
1. **Code runs**: The code runs in a sandboxed environment with access to common libraries (for example, pandas, numpy, and math).
1. **Results returned**: The model receives the output and incorporates it into its response.

## Use the web_search tool

It gives a generative AI model access to current, external information at runtime. Instead of relying only on training data, the model can issue a search query, review relevant sources, and produce an answer grounded in up-to-date content.

Key features include:

- **Live information retrieval**: Get recent information not available in static model training data.
- "**Source-grounded responses**": Build answers from retrieved web content.
- **Reduced hallucination risk**: Improve reliability by checking external sources.
- **Automatic query generation**: The model decides when and how to search based on user intent.
- **Seamless user experience**: Search and response generation happen in one flow.

**Common use cases**:

| Use case | Example |
| --- | --- |
| **Current Events** | Summarize key updates on a breaking technology announcement |
| **Market research** | Compare recent product features or pricing across vendors |
| **Policy monitoring** | Check whether regulations or guidance have changed |
| **Fact Verification** | Validate claims against reputable public sources |

**Code example**:

```python
from openai import OpenAI

client = OpenAI(
    base_url={openai_endpoint},
    api_key={auth_key_or_token}
)

# Get response using the web_search tool
response = client.responses.create(
    model={model_deployment},
    instructions="You are an AI assistant. Use web search when current information is required.",
    input="What are three major announcements from Microsoft Build this week?",
    tools=[{"type": "web_search"}]
)

print(response.output_text)
```

## Use the file_search tool

It helps a model answer questions using private or domain-specific files, such as policy documents, manuals, contracts, and internal knowledge bases. Instead of relying only on general training data, the model can search indexed file content and return grounded answers.

Key features include:

- **Document-grounded answers**: Responses are based on your uploaded files.
- **Semantic retrieval**: Finds relevant passages by meaning, not only exact keyword matches.
- **Vector store integration**: Search across one or more indexed document collections.
- **Citations and transparency**: Include matched results for debugging and traceability.
- **Better enterprise relevance**: Use organization-specific knowledge in model outputs.

**Common use cases**:

| Use case | Example |
| --- | --- |
| **Policy Q&A** | Answer employee questions from HR policy PDFs |
| **Support Assistants** | Retrieve product steps from internal troubleshooting guides |
| **Legal review** | Locate specific clauses across contract documents |
| **Knowledge discovery** | Summarize answers from technical documentation sets |

**Code example**:

```python
from openai import OpenAI

client = OpenAI(
    base_url={openai_endpoint},
    api_key={auth_key_or_token}
)

# Create vector store and upload a file
vector_store = client.vector_stores.create(name="policy-docs")
client.vector_stores.files.upload_and_poll(
    vector_store_id=vector_store.id,
    file=open("expenses_policy.pdf", "rb")
)

# Get response using the file_search tool
response = client.responses.create(
    model=model_deployment,
    instructions="You are an AI assistant that provides information from HR policy documents.",
    input="What's the maximum amount I can claim for a taxi ride?",
    tools=[{
        "type": "file_search",
        "vector_store_ids": [vector_store.id]
    }],
    include=["file_search_call.results"]
)
print(response.output_text)
```

> [!IMPORTANT]
> The file_search tool is a great way to ground a model in a specific set of documents or data files. However, for enterprise-scale agents that need to access large quantities of data in multiple data stores, you should consider using the Foundry IQ knowledge store solution with a Microsoft Foundry agent.

## Use the function tool

The function tool (function calling) lets a model decide when to call named tools you expose in your application. The model doesn't run your business logic directly. Instead, it returns a structured function call, your code runs the function, and then you pass the function output back to the model.

Key features include:

- **Structured tool calls**: The model emits explicit function-call requests.
- **Developer-controlled execution**: Your application decides how and where functions run.
- **Reliable integration pattern**: Call APIs, internal services, or helper utilities safely.
- **Multi-turn orchestration**: Return tool output and let the model continue reasoning.
- **Grounded responses**: Answers can include live, system-generated data.

**Common use cases**:

| Use case | Example |
| --- | --- |
| **System Integration** | Call an internal API for account or order details |
| **Task Automation** | Trigger workflows like ticket creation or notifications |
| **Data Lookup** | Query business rules or reference tables before answering |

**Code example**:

```python
import time
from openai import OpenAI

# Function to get the current time
def get_time():
    return f"The time is {time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())}"


# Main function
def main():
    client = OpenAI(
        base_url={openai_endpoint},
        api_key={auth_key_or_token}
    )

    function_tools = [
        {
            "type": "function",
            "name": "get_time",
            "description": "Get the current time"
        }
    ]

    # Initialize messages with a system prompt
    messages = [
        {"role": "developer", "content": "You are an AI assistant that provides information."},
    ]

    # Loop until the user types 'quit'
    while True:
        prompt = input("\nEnter a prompt (or type 'quit' to exit)\n")
        if prompt.lower() == "quit":
            break

        # Append the user prompt to the messages
        messages.append({"role": "user", "content": prompt})

        # Get initial response
        response = client.responses.create(
            model=model_deployment,
            input=messages,
            tools=function_tools
        )

        # Append model output to the messages
        messages += response.output

        # Was there a function call?
        for item in response.output:
            if item.type == "function_call" and item.name == "get_time":
                current_time = get_time()
                messages.append({
                    "type": "function_call_output",
                    "call_id": item.call_id,
                    "output": current_time
                })

                # Get a follow up response using the tool output
                response = client.responses.create(
                    model=model_deployment,
                    instructions="Answer only with the tool output.",
                    input=messages,
                    tools=function_tools
                )

        print(response.output_text)


# Run the main function when the script starts
if __name__ == '__main__':
    main()
```

**How the function tool works|?**:

The general process for using the function tool is:

1. **You define tools**: Provide one or more function definitions in the tools array.
1. **Model evaluates the prompt**: It determines whether a function call is needed.
1. **Model emits a function call**: The response includes the function name and call metadata.
1. **Your app runs logic**: Run the matching function in your code.
1. **You return function output**: Send a function_call_output item with the result.
1. **Model completes the answer**: It incorporates tool results into the final response.

[develop-generative-AI-apps-that-use-tools]: https://learn.microsoft.com/en-us/training/modules/use-generative-ai-tools/
