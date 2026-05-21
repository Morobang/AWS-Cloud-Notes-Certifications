# Amazon Rekognition

## What you'll learn
- What Amazon Rekognition does
- Image vs video analysis capabilities
- Key use cases
- Key exam facts

---

## What is Amazon Rekognition?

Amazon Rekognition is a **fully managed computer vision service** that analyzes images and videos to detect objects, scenes, text, faces, and activities. You provide an image or video; Rekognition returns structured labels describing what it found.

No ML experience is required — Rekognition's models are pre-trained and accessed via API.

---

## Image analysis capabilities

| Feature | What it detects | Example |
|---------|----------------|---------|
| Object and scene detection | Labels for objects, scenes, and activities | "dog," "beach," "sunset," "sports" |
| Facial analysis | Face presence, estimated age, gender, emotions, facial landmarks | "smiling," "eyes open," "estimated age 30–40" |
| Facial comparison | Compare two faces to determine if they are the same person | Identity verification |
| Facial search | Search a face against a collection of known faces | Find a person in a large database |
| Text detection | Read printed or handwritten text within an image | License plates, signs, captions |
| Celebrity recognition | Identify public figures | Identify athletes or actors in media |
| Content moderation | Detect explicit, suggestive, or violent content | Filter inappropriate images in user uploads |

---

## Video analysis capabilities

Rekognition can analyze video streams (from Kinesis Video Streams) or stored videos (from S3):

- Detect people, activities, and objects over time
- Track people across video frames (person tracking)
- Detect when specific activities occur (e.g., package delivery, person entering a restricted area)
- Facial recognition in video — identify when a specific person appears

---

## Key use cases

**Content moderation** — Automatically scan user-uploaded photos or videos on a platform to detect and flag inappropriate content before it is published.

**Identity verification** — Compare a selfie to a government ID photo to verify a user's identity during account creation or login.

**Workplace safety** — Analyze security camera footage to detect if workers are wearing required safety equipment (hard hats, vests).

**Media and entertainment** — Index video content by detecting scenes, celebrities, and activities for searchable content libraries.

---

## Exam focus

- Rekognition = **image and video analysis** — detects objects, faces, text, scenes, and activities
- No ML training required — pre-trained models via API
- Key features for the exam: **object detection**, **facial recognition**, **content moderation**, **text in images**
- Use when exam describes: "detect faces," "label objects in images," "moderate user-uploaded photos," "identify text in images"

---

## Practice questions

**Q1.** A social media platform wants to automatically detect and blur explicit content in photos uploaded by users before the photos are published. Which AWS service can analyze uploaded images and flag inappropriate content?

A) Amazon Comprehend  
B) Amazon Textract  
C) Amazon Rekognition  
D) Amazon Macie

**Answer: C** — Rekognition's content moderation feature analyzes images and returns labels for explicit or suggestive content, allowing the platform to automatically flag or block inappropriate uploads. Comprehend analyzes text, not images. Textract extracts text from document images. Macie detects sensitive data (PII) in S3 — it analyzes data files, not visual image content.
