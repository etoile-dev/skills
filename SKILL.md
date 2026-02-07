---
name: etoile
description: Implement and troubleshoot Etoile integrations for real apps using the API, Node SDK, and React SDK. Use when requests mention Etoile indexing/search, `/api/v1/index`, `/api/v1/search`, `/api/v1/documents`, `@etoile-dev/client`, `@etoile-dev/react`, cURL/fetch integration examples, key usage (public vs secret), search UI setup, or migration from another search provider.
---

# Etoile

## Overview

Use this skill to help users directly use Etoile or integrate it into production code. Return concise, working snippets with correct auth and documented payloads.

## Workflow

1. Identify the requested operation:

- `index` document
- `search` documents
- `update metadata`
- `delete` document
- React search UI integration
- integration troubleshooting

2. Choose the right surface:

- Prefer SDK when writing app code.
- Provide JavaScript `fetch` and cURL when user asks for raw HTTP or backend integration.

3. Select the correct auth key:

- `secret key`: index, patch metadata, delete
- `public key`: search
- Never expose secret keys in client-side code.

4. Validate request limits before returning code:

- Query length max `150`
- Search results default `10`, max `100`
- Title max `200`
- Collection name max `50`
- Metadata size max `10,000` chars (`JSON.stringify(metadata).length`)

5. Return examples in this order when multiple formats are needed:
1. SDK
1. JavaScript (`fetch`)
1. cURL

## Canonical Endpoints

Base URL:

- `https://etoile.dev/api/v1`

Endpoints:

- `POST /api/v1/index`
- `POST /api/v1/search`
- `PATCH /api/v1/documents`
- `DELETE /api/v1/documents`

Auth header:

- `Authorization: Bearer <API_KEY>`
- `Content-Type: application/json`

## Operation Checklist

### Index

- Require: `id`, `collection`, `title`, `content`
- Optional: `metadata`
- Accept `content` as:
  - plain text
  - website URL
  - image URL (`PNG`, `JPEG`, `WEBP`, `GIF`, max `5MB`)
- Response contains: `id`, `message`, `type`, `usage`
- Use secret key

### Search

- Require: `query`, `collections`
- Optional: `limit`, `offset`
- Response results include: `external_id`, `title`, `collection`, `metadata`, `score`
- Score range is `0-1`
- Use public key (or secret key)

### Update Metadata

- Endpoint: `PATCH /api/v1/documents`
- Require: `id`, `metadata`
- Behavior: metadata is replaced with provided object
- Use secret key

### Delete

- Endpoint: `DELETE /api/v1/documents`
- Require: `id`
- Use secret key

## SDK Guidance

### Node SDK (`@etoile-dev/client`)

- Install: `npm i @etoile-dev/client`
- Initialize with:
  - `new Etoile({ apiKey: process.env.ETOILE_API_KEY })`
- For search in browser/client contexts, use public key variables.
- If editing existing quickstart snippets, keep existing `secretKey`/`publicKey` style when already present in that file.

### React SDK (`@etoile-dev/react`)

- Install: `npm i @etoile-dev/react`
- Fastest setup:
  - optional `import "@etoile-dev/react/styles.css"`
  - use `<Search apiKey={process.env.NEXT_PUBLIC_ETOILE_PUBLIC_KEY} collections={[...]} />`
- Advanced:
  - primitives: `SearchRoot`, `SearchInput`, `SearchResults`, `SearchResult`
  - hook: `useSearch({ apiKey, collections, limit?, debounceMs? })`

## Integration Guardrails

- Never output real API keys.
- Use secret keys only server-side.
- Use public keys for browser search.
- Keep payloads to documented fields only.
- Include end-to-end flow when useful: index first, then search.

## Reference

For exact payloads, limits, and response snippets, read:

- `references/etoile-api-reference.md`
