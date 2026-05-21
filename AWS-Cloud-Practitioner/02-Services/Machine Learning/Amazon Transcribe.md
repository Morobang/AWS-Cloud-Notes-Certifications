# Amazon Transcribe

## What you'll learn
- What Amazon Transcribe does
- Key features: speaker identification, custom vocabulary
- Key use cases
- Key exam facts

---

## What is Amazon Transcribe?

Amazon Transcribe is a **fully managed automatic speech recognition (ASR) service** that converts spoken audio into written text. You provide an audio or video file (or a live audio stream); Transcribe returns a text transcript.

Transcribe is the reverse of Amazon Polly: Polly converts text to speech; Transcribe converts speech to text.

---

## Key features

**Speaker identification (diarization)** — Transcribe can identify when different speakers are talking in a recording and label each speaker's turns. Useful for meeting transcriptions, call recordings, and interviews.

**Custom vocabulary** — Add domain-specific words that Transcribe might not recognize correctly — medical terms, product names, industry jargon — to improve accuracy.

**Custom language models** — Train a model on your domain-specific text to improve transcription accuracy for specialized content.

**Automatic content redaction** — Automatically identify and redact PII (names, phone numbers, SSNs) from transcripts.

**Multiple language support** — Transcribe supports 100+ languages.

**Real-time transcription** — Stream audio and receive transcript text in near real time, as the speech is happening.

---

## Transcribe Medical

A specialized version of Transcribe optimized for medical terminology. Used to transcribe doctor-patient conversations, clinical notes, and medical dictation with higher accuracy for medical vocabulary.

---

## When to use it

**Call center analytics** — Transcribe all customer service calls, then analyze the transcripts with Comprehend for sentiment, common complaints, and compliance.

**Meeting transcription** — Automatically generate meeting notes and searchable transcripts of video conference recordings.

**Media subtitling** — Generate subtitles and closed captions for video content at scale.

**Accessibility** — Provide text alternatives to audio content for hearing-impaired users.

**Medical documentation** — Use Transcribe Medical to convert physician dictation to text for EHR systems.

---

## Exam focus

- Transcribe = **speech-to-text** — converts audio/video to written text
- Supports **speaker identification** (who said what)
- **Transcribe Medical** = specialized version for medical terminology
- Use when exam describes: "transcribe audio," "convert speech to text," "generate meeting transcript," "subtitles for video"
- Contrast with Polly: Transcribe = speech → text; Polly = text → speech

---

## Practice questions

**Q1.** A company records all customer support calls and wants to automatically generate text transcripts of each call so they can search call content and analyze customer sentiment. Which AWS service converts the audio recordings to text?

A) Amazon Polly  
B) Amazon Comprehend  
C) Amazon Transcribe  
D) Amazon Lex

**Answer: C** — Transcribe converts audio recordings to text transcripts. Those transcripts can then be analyzed by Comprehend for sentiment. Polly converts text to speech (the reverse). Comprehend analyzes text but requires text input — it cannot process audio. Lex handles live conversational interactions, not transcription of recorded audio.
