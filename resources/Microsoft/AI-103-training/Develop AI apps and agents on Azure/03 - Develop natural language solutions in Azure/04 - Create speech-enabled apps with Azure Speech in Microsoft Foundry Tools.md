# Create speech-enabled apps with Azure Speech in Microsoft Foundry Tools

Source: [Create speech-enabled apps with Azure Speech in Microsoft Foundry Tools][src-msft-docs]

## Azure Speech in Foundry Tools

**Creating a SpeechConfig**:

The initial object you need to create to provide access to the Azure Speech in Foundry Tools endpoint is a `SpeechConfig` object; which encapsulates the connection details for the service in your Foundry resource.

For example, the following Python code creates a SpeechConfig object that can be used to submit requests to Azure Speech APIs in a Foundry resource.

```python
# run "pip install azure-cognitiveservices-speech" first to install the package 
import azure.cognitiveservices.speech as speech_sdk

# Create SpeechConfig using endpoint and key
speech_config = speech_sdk.SpeechConfig(subscription="YOUR_FOUNDRY_KEY",
                                        endpoint="YOUR_FOUNDRY_ENDPOINT")
```

## Use the Speech to Text API

![img-speech-config](./images/04%20-%20speech%20to%20text%20api.png)

There's a consistent pattern for using the Speech to text API:

1. Use a `SpeechConfig` object to encapsulate the information required to connect to your Foundry resource. Specifically, its endpoint (or region) and key.
1. Optionally, use an `AudioConfig` to define the input source for the audio to be transcribed. By default, this is the default system microphone, but you can also specify an audio file.
1. Use the `SpeechConfig` and `AudioConfig` to create a `SpeechRecognizer` object. This object is a proxy client for the Speech to text API.
1. Use the methods of the `SpeechRecognizer` object to call the underlying API functions. For example, the `RecognizeOnceAsync()` method uses the Azure Speech service to asynchronously transcribe a single spoken utterance.
1. Process the response. In the case of the `RecognizeOnceAsync()` method, the result is a `SpeechRecognitionResult` object that includes the following properties:
   1. Duration
   1. OffsetInTicks
   1. Properties
   1. Reason
   1. ResultId
   1. Text

**Example - Transcribing an audio file**:

```python
import azure.cognitiveservices.speech as speech_sdk

# Speech config encapsulates the connection to the resource
speech_config = speech_sdk.SpeechConfig(subscription="YOUR_FOUNDRY_KEY",
                                       endpoint="YOUR_FOUNDRY_ENDPOINT")

# Audio config determines the audio stream source (defaults to system mic)
file_path = "audio.wav"
audio_config = speech_sdk.audio.AudioConfig(filename=file_path)

# Use a speech recognizer to transcribe the audio
speech_recognizer = speech_sdk.SpeechRecognizer(speech_config=speech_config,
                                               audio_config=audio_config)

result = speech_recognizer.recognize_once_async().get()

# Did it succeeed
if result.reason == speech_sdk.ResultReason.RecognizedSpeech:
    # Yes!
    print(f"Transcription:\n{result.text}")
else:
    # No. Try to determine why.
    print("Error transcribing message: {}".format(result.reason))
```

## Use the Text to Speech API

![text-2-speech-api](./images/04%20-%20text%20to%20speech%20api.png)

The pattern for implementing speech synthesis is similar to that of speech recognition:

1. Use a `SpeechConfig` object to encapsulate the information required to connect to your Azure Speech resource. Specifically, its location and key.
1. Optionally, use an `AudioConfig` to define the output device for the speech to be synthesized. By default, this is the default system speaker, but you can also specify an audio file, or by explicitly setting this value to a null value, you can process the audio stream object that is returned directly.
1. Use the `SpeechConfig` and `AudioConfig` to create a `SpeechSynthesizer` object. This object is a proxy client for the Text to speech API.
1. Use the methods of the `SpeechSynthesizer` object to call the underlying API functions. For example, the `SpeakTextAsync()` method uses the Azure Speech service to convert text to spoken audio.
1. Process the response from the Azure Speech service. In the case of the `SpeakTextAsync` method, the result is a `SpeechSynthesisResult` object that contains the following properties:
   1. AudioData
   1. Properties
   1. Reason
   1. ResultId

**Example - synthesizing text as speech**:

```python
import azure.cognitiveservices.speech as speechsdk

# Speech config encapsulates the connection to the resource
speech_config = speechsdk.SpeechConfig(subscription=KEY, endpoint=ENDPOINT)

# Audio output config determines where to send the audio stream (defaults to speaker)
audio_config = speechsdk.audio.AudioOutputConfig(use_default_speaker=True)

# Use speech synthesizer to synthesize text as speech
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config,
                                                 audio_config=audio_config)
text = "My voice is my password!"
speech_synthesis_result = speech_synthesizer.speak_text_async(text).get()

# Did it succeeed?
if speech_synthesis_result.reason == speechsdk.ResultReason.SynthesizingAudioCompleted:
    # Yes!
    print("Speech synthesized for text [{}]".format(text))
elif speech_synthesis_result.reason == speechsdk.ResultReason.Canceled:
    # No - Ty to find out why not
    cancellation_details = speech_synthesis_result.cancellation_details
    print("Speech synthesis canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        if cancellation_details.error_details:
            print("Error details: {}".format(cancellation_details.error_details))
```

## Configure audio format and voices

**Audio format**:

Azure Speech supports multiple output formats for the audio stream that is generated by speech synthesis. Depending on your specific needs, you can choose a format based on the required:

- Audio file type
- Sample-rate
- Bit-depth

**Voices**:

The Azure Speech service provides multiple voices that you can use to personalize your speech-enabled applications. Voices are identified by names that indicate a locale, a person's name, and other details - for example `en-US-Brian:DragonHDLatestNeural`.

## Use Speech Synthesis Markup Language

While the Azure Speech SDK enables you to submit plain text to be synthesized into speech, the service also supports an XML-based syntax for describing characteristics of the speech you want to generate. This Speech Synthesis Markup Language (SSML) syntax offers greater control over how the spoken output sounds, enabling you to:

- Specify a speaking style, such as "excited" or "cheerful" when using a neural voice.
- Insert pauses or silence.
- Specify phonemes (phonetic pronunciations), for example to pronounce the text "SQL" as "sequel".
- Adjust the prosody of the voice (affecting the pitch, timbre, and speaking rate).
- Use common "say-as" rules, for example to specify that a given string should be expressed as a date, time, telephone number, or other form.
- Insert recorded speech or audio, for example to include a standard recorded message or simulate background noise.

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/create-speech-enabled-apps/
