---
name: Classify customer feedback with a Kimola model
description: Run one or many pieces of customer feedback text through a Kimola Cognitive model (custom or pre-built) and read back the predicted tags with probabilities.
api: https://api.kimola.com/v1/cognitive
operations:
  - "GET /v1/cognitive/Models"
  - "GET /v1/cognitive/Models/{secret}"
  - "GET /v1/cognitive/Models/{secret}/tags"
  - "POST /v1/cognitive/Models/{secret}/tags"
  - "GET /v1/cognitive/Models/{code}/{language}/tags"
---

# Classify customer feedback with a Kimola model

## Auth
Send `Authorization: Bearer <apiKey>` on every request. Get the key from the
Kimola dashboard (Models page). A missing/malformed header returns 400; an
invalid key 401; a key without access to the model 403.

## Steps
1. **Find a model.** `GET /v1/cognitive/Models?pageIndex=0&pageSize=10` to list
   your custom models, or use a pre-built model `code` from the gallery. Confirm
   the model with `GET /v1/cognitive/Models/{secret}` and check `isReady: true`.
2. **Classify one text.** `GET /v1/cognitive/Models/{secret}/tags?text=<text>&strict=false`
   for a custom model, or `GET /v1/cognitive/Models/{code}/{language}/tags?text=<text>`
   for a pre-built model (language is an ISO 639-1 code). The response is an array
   of `{name, probability}` ordered by probability (highest first).
3. **Classify in batch.** `POST /v1/cognitive/Models/{secret}/tags` with a JSON
   array of `{id, text}` objects (up to 100 per request). The response returns
   `{id, label, probability}` per input.

## Rules
- Each classification consumes queries from your allocation (standard 1,
  aspect-based 2). Check remaining usage with `GET /v1/subscription/usage`.
- No idempotency key is supported — batch instead of retrying blindly.
- Errors are plain-text messages; see errors/kimola-error-codes.yml.
