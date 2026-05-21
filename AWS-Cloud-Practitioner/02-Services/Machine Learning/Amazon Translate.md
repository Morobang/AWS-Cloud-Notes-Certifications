# Amazon Translate

## What you'll learn
- What Amazon Translate does
- Batch vs real-time translation
- Key use cases
- Key exam facts

---

## What is Amazon Translate?

Amazon Translate is a **fully managed neural machine translation service** that automatically translates text between languages. It uses deep learning models to produce natural, accurate translations.

---

## How it works

You provide text and specify source and target languages. Translate returns the translated text. The source language can be auto-detected if unknown.

**Real-time translation** — Call the API with text; get translated text back immediately. Used for live chat, user interface localization, and on-demand content translation.

**Batch translation** — Submit large volumes of text or documents to S3; Translate processes them asynchronously. Used for translating large document archives, product catalogs, or content libraries.

---

## Key features

- **75+ languages**: Supports major world languages including English, Spanish, French, German, Mandarin, Japanese, Arabic, and more
- **Language auto-detection**: Translate can detect the source language automatically
- **Custom terminology**: Specify that certain terms (product names, brand names, technical terms) should not be translated or should use specific translations
- **Active Custom Translation**: Fine-tune the translation model on your own parallel text data to improve quality for domain-specific content
- **Formality**: Control whether translated output uses formal or informal register (for languages that distinguish these)

---

## When to use it

**Multilingual applications** — Translate user-generated content (reviews, comments, messages) in real time so users across languages can interact.

**Content localization** — Translate a product catalog, website, or documentation into multiple languages without a human translation team.

**Global customer support** — Translate incoming support tickets to the agent's language and translate responses back — enabling cross-language support.

**Document translation** — Translate large archives of documents (contracts, technical manuals) between languages in batch.

---

## Exam focus

- Translate = **neural machine translation** — translates text between 75+ languages
- Supports both **real-time** and **batch** translation
- **Custom terminology** prevents specific terms from being translated incorrectly
- Use when exam describes: "translate between languages," "localize content," "multilingual application," "translate documents"

---

## Practice questions

**Q1.** A global e-commerce platform operates in 20 countries. They want to automatically translate customer product reviews from any language into the viewer's local language, in real time as reviews are loaded. Which AWS service enables this?

A) Amazon Comprehend  
B) Amazon Translate  
C) Amazon Polly  
D) Amazon Transcribe

**Answer: B** — Translate provides real-time neural machine translation between languages — exactly what's needed to translate reviews on-demand as users load them. Comprehend analyzes text for sentiment and entities but does not translate. Polly converts text to speech. Transcribe converts speech to text.
