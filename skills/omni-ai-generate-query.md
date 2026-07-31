---
name: Generate and run a query with Omni AI
description: Use Omni's AI endpoints to translate a natural-language question into a query and retrieve the result.
api: openapi/omni-openapi-original.yml
operations: [generateQuery, aiPickTopic, createAIJob, getAIJobStatus, getAIJobResult]
---

# Generate and run a query with Omni AI

Turn a natural-language question into an Omni query using the AI endpoints.

## Base URL & auth
- `https://{instance}.omniapp.co/api`, `Authorization: Bearer <token>` (Organization API Key or PAT).

## Steps
1. **Pick a topic** — `POST /v1/ai/pick-topic` (`aiPickTopic`) with the model and the question to select the most relevant topic.
2. **Generate a query** — `POST /v1/ai/generate-query` (`generateQuery`) to translate the question into an Omni query against the chosen model/topic.
3. **Run as an AI job (async)** — `POST /v1/ai/jobs` (`createAIJob`) to execute; then poll `GET /v1/ai/jobs/{jobId}` (`getAIJobStatus`) until complete, and fetch `GET /v1/ai/jobs/{jobId}/result` (`getAIJobResult`). Cancel with `POST /v1/ai/jobs/{jobId}/cancel` (`cancelAIJob`) if needed.

## Conventions & errors
- AI usage is governed by AI Credit Controls; a `403`/`429` may indicate a credit or rate limit.
- Errors are JSON `{status, detail}`; rate limit is 60 requests/minute. See `conventions/omni-conventions.yml`.
