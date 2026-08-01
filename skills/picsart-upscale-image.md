---
name: Upscale an image with Picsart
description: Increase image resolution using the Picsart Upscale / Ultra Upscale APIs, handling the async get-result flow.
api: openapi/picsart-image-tools-api-openapi-original.yml
operations: [image-upscale, image-ultra-upscale, image-ultra-upscale-getresult, image-getresult]
---

# Upscale an image with Picsart

## Auth
- `X-Picsart-API-Key` header. Base URL `https://api.picsart.io/tools/1.0`.

## Steps
1. For standard upscaling call `image-upscale` with `image_url` and an `upscale_factor` (e.g. 2/4/6/8). Synchronous by default; the response returns the upscaled image URL.
2. For higher quality / noise suppression call `image-ultra-upscale`. This runs asynchronously: it returns a `transaction_id`.
3. Poll `image-ultra-upscale-getresult` (or `image-getresult`) with the `transaction_id` until the finished image URL is returned.

## Rules
- Use `Prefer: respond-async` to force async, or `Prefer: wait=<seconds>` for bounded-wait-then-async.
- Watch credit balance; each retry consumes credits — avoid retry counts > 1.
- Handle `429` with back-off; check `status.picsart.io` on repeated `5xx`.
