---
name: denbury-resources-retrieve-esg-and-investor-documents
description: >-
  Locate and download Denbury Inc. corporate presentations, ESG and corporate responsibility reports
  and other PDFs from the anonymous WordPress media library, filtering by MIME type and date.
api: denbury-resources:denbury-resources-media-api
operations:
- listMedia
- getMediaItem
- search
---

# Retrieve Denbury ESG and investor documents

The Denbury media library holds 432 items, and it is where the substantive material lives —
corporate presentations, corporate responsibility reports, ESG metrics, past reports and imagery.
The `/wp/v2/media` route is anonymous and read-only.

## 1. Find the documents

`listMedia` — `GET https://www.denbury.com/wp-json/wp/v2/media`

Useful parameters, all declared in the server's own OPTIONS schema and captured in
`openapi/denbury-resources-media-api-openapi.yml`:

- `media_type=application` — narrows to non-image attachments, which is where the PDFs are.
- `mime_type=application/pdf` — narrows further to PDFs specifically.
- `search=<term>` — e.g. `corporate presentation`, `responsibility`, `ESG`.
- `after` / `before` — ISO 8601 publication-date bounds, useful for pulling a single reporting year.
- `orderby=date&order=desc` — newest first.
- `per_page=100` — the enforced maximum.

Page with the `Link` header's `rel="next"`, and size the walk from `X-WP-Total` (432 at capture).

## 2. Download

Each item carries `source_url` — the direct file URL on www.denbury.com. Fetch that, not the `link`
field, which is the WordPress attachment *page*.

Other fields worth reading before you download: `mime_type`, `filesize`, `filename`, `title.rendered`,
`date`, and `media_details` (for images, the generated size variants; for documents, page metadata
where WordPress captured it).

## 3. Cross-check against the site

`search` — `GET https://www.denbury.com/wp-json/wp/v2/search?search=<term>`

Returns lightweight `{id, title, url, type, subtype}` results across all 105 indexed objects. Use
`subtype` to decide where to resolve each hit: `attachment` → `/wp/v2/media/{id}`, `page` →
`/wp/v2/pages/{id}`. The Resource Center, Corporate Responsibility Report, Past Reports and ESG
Metrics pages are the human entry points to the same files.

## 4. Rules

- **Anonymous, read-only.** No key, no token, no signup. Do not send an `Authorization` header.
- **Throttle.** No rate-limit headers are returned; treat the limit as unspecified rather than
  absent, and keep the download rate low.
- **`author` is unresolvable** — `/wp/v2/users` returns `403`. Attribute documents to Denbury Inc.,
  not to a user id.
- **Provenance matters here.** These are historical documents from a company acquired by ExxonMobil
  in November 2023 and deregistered with the SEC on 2023-11-22. Date-stamp anything you extract and
  do not present it as current corporate reporting. See
  `lifecycle/denbury-resources-lifecycle.yml`.
- **Errors:** `rest_post_invalid_id` (404) for a bad media id, `rest_invalid_param` (400) for an
  out-of-range `per_page` or a malformed date. HTML 403s come from nginx, not the REST layer.
