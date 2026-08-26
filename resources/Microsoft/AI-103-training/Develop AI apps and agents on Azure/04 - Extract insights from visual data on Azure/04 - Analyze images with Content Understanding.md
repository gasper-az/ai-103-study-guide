# Analyze images with Content Understanding

Source: [Analyze images with Content Understanding][src-msft-docs]

## What is Content Understanding?

Azure Content Understanding is a Foundry Tool that uses generative AI to process and extract insights from many types of content, including documents, images, videos, and audio. It transforms unstructured data into structured, actionable output that you can integrate into automation and analytical workflows.

**Components**:

| Component | Description |
| --- | --- |
| Inputs | Source content including documents, images, video, and audio |
| Analyzer | Defines how content is processed, including extraction settings and field schema |
| Content extraction | Transforms unstructured input into normalized text and metadata using OCR, speech transcription, and layout detection |
| Field extraction | Generates structured key-value pairs based on your defined schema |
| Confidence scores | Provides reliability estimates from 0 to 1 for each extracted field value |
| Grounding | Identifies specific regions in content where each value was extracted |
| Structured output | Final result as Markdown for search scenarios or JSON for automation workflows |

**Analyzers**:

Analyzers are the core component that defines how your content is processed. Content Understanding offers two types:

- Prebuilt analyzers: Ready-to-use analyzers designed for common scenarios like invoice processing, receipt extraction, and call center analytics
- Custom analyzers: Tailored analyzers you create with your own field schema for specific business needs

When you create an analyzer, you configure:

- The base analyzer type (document, image, audio, or video)
- The AI models to use for processing
- The field schema that defines what data to extract
- Options like confidence scoring and content segmentation

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/analyze-images-with-content-understanding/
