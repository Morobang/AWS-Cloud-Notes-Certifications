# Amazon Textract

## What you'll learn
- What Textract does and how it differs from basic OCR
- What it extracts: text, forms, tables
- Key use cases
- Key exam facts

---

## What is Amazon Textract?

Amazon Textract is a **fully managed document analysis service** that automatically extracts text, forms data, and table data from scanned documents and images. It goes beyond simple optical character recognition (OCR) by understanding the structure of documents — recognizing form fields, table rows, and their relationships.

---

## Textract vs basic OCR

**Basic OCR** extracts raw text from a document image — just the words, with no understanding of structure. You get a stream of characters without knowing which text is a form label and which is a form value.

**Textract** understands document structure:

- Knows that "First Name:" is a form label and "John" is its value
- Knows that a table has rows and columns
- Returns structured key-value pairs: `{"First Name": "John", "Last Name": "Smith", "Date": "2024-01-15"}`

---

## What Textract extracts

| Extraction type | What it returns |
|----------------|----------------|
| Raw text | All printed or handwritten text in the document |
| Form data | Key-value pairs from form fields (label + value) |
| Tables | Table cells with row and column relationships preserved |
| Queries | Answer specific questions about the document ("What is the invoice total?") |
| Signatures | Detect the presence of signatures |

---

## How it works

1. You submit an image (JPEG, PNG, TIFF) or PDF to Textract
2. Textract analyzes the document layout and extracts the content
3. It returns structured JSON with the extracted text, form fields, and tables
4. Your application processes the structured data (store in a database, validate fields, trigger workflows)

For large volumes, Textract supports asynchronous processing — submit a job, poll for completion, retrieve results.

---

## When to use it

**Document processing automation** — Extract data from invoices, receipts, and purchase orders without manual data entry.

**Healthcare forms** — Process patient intake forms, insurance claims, and medical records.

**Financial documents** — Extract data from bank statements, tax forms, and contracts.

**Identity document processing** — Read data from passports, driver's licenses, and identity documents.

**Compliance document review** — Extract and index content from regulatory filings or legal contracts.

---

## Exam focus

- Textract = **extract text and structure from documents** — goes beyond OCR to understand forms and tables
- Returns **key-value pairs** from forms and **structured tables**
- Processes PDFs, scanned documents, and images
- Use when exam describes: "extract data from scanned forms," "process invoices automatically," "OCR with structure understanding," "parse forms without manual data entry"

---

## Practice questions

**Q1.** A healthcare company receives thousands of paper patient intake forms that are scanned and stored as PDFs. They want to automatically extract patient name, date of birth, and insurance information from each form into a database without manual data entry. Which AWS service is most appropriate?

A) Amazon Comprehend  
B) Amazon Rekognition  
C) Amazon Textract  
D) Amazon Transcribe

**Answer: C** — Textract extracts text and form data (key-value pairs) from document images and PDFs — exactly the use case of reading structured fields from a form. Comprehend analyzes text for sentiment and entities but requires text input, not image input. Rekognition detects objects and faces in images but doesn't extract structured form data. Transcribe converts audio to text.
