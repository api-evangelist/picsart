---
name: Generate an image from text with Picsart GenAI
description: Create images from a text prompt using the Picsart GenAI Text2Image API and retrieve the async results.
api: openapi/picsart-genai-tools-api-openapi-original.yml
operations: [genai-text2image, genai-text2image-getresult, genai-credits-balance]
---

# Generate an image from text with Picsart GenAI

## Auth
- `X-Picsart-API-Key` header. Base URL `https://genai-api.picsart.io/v1`.

## Steps
1. (Optional) Call `genai-credits-balance` to confirm auth and credits.
2. Call `genai-text2image` with a `prompt` (and optional `negative_prompt`, `count`, dimensions up to 1024x1024, and a `model` from the AI Hub catalog). GenAI generation defaults to async and returns an `inference_id`.
3. Poll `genai-text2image-getresult` with the `inference_id`. Treat status `success` and `error` as terminal; `processing` means keep waiting.

## Rules
- Always send `Accept: application/json`.
- Errors use `{ code, message, detail }`; `402` = credits exhausted, `422` = invalid parameters.
- Respect the 500 req/min limit and `X-Picsart-RateLimit-*` headers.
