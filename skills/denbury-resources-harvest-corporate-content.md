---
name: denbury-resources-harvest-corporate-content
description: >-
  Harvest the Denbury Inc. corporate content corpus — all 48 published pages plus their full
  rendered bodies — from the anonymous WordPress REST API behind www.denbury.com, correctly paged
  and without over-fetching.
api: denbury-resources:denbury-resources-pages-api
operations:
- listPages
- getPage
---

# Harvest the Denbury corporate content corpus

The Denbury Inc. website is the whole corpus: `/wp/v2/posts` is empty (`X-WP-Total: 0`), so every
piece of published content is a Page. There are 48 of them.

## 1. Page through the collection

`listPages` — `GET https://www.denbury.com/wp-json/wp/v2/pages`

Send no credentials. The route answers `Allow: GET` anonymously.

- Set `per_page` to `100`. The server enforces a hard maximum of 100 and returns
  `400 rest_invalid_param` with `data.details.per_page.code = rest_out_of_bounds` above it.
- Read `X-WP-Total` (48 at capture) and `X-WP-TotalPages` from the response headers to size the walk.
- Follow `rel="next"` in the RFC 8288 `Link` header rather than incrementing `page` yourself; stop
  when no `rel="next"` is present.
- Use `_fields=id,slug,link,title,modified` on the first pass if you only need an index. The full
  page objects carry rendered HTML and are large.

## 2. Fetch bodies

`getPage` — `GET https://www.denbury.com/wp-json/wp/v2/pages/{id}`

Only needed if you skipped `content` with `_fields`. `content.rendered` is HTML, not markdown.

Leave `context` at its default `view`. Passing `context=edit` returns `401 rest_forbidden_context` —
that context is for authenticated site administrators and there is nothing in it you are entitled to.

## 3. Resolve the graph, and know where it dead-ends

- `parent` is another page id (`0` = top level). The site is hierarchical: Operations → Gulf Coast,
  Sustainability → Environment → Managing Our Carbon Footprint, and so on.
- `featured_media` is a media item id — resolve against `/wp/v2/media/{id}` (see the media skill).
- `author` is a user id and **cannot be resolved**: `/wp/v2/users` returns `403` at the origin.
  Do not retry it and do not report it as a transient failure; it is a deliberate origin rule.

## 4. Rules that apply to every call here

- **Read-only.** There is no public write surface. Idempotency, dry-run and reversibility are all
  `na` for this provider — see `conventions/denbury-resources-conventions.yml`.
- **No rate-limit signal.** No `X-RateLimit-*`, `RateLimit-*` or `Retry-After` header is returned and
  no 429 has been observed. That is an absence of signal, not permission: this is a legacy corporate
  site inside an acquirer's estate. Keep concurrency at 1–2 and pause between pages.
- **Branch on content-type before parsing.** Most errors are the WordPress JSON envelope
  (`{code, message, data}`), but `/wp/v2/users` and everything under `/.well-known/` return an nginx
  **HTML** 403. A blind `response.json()` will throw there.
- **Errors you will actually see:** `rest_invalid_param` (400), `rest_post_invalid_id` (404),
  `rest_no_route` (404), `rest_forbidden_context` (401). Full catalogue in
  `errors/denbury-resources-problem-types.yml`.
- **No versioning or deprecation policy exists.** The `wp/v2` namespace is whatever WordPress core
  ships on this host; routes can change with a core upgrade and no notice. Re-derive the specs rather
  than trusting a cached capture.
