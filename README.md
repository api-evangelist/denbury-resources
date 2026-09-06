# Denbury Resources (denbury-resources)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Denbury Resources (now Denbury Inc.) was an independent oil and natural gas company headquartered in
Plano, Texas, focused on enhanced oil recovery (EOR) using carbon dioxide injection in mature oil
fields across the Gulf Coast and Rocky Mountain regions, and on carbon capture, transport and
sequestration through the largest CO2 pipeline network in the United States. ExxonMobil acquired
Denbury in November 2023; it now operates inside ExxonMobil's Low Carbon Solutions business and
filed a Form 15-12G to deregister its securities on 2023-11-22.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/denbury-resources/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Producing
- **Access:** 3rd-Party
- **x-type:** company

## Tags

- Acquired, Carbon Capture, CO2 EOR, Energy, Enhanced Oil Recovery, ExxonMobil, Oil and Gas, Fortune 1000, Content, Carbon Sequestration

## Timestamps

- **Created:** 2024-12-03
- **Modified:** 2026-09-06

## APIs

Denbury publishes **no product API**: no developer portal, no API documentation, no SDKs, no CLI, no
package-registry presence, no GitHub organization, no status page, no changelog and no pricing. The
whole of `/.well-known/` is refused at the origin with HTTP 403.

The one machine-readable interface it exposes is the **WordPress core REST API** behind its corporate
website at `https://www.denbury.com/wp-json` — anonymous, read-only and unmetered. 276 routes across
14 namespaces are registered on the host; twelve operations are both anonymously readable and useful,
and they are documented in five OpenAPI 3.2.0 definitions **derived from the server's own HTTP OPTIONS
schema documents** on 2026-09-06. Nothing in those specs is invented.

| API | Surface | Size at capture |
|---|---|---|
| [Pages API](openapi/denbury-resources-pages-api-openapi.yml) | `/wp/v2/pages` | 48 published pages |
| [Media API](openapi/denbury-resources-media-api-openapi.yml) | `/wp/v2/media` | 432 items — presentations, ESG reports, PDFs |
| [Search API](openapi/denbury-resources-search-api-openapi.yml) | `/wp/v2/search` | 105 indexed objects |
| [Discovery API](openapi/denbury-resources-discovery-api-openapi.yml) | `/wp/v2/types`, `/taxonomies`, `/categories` | self-describing registry |
| [oEmbed API](openapi/denbury-resources-oembed-api-openapi.yml) | `/oembed/1.0/embed` | oEmbed 1.0 |

### The site is collapsing around the API

Probed 2026-09-06: of the 48 pages the API publishes, **41 redirect to the homepage** when fetched at
the `link` their own API record advertises. The bodies are still served by the API — page 31 "Carbon
Solutions" returns 17,650 characters of rendered HTML — so for 41 pages of Denbury's carbon-solutions,
EOR, pipeline-network and sustainability material this REST API is now the only public route to the
content. That is a wind-down signal, not a feature.

### Gated on this host

- `/wp/v2/users` — HTTP 403 at the origin; the `author` edge on every page and media item is unresolvable.
- `/wp-json/wp-abilities/v1/*` — HTTP 401 `rest_forbidden`; the WordPress Abilities API namespace is registered but entirely auth-gated.
- `context=edit` on any route — HTTP 401 `rest_forbidden_context`.
- `/.well-known/*` — HTTP 403 (nginx HTML) on every path probed.

## Common Properties

- [Website](https://www.denbury.com/)
- [Contact](https://www.denbury.com/contact-us/)
- [Terms and Conditions](https://www.denbury.com/terms-and-conditions/)
- [Privacy Policy](https://www.denbury.com/privacy-policy/)
- [LinkedIn](https://www.linkedin.com/company/denbury-resources)
- [Acquisition announcement (ExxonMobil)](https://corporate.exxonmobil.com/news/news-releases/2023/0713_exxonmobil-announces-acquisition-of-denbury)
- [SEC Filings (CIK 0000945764)](https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0000945764)
- [Notes](https://en.wikipedia.org/wiki/Denbury_Resources)

## Maintainers

- **Kin Lane** - kin@apievangelist.com
