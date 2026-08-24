# Analyze text with Azure Language in Foundry Tools

Source: [Analyze text with Azure Language in Foundry Tools][src-msft-docs]

## Azure Language in Microsoft Foundry Tools

Azure Language in Foundry Tools is designed to help you extract information from text. It provides functionality that you can use for tasks like:

- *Language detection*: determining the language in which text is written.
- *Named entity recognition*: detecting references to entities, including people, locations, time periods, organizations, and more.
- *Personally Identifiable Information (PII) extraction*: identifying and redacting personal details in text.

### Authentication

**Key-based Authentication**:

```python
# run "pip install azure-ai-textanalytics" first to install the package 
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Create client using endpoint and key
credential = AzureKeyCredential("YOUR_FOUNDRY_RESOURCE_KEY")
client = TextAnalyticsClient(endpoint="YOUR_FOUNDRY_RESOURCE_ENDPOINT", 
                             credential=credential)
```

**Entra-id Authentication**:

```python
# run "pip install azure-identity azure-ai-textanalytics" first to install the packages 
from azure.identity import DefaultAzureCredential
from azure.ai.textanalytics import TextAnalyticsClient

# Create client using endpoint and default Azure identity
credential = DefaultAzureCredential()
client = TextAnalyticsClient(endpoint="YOUR_FOUNDRY_RESOURCE_ENDPOINT", 
                             credential=credential)
```

## Detect language

The Azure Language detection API evaluates text input and, for each document submitted, returns language identifiers with a score indicating the strength of the analysis.

You can parse the results of this analysis to determine which language is used in the input document. The response also returns a score, which reflects the confidence of the model (a value between 0 and 1).

Language detection can work with documents or single phrases. **It's important to note that the document size must be under 5,120 characters**. The size limit is per document and **each collection is restricted to 1,000 items** (IDs).

```python
# Assumes code to create TextAnalyticsClient is above...

# Example text to analyze
documents = ["Hello World!", "Bonjour le monde!"]

# Detect language
response = client.detect_language(documents=documents)
for doc in response:
    print(f"Document: {doc.id}")
    print(f"\tPrimary Language: {doc.primary_language.name}")
    print(f"\tISO6391 Name: {doc.primary_language.iso6391_name}")
    print(f"\tConfidence Score: {doc.primary_language.confidence_score}")
```

Response format:

```txt
Document: 0
        Primary Language: English
        ISO6391 Name: en
        Confidence Score: 0.9
Document: 1
        Primary Language: French
        ISO6391 Name: fr
        Confidence Score: 0.98
```

The last condition to consider is when there's ambiguity as to the language content. The scenario might happen if you submit textual content that the analyzer isn't able to parse, for example because of character encoding issues when converting the text to a string variable. As a result, the response for the language name and ISO code will be returned as (unknown) and the score value will be returned as 0.

## Extract entities

```python
# Example text to analyze
documents = ["Microsoft was founded on April 4, 1975 by Bill Gates and Paul Allen in Albuquerque, New Mexico.",
             "Satya Nadella became CEO of Microsoft on February 4, 2014."]

# Extract named entities
response = client.recognize_entities(documents=documents)
for doc in response:
    print(f"Entities in document {doc.id}:")
    for entity in doc.entities:
        print(f" - {entity.text} ({entity.category})")
```

```markdown
Entities in document 0:
 - Microsoft (Organization)
 - April 4, 1975 (DateTime)
 - Bill Gates (Person)
 - Paul Allen (Person)
 - Albuquerque (Location)
 - New Mexico (Location)
Entities in document 1:
 - Satya Nadella (Person)
 - CEO (PersonType)
 - Microsoft (Organization)
 - February 4, 2014. (DateTime)
```

## Extract personally identifiable information (PII)

Azure Language provides PII detection and redaction capabilities to identify sensitive information such as names, addresses, phone numbers, email addresses, social security numbers, and credit card numbers. You can both extract PII entities for analysis and redact (mask) them to protect privacy.

```python
# Example text to analyze
documents = ["John Smith works at Contoso Ltd. His email is john.smith@contoso.com and his phone number is 555-012-456.",
             "Patient Sarah Johnson, SSN 123-45-6789, was admitted on 03/15/2024."]

# Extract PII entities
response = client.recognize_pii_entities(documents=documents, language="en")
for doc in response:
    print(f"\nPII entities in document {doc.id}:")
    for entity in doc.entities:
        print(f" - {entity.text}: {entity.category} (confidence: {entity.confidence_score:.2f})")
```

```markdown
PII entities in document 0:
 - John Smith: Person (confidence: 0.99)
 - Contoso Ltd: Organization (confidence: 0.85)
 - john.smith@contoso.com: Email (confidence: 1.00)
 - 555-012-456: PhoneNumber (confidence: 0.80)
PII entities in document 1:
 - Sarah Johnson: Person (confidence: 0.99)
 - 123-45-6789: USSocialSecurityNumber (confidence: 0.99)
 - 03/15/2024: DateTime (confidence: 0.80)
```

You can also redact the PII entities to protect sensitive information. The service returns a redacted version of the text with PII replaced by asterisks or a specified character:

```python
# Redact PII entities
response = client.recognize_pii_entities(documents=documents, language="en")
for doc in response:
    print(f"\nDocument {doc.id} (redacted):")
    print(f" {doc.redacted_text}")
```

```markdown
Document 0 (redacted):
 ********** works at ************. His email is ************************ and his phone number is ********.
Document 1 (redacted):
 Patient *************, SSN ***********, was admitted on **********.
```

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/analyze-text-ai-language/
