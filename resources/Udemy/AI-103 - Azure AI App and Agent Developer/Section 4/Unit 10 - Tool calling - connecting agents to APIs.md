# Tool calling - connecting agents to APIs

Source: [Unit 10 - Tool calling - connecting agents to APIs][src-udemy]

## What is a tool?

- **Tool**: a tool is any external capability an agent can use, like a search API, a database query, an email sender, etc.
- **Why do they matter?**: Without tools, agents can only talk. With tools, agents can take real actions in the world (like send an email, update databases, etc.).

## How tool calling works? The three step flow

- **Step 1 - Agent requests**: The LLM outputs a JSON structure like `{"tool": "send_email". "parameters": {"to": "user@example.com". "body": "..." }}`.
- **Step 2 - code execution**: the orchestration code parses the JSON, looks up the tool function, and executes it with the provided parameters.
- **Step 3 - result returns**: The code sends the tool's result back to the LLM as a new message. The LLM continues reasoning based on the result.

## Tool definition - telling the agent what tools exist

- **Tool definition structure**: each tool has a `name` (unique identifier), `description` (what it does), and `parameter` (JSON schem of required inputs).
- **Including tools in requests**: when we call the LLM, we pass an array of tool definitions in the `tool` parameter. The LLM chooses which tool to call.

## Rest API tool - Calling external HTTP endpoints

- **Rest tool**: the tool definition includes the HTTP method (GET, POST), URL endpoint, headers (like API keys), and expected response format.
- **Authentication**: Most REST tools require an API key in the `Authorization` header. Your code injects the key from configuration when exectuing the tool.

## Codign pattern - Constructing a REST API Tool call

- **Build the request**: Extract `url`, `method`, `headers`, and `body` from the tool call parameters. See `headers["Authorization"] = f"Bearer {api_key}"`.
- **Send the request**: Use `request.post(url, headers=headers, json=body)` for POST or `request.get(url, headers=headers, params=body)` for GET.
- **Handle response**: Check `response.status_code`. If 200 (success), parse `response.json()`. If error (4xx, 5xx), return error message to agent.

## Managing API keys for tools

- **Per-tool key storage**: Create a dictionary mapping tool names to their API keys: `tool_keys = { "slack": "asdasd-123", "weather": "asdas-1234", "database": "gfd-1231" }`.
- **Key injection at runtime**: When executing a tool, look up its key: `api_key = tool_keys.get(tool_name)`. Add to headers before sending the request.
- **Key rotation**: Update keys in configuration (environment variables or Key Vault). Your code reads current values at startup or dynamically on each call.

## Tool response - returning results to the agent

- **Result message structure**: `{ "role": "tool", "tool_call_id": "call_123", "content": "The weather in Seattle is..." }`.
- **Tool call ID matching**: the agent's original tool call has a `tool_call_id`. The result message must include the same ID so the LLM knows which call this result belongs to.
- **Error handling**: If the tool fails, return an error message: `{ "role": "tool", "tool_call_id": "call_123", "content": "Error: API key invalid." }`.

## Parsing tool calls from LLM responses

- **Response structure**: `response["choices"][0]["message"]["tool_calls"]` is an array. Each element has `function.name` and `function.arguments` (JSON string).
- **Extracting parameters**: Parse `arguments` string with `json.loads()` to get a Python dictionary of parameter names and values.
- **Handling multiple calls**: An agent can request multiple tools in one response. Loop through `tool_calls` array and execute each sequentially or concurrently.

## Tool choice - Forcing or allowing tool use

- **`tool_choice: "auto"`**: Default. The agent decides whether to call a tool. Use when the agent should have autonomy.
- **`tool_choice: "required"`**: The agent must call one of the provided tools. Use when the user's request always requires an action.
- **`tool_choice: { "type": "function", "function": {"name": "get_weather"}}`**: Forces the agent to call a specific tool. Use for deterministic workflows.

## Parallel tool calls - Executing multiple tools at once

- **Parallel execution**: use `asyncio.gather()` to execute multiple tool calls simultaneously. Results arrive in any order.
- **When to use parallel**: when they are independent (no data dependency).
- **Sequential feedback**: If tools have dependencies (tool B needs tool A's result), execute sequentially. The agent's second tool call can reference the first result.

## Idempotent tools - Handling duplicate calls

- **Implement idempotency**: include a `request_id` parameter. The tool checks if it already processed that `request_id` and returns cached result without repeating the action.

## Tool timeouts and retries

- **Setting timeouts**: use `requests.get(url, timeout=10)` - fails after 10 seconds. Choose timeout based on expected tool latency (fast tools 5s; slow tools 30s).
- **Retry strategy**: for network errors (timeout, connection refused), retry up to 3 times with exponential backoff (1s, 2s, 4s between retries).
- **Circuit breaker**: if a tool fails 5 times in 1 minute, stop calling it for 30 seconds. Prevents cascading failures when a downstream service is down.

## Tool call history - Maintaining context

- **Storing in conversation history**: every tool call (request) and tool response (result) is stored in the conversation messages array as separate entries.
- **Preventing repeated calls**: before calling a tool, check if the same tool with same parameters was called in the last 10 messages. If so, reuse cached results.
- **Referencing past results**: the agent's system instruction can say "Remember past tool results. If asked again, answer from memory without recalling the tool".

## Logging tool calls with Foundry trace

- **Trace span per tool call**: create a span (logged operation) for each tool call. Add attributes: `tool_name`, `parameters_json`, `status_code`, `duration_ms`.
- **Recording results**: add `tool_result` attribute to the span. If error, add `error_type` and `error_message` attributes.
- **Debugging failures**: in Foundry trace, filter spans by `error_type` to find all failed tool calls. Examine parameters to see what caused the failure.

## Security - Validating tool parameters

- **parameter validation**: check that `email_address` matches regex. Check that `amount` is a positive number within allowed range.
- **SQL injection prevention**: if tool queries a database, use parameterized queries, not string concatenation: `cursor.execute("SELECT * FROM users WHERE ID =?", (user_id,))`.
- **URL injection**: if tool calls a URL from a parameter, validate it belongs to an allowed domain list. Block URLs that try to call internal services.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57569051#overview
