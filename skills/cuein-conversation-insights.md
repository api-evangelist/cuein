---
name: Retrieve customer conversation insights
description: Pull paginated conversation insight records from the Cuein Insights API over a date range, then drill into a single conversation.
api: openapi/cuein-insights-openapi.json
operations:
  - Retrieve the latest processed conversation timestamp
  - Retrieve insights for multiple conversations
  - Retrieve insights for a conversation
---

# Retrieve customer conversation insights

Use the Cuein **Insights API** (`https://api.cuein.ai/insights/v1`) to read AI-derived
insights (contact reasons, root causes, resolutions, and metrics such as Inferred CSAT
and Resolution Rate) about customer-support conversations.

## Auth
Send an `x-api-key: <YOUR_KEY>` header on every request (scheme `ApiKeyAuth`). Keys are
issued by Cuein — there is no self-serve signup. A missing/invalid key returns `401
Authentication Failure`.

## Steps
1. **Check freshness** — call `Retrieve the latest processed conversation timestamp`
   (`GET /conversations/meta/lastProcessed`) to learn the newest conversation that is
   processed and ready to read. Use its `timestamp` as your upper bound.
2. **List insights over a window** — call `Retrieve insights for multiple conversations`
   (`GET /conversations`) with required `timeframeFrom`, `timeframeTo`
   (`YYYY-MM-DD` or `YYYY-MM-DD HH:mm:SS`) and `limit`. Read `meta` for paging and
   iterate: pass the returned cursor back as `nextToken` until the page is empty.
3. **Drill into one** — for any `sourceId` of interest, call `Retrieve insights for a
   conversation` (`GET /conversations/{id}`) to get the full `ConversationResponse`
   (summary, intents, rootCauses, resolutions, policies, metrics, botMetrics,
   agentMetrics).

## Conventions & errors
- **Pagination is cursor-based** via `nextToken` + `limit` (see `conventions/cuein-conventions.yml`).
- Errors are plain HTTP status codes (`400` Invalid Request Payload, `401` Authentication
  Failure, `500` Internal Cuein Error) — no problem+json envelope. See
  `errors/cuein-problem-types.yml`.
- No idempotency-key or request-id header is documented.
