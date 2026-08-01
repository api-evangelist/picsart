---
name: Remove image background with Picsart
description: Remove or replace the background of an image using the Picsart Programmable Image API, handling async execution and results.
api: openapi/picsart-image-tools-api-openapi-original.yml
operations: [image-remove-background, image-getresult, image-credits-balance]
---

# Remove image background with Picsart

Use the Picsart Programmable Image API to cut out or replace an image background.

## Auth
- Send your key in the `X-Picsart-API-Key` header on every request. Get it at https://console.picsart.io/.
- Base URL: `https://api.picsart.io/tools/1.0`.

## Steps
1. (Optional) Call `image-credits-balance` to confirm auth works and you have credits (free accounts start with 200).
2. Call `image-remove-background` with the source image as `image_url` (a URL or base64 DATA URI) or a binary upload. Set `output_type` / `bg_color` / `bg_image_url` if replacing the background.
3. For large images or heavy jobs, request async with the `Prefer: respond-async` header — you get `202 Accepted` and a `transaction_id`.
4. If async, poll `image-getresult` with the `transaction_id` until the result URL is returned.

## Rules
- Prefer `application/json` requests; always send `Accept: application/json`.
- On `429`, honor `X-Picsart-RateLimit-Reset-Time` and back off exponentially.
- Errors return `{ code, message, detail }`; `402` means credits are exhausted.
- Do not use deprecated `*_id` inputs (removed 2026-06-01) — use `*_url` or binary upload.
