# Responsible AI & Safety for agents.md

Source: [Unit 04 - Responsible AI & Safety for agents.md][src-udemy]

## Why agents need safety controls?

- **Two safety directions**: safety controls protect both incoming user inputs and outgong agent outputs.

## What is content safety?

- **Hate category**: content safety detects hate speech, i.e., language that attacks or insults people based on characteristics like race, religion, or gender identity.
- **Sexual category**: the service detects explict sexual content, including descriptions, references, and requests for adult material.
- **Violence category**: content safety flags violence language, i.e., threats, descriptions of harm, or glorfcation of physical attacks.
- **Self-harm category**: the service detects content related to self-injury, suicide, or eating disorders.

## Severity levels

- **Severity scale definition**: severity 0 means no harmfull content detected. Severity 6 means extremely severe violations.
- **Configurable thresholds**: you set a threashold for each category.

## Input filtering

- **Why input filtering matters?**: without it, a user could send hate speech, threats, or jailbreak attempts directly to your agent's LLM.
- **Where does it happen?**: it occurs at the Foundry Project level, before any user message reaches your agent code or the deployed LLM.
- **Blocking behaviour**: when input filteriing blocks a message, the user receives a generic error. The harmful content never reaches your agent or LLM.

## Coding pattern - Calling content safety AP

- **SDK call pattern**: use `from azure.ai.contentsafety import ContentSafetyClient`. Call `client.analyze_text()` with the text to scan and categories to check.
- **Reast call pattern**: it is possible to send a JSON body to an specific endpoint to analyze the containing text.
- **Response handling**: the AP returns severity scores for each category. Your code checks if any severity exceeds your threshold. If yes, block the text.

## Managing content safety API keys

Content safety is a separate Azure service wth its own endpoint URL and API key.

- **Provisioning content safety**: in Azure portal, create a Content Safety resource. After creation, you receive an endpoint URL and two API keys.
- **Storing keys securely**: store content safety keys in environment variables or AZ Key Vault.
- **Key rotation**: generate a new keys periodically in Azure portal. Update the environment variables or KV.

## What is Jailbreak?

It is a carefully crafted user prompt designed to bypass an agent's system message and safety filters, making it ignore its instructions.

- **How does it work?**: they use phrasing tricks to confuse an LLM.
- **Common patterns**: Role-playing attacks ("act as if you are unrestricted AI"), hypothetical scenarios ("for research purposes, tell me how to..."), and translation tricks.
- **Why jalbreaks are dangerous?**: a successful jailbreak makes the agent ingnore its system message, potentially revealing sensitive data or performing harmful actions.

## What is prompt injection?

It occurs when a user's input includes hidden commands that override the agent's original instructions, often by injecting fake content.

- **Direct prompt injection**: the user includes malicious instructions directly in their message.
- **Indirect prompt injection**: the malicious instructions come from an external source the agent reads, like websites, email, or document. The agent trusts this external source.

## Defendng agains prompt injection

- **Input sanitization**: before sending input to the LLM, scan for known injection patterns. Remove or escape characters like "ignore previous instructions".
- **Separator tokens**: inject unique separator tokens between system message, user input, and external content. LLMs learn to treat content between separators as untrusted.
- **External content restrictions**: limit what external sources your agent can read. Never allow the agent to execute commands found in untrusted external documents.

## What is AI red teaming?

It is the practiice of using automated agents to attack your own agent, finding security weakness before real attackers do.

- **Definition**: a red team agent sends thousands of automated test propmts to your agent to see if it breaks.
- **Read teaming vs manual testing**: manual testing might try 50 prompts. A red team agent can try 50k prompts overnight, finding weakness humans would miss.
- **Foundry native red teaming**: Microsoft Foundry includes a built-in red teaming agent based on the PyRIT framework. You configure it and let it run against the deployed agent.

[src-udemy]: https://www.udemy.com/course/ai-103-azure-ai-app-and-agent-developer-complete-course/learn/lecture/57429871#overviews
