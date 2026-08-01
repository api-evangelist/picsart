---
name: Remove video background with Picsart
description: Remove or replace the background of a video using the Picsart Programmable Video API and retrieve the async result.
api: openapi/picsart-video-tools-api-openapi-original.yml
operations: [video-upload, video-remove-background, video-getresult, video-credits-balance]
---

# Remove video background with Picsart

## Auth
- `X-Picsart-API-Key` header. Base URL `https://video-api.picsart.io/v1`.

## Steps
1. (Optional) Call `video-credits-balance` to confirm auth and credits.
2. (Optional) Call `video-upload` to store the source video once and reuse its `transaction_id` across operations.
3. Call `video-remove-background` with the video (`video_url`, an uploaded `transaction_id`, or binary upload) and optional replacement background. Video jobs run asynchronously and return a `transaction_id`.
4. Poll `video-getresult` with the `transaction_id` until the output video URL is returned.

## Rules
- Video operations are long-running — use async (`Prefer: respond-async`) and poll rather than blocking.
- Handle `429` with back-off; `5xx` → retry later and check `status.picsart.io`.
- Errors return `{ code, message, detail }`.
