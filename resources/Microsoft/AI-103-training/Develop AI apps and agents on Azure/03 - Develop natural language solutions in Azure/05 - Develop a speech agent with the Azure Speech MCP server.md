# Develop a speech agent with the Azure Speech MCP server

Source: [Develop a speech agent with the Azure Speech MCP server][src-msft-docs]

## Understand the Azure Speech MCP server

**Azure Speech MCP server capabilities**:

The Azure Speech MCP server exposes two core speech capabilities as tools that any MCP-compatible agent can call:

| Capability | Description |
| --- | --- |
| Speech-to-text (Recognize) | Converts audio files to text using advanced speech recognition. Supports WAV, MP3, OGG, FLAC, MP4, M4A, AAC, and other common audio formats. Includes options for language selection, phrase hints for improved accuracy, profanity filtering, and detailed or simple output formats. |
| Text-to-speech (Synthesize) | Converts text input into natural-sounding audio files using neural text-to-speech voices. Supports multiple languages and voices (for example, en-US-JennyNeural or en-GB-SoniaNeural), and generates output in WAV, MP3, or other formats. |

**Storage requirements**:

Unlike text-only MCP tools, the Azure Speech MCP server works with audio files, which requires an Azure Storage account.

- Text-to-speech: The Speech MCP server saves generated audio files to an Azure Blob Storage container. The agent's response includes a link to the generated audio file.
- Speech-to-text: The agent can transcribe audio files from a publicly accessible URL or from an Azure Blob Storage container accessed with a SAS URL.

When you connect the Speech MCP server to your agent, you provide a SAS URL for a blob container. The SAS URL grants the MCP server permission to read and write files in that container.

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/develop-speech-agent-speech-mcp/
