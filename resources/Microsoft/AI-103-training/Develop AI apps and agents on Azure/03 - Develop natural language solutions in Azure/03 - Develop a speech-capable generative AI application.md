# Develop a speech-capable generative AI application

Source: [Develop a speech-capable generative AI application][src-msft-docs]

## Transcribe speech

Models that support speech-to-text operations include:

- gpt-4o-transcribe
- gpt-4o-mini-transcribe
- gpt-4o-transcribe-diarize

```python
from openai import AzureOpenAI
from pathlib import Path

# Create an AzureOpenAI client
client = AzureOpenAI(
    azure_endpoint=YOUR_FOUNDRY_ENDPOINT,
    api_key=YOUR_FOUNDRY_KEY,
    api_version="2025-03-01-preview"
)

# Get the audio file
file_path = Path("speech.mp3")
audio_file = open(file_path, "rb")

# Use the model to transcribe the audio file
transcription = client.audio.transcriptions.create(
    model=YOUR_MODEL_DEPLOYMENT,
    file=audio_file,
    response_format="text"
)

print(transcription)
```

### Synthesize speech

Models that support text-to-speech operations include:

- gpt-4o-tts
- gpt-4o-mini-tts

```python
from openai import AzureOpenAI
from pathlib import Path

# Create an AzureOpenAI client
client = AzureOpenAI(
    azure_endpoint=YOUR_FOUNDRY_ENDPOINT,
    api_key=YOUR_FOUNDRY_KEY,
    api_version="2025-03-01-preview"
)

# Path for audio output file
speech_file_path = Path("output_speech.wav")

# Generate speech and save to file
with client.audio.speech.with_streaming_response.create(
            model=YOUR_MODEL_DEPLOYMENT,
            voice="alloy",
            input="This speech was AI-generated!",
            instructions="Speak in an upbeat, excited tone.",
    ) as response:
    response.stream_to_file(speech_file_path)

print(f"Speech generated and saved to {speech_file_path}")
```

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/develop-generative-ai-audio-apps/
