# Amazon Polly

## What you'll learn
- What Amazon Polly does
- Voice types: Standard vs Neural
- Key use cases
- Key exam facts

---

## What is Amazon Polly?

Amazon Polly is a **text-to-speech (TTS) service** that converts written text into natural-sounding spoken audio. It uses deep learning models to synthesize speech that sounds human.

Polly is the reverse of Amazon Transcribe: Transcribe converts speech to text; Polly converts text to speech.

---

## How it works

1. You provide text (a sentence, a paragraph, a full document)
2. Polly returns an audio file (MP3, OGG, PCM) containing the spoken version
3. You play the audio in your application

You can also stream the audio in real time rather than waiting for the full file to be generated.

---

## Voice types

**Standard voices** — Good quality text-to-speech at lower cost. Uses concatenative synthesis.

**Neural voices** — Higher quality, more natural-sounding speech using neural text-to-speech (NTTS). Noticeably more human-like with better intonation and pacing. Higher cost than standard.

Polly supports **60+ voices across 30+ languages** — useful for multilingual applications.

---

## SSML support

Polly supports Speech Synthesis Markup Language (SSML) — an XML-based language that lets you control how Polly speaks:

- Add pauses: `<break time="500ms"/>`
- Change speaking rate: `<prosody rate="slow">`
- Emphasize words: `<emphasis level="strong">`
- Specify pronunciations: `<phoneme>`

---

## When to use it

**Accessibility** — Convert website content or articles to audio for visually impaired users.

**E-learning and training** — Generate audio narration for educational content without recording a human voice actor.

**Notifications and alerts** — Speak alert messages or notifications in voice applications.

**IVR systems** — Generate spoken prompts for phone menus and contact center flows (often paired with Amazon Lex).

**Audiobooks** — Convert written content to audio format at scale.

---

## Exam focus

- Polly = **text-to-speech** — converts text into spoken audio
- Supports 60+ voices and 30+ languages
- **Neural voices** = higher quality, more natural sounding
- Use when exam describes: "generate audio from text," "text to speech," "read content aloud," "voice narration"
- Contrast with Transcribe: Polly = text → speech; Transcribe = speech → text

---

## Practice questions

**Q1.** A news website wants to add an audio version of every article so users can listen while commuting. The audio should be generated automatically when articles are published, without hiring voice actors. Which AWS service provides this capability?

A) Amazon Transcribe  
B) Amazon Translate  
C) Amazon Polly  
D) Amazon Lex

**Answer: C** — Polly converts text to natural-sounding speech, enabling the website to automatically generate audio versions of articles. Transcribe converts speech to text (the reverse). Translate converts text between languages. Lex builds conversational interfaces — it doesn't generate narration from articles.
