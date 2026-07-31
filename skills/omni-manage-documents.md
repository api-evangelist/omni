---
name: Create and manage Omni documents
description: Organize folders and create, list, move, and delete Omni workbook/dashboard documents.
api: openapi/omni-openapi-original.yml
operations: [listFolders, createDocument, listDocuments, getDocument, moveDocument, deleteDocument]
---

# Create and manage Omni documents

Manage Omni documents (workbooks / dashboards) and their folder placement.

## Base URL & auth
- `https://{instance}.omniapp.co/api`, `Authorization: Bearer <token>` (Organization API Key or PAT).

## Steps
1. **Find a folder** — `GET /v1/folders` (`listFolders`) to pick a destination `folderId`.
2. **Create a document** — `POST /v1/documents` (`createDocument`).
3. **List documents** — `GET /v1/documents` (`listDocuments`); paginate via `cursor`/`pageSize` and `pageInfo.nextCursor`.
4. **Read one** — `GET /v1/documents/{documentId}` (`getDocument`).
5. **Move it** — `PUT /v1/documents/{documentId}/move` (`moveDocument`) to relocate between folders.
6. **Delete it** — `DELETE /v1/documents/{documentId}` (`deleteDocument`).

## Conventions & errors
- Cursor pagination with `{records, pageInfo}`; rate limit 60 requests/minute.
- Errors are JSON `{status, detail}`: `403` = no permission on the document, `404` = not found, `409` = identifier conflict. See `errors/omni-problem-types.yml`.
- Documents v2 offers a draft/publish workflow under `/v2/documents` (`documentsV2Create`, `documentsV2PublishDraft`).
