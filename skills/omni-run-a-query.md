---
name: Run a query against an Omni model
description: Authenticate to an Omni instance, discover an available model, run a query, and retrieve results.
api: openapi/omni-openapi-original.yml
operations: [whoami, listModels, runQuery, waitForQuery]
---

# Run a query against an Omni model

Use the Omni REST API to run an analytics query against a semantic model.

## Base URL & auth
- Base URL mirrors your login URL with `/api` appended: `https://{instance}.omniapp.co/api`. HTTPS only.
- Send `Authorization: Bearer <token>` where the token is an Organization API Key or a Personal Access Token (PAT).

## Steps
1. **Confirm identity** — `GET /v1/whoami` (`whoami`) to verify the token and see the caller's user/permissions.
2. **List models** — `GET /v1/models` (`listModels`) and choose the `modelId` you want to query. Paginate with `cursor`/`pageSize`, following `pageInfo.nextCursor` while `pageInfo.hasNextPage` is true.
3. **Run the query** — `POST /v1/query/run` (`runQuery`) with the query definition scoped to the chosen model.
4. **Wait for results** — if the query runs asynchronously, poll `GET /v1/query/wait` (`waitForQuery`) until it completes, then read the returned records.

## Conventions & errors
- Rate limit: 60 requests/minute; a `429` returns `{status, detail}` — back off and retry.
- Errors are JSON `{status, detail}` (not RFC 9457). `401` = missing/invalid token, `403` = no access to the resource. See `errors/omni-problem-types.yml`.
- No idempotency-key support; treat retries accordingly.
