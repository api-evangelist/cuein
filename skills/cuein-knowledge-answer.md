---
name: Answer a question from the knowledge base
description: Generate an answer to a customer or agent question from Cuein's knowledge articles and documents, with source references.
api: openapi/cuein-answers-openapi.json
operations:
  - getAnswer
---

# Answer a question from the knowledge base

Use the Cuein **Answers API** (`https://api.cuein.ai/answers/v1`) to generate an answer
grounded in a tenant's knowledge articles and documents.

## Auth
Send an `x-api-key: <YOUR_KEY>` header (scheme `ApiKeyAuth`).

## Steps
1. **Ask** — call `getAnswer` (`GET /answer`) with the query params:
   - `question` — the question to answer.
   - `tenantId` — the tenant/client whose documents are searched.
   - `numDocuments` — how many documents to use as context.
   - optional context: `sessionId`, `customerId`, `userType`, `userName`,
     `documentType`, and `filter` (key/value pairs, AND-combined).
2. **Read the result** — the `AnswerResponse` carries the generated answer plus
   `ReferenceDocument` entries citing the source documents. Surface the references so the
   answer is auditable.

## Conventions & errors
- A malformed request returns `400 Invalid Request Payload`
  (see `errors/cuein-problem-types.yml`).
- Reuse a stable `sessionId` across turns to keep a conversational context.
