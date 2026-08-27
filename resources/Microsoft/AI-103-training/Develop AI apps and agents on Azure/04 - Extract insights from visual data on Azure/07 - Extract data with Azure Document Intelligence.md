# Extract data with Azure Document Intelligence

Source: [Extract data with Azure Document Intelligence][src-msft-docs]

## What is Azure Document Intelligence?

Azure Document Intelligence is a cloud-based AI service in Microsoft Foundry that uses OCR and deep learning models to extract text, key-value pairs, selection marks, and tables from documents.

OCR captures document structure by creating bounding boxes around detected objects in an image. The locations of the bounding boxes are recorded as coordinates in relation to the rest of the page. Azure Document Intelligence returns bounding box data and other information in a structured JSON format that preserves the relationships from the original document.

**Input requirements**:

Azure Document Intelligence works on input documents that meet these requirements:

- Format must be JPEG, PNG, BMP, PDF (text or scanned), or TIFF. The read model also accepts Microsoft Office file formats.
- File size must be less than 500 MB for the standard tier and 4 MB for the free tier.
- Image dimensions must be between 50 x 50 pixels and 10,000 x 10,000 pixels.
- PDF documents must have dimensions less than 17 x 17 inches (A3 paper size).
- PDF documents must not be password-protected.

[src-msft-docs]: https://learn.microsoft.com/en-us/training/modules/extract-data-with-document-intelligence/1-introduction?pivots=text
