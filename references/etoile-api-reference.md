# Etoile API Reference (Docs-Aligned)

## Base

- Base URL: `https://etoile.dev/api/v1`
- Auth header: `Authorization: Bearer <API_KEY>`
- Content-Type: `application/json`

## Key Types

- Secret key: indexing, metadata updates, deletion
- Public key: search (safe for client-side)

## Endpoints

### `POST /api/v1/index`

Request:

```json
{
  "id": "starry-night",
  "collection": "paintings",
  "title": "The Starry Night",
  "content": "A swirling night sky over a village...",
  "metadata": {
    "artist": "Vincent van Gogh",
    "year": 1889
  }
}
```

Response:

```json
{
  "id": "starry-night",
  "message": "Document indexed successfully.",
  "type": "text",
  "usage": 847
}
```

Notes:
- `content` can be text, a website URL, or an image URL.
- Image URL formats: PNG, JPEG, WEBP, GIF (max 5MB).

### `POST /api/v1/search`

Request:

```json
{
  "query": "swirling sky painting",
  "collections": ["paintings"],
  "limit": 10,
  "offset": 0
}
```

Response:

```json
{
  "query": "swirling sky painting",
  "results": [
    {
      "external_id": "starry-night",
      "title": "The Starry Night",
      "collection": "paintings",
      "metadata": {
        "artist": "Vincent van Gogh",
        "year": 1889
      },
      "score": 0.94
    }
  ]
}
```

Notes:
- Scores are in `0-1`.
- Public key or secret key can be used.

### `PATCH /api/v1/documents`

Request:

```json
{
  "id": "starry-night",
  "metadata": {
    "artist": "Vincent van Gogh",
    "year": 1889,
    "featured": true
  }
}
```

Response:

```json
{
  "message": "Document metadata updated successfully.",
  "id": "starry-night"
}
```

Note:
- Metadata is replaced by the new object.

### `DELETE /api/v1/documents`

Request:

```json
{
  "id": "starry-night"
}
```

Response:

```json
{
  "message": "Document deleted successfully.",
  "id": "starry-night"
}
```

## Platform Limits

- Query length: max 150 chars
- Search results: default 10, max 100
- Title length: max 200 chars
- Collection name: max 50 chars
- Metadata size: max 10,000 chars (`JSON.stringify(metadata).length`)

## SDK Notes

### Node (`@etoile-dev/client`)

- Install: `npm i @etoile-dev/client`
- Client:

```ts
import { Etoile } from "@etoile-dev/client";

const etoile = new Etoile({
  apiKey: process.env.ETOILE_API_KEY,
});
```

### React (`@etoile-dev/react`)

- Install: `npm i @etoile-dev/react`
- Quick setup:

```tsx
import "@etoile-dev/react/styles.css";
import { Search } from "@etoile-dev/react";

<Search
  apiKey={process.env.NEXT_PUBLIC_ETOILE_PUBLIC_KEY}
  collections={["paintings"]}
/>;
```
