# Index Overview

| Field | Value |
|---|---|
| Status | `CURRENT` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Index/index.html`](../../../Just_Cook_Index/index.html); [`Just_Cook_Index/legal.html`](../../../Just_Cook_Index/legal.html); [`Just_Cook_Index/nginx.conf`](../../../Just_Cook_Index/nginx.conf); [`Just_Cook_Index/Dockerfile`](../../../Just_Cook_Index/Dockerfile); [`Just_Cook_Index/compose.yml`](../../../Just_Cook_Index/compose.yml) |

## Purpose

`Just_Cook_Index` is the public static website. It introduces the product and
links visitors to the application; it is not part of the authenticated recipe
runtime.

## Current behavior

The monolithic `index.html` contains inline CSS and JavaScript, SEO metadata,
product text, pricing and credit content, and local images. Its JavaScript
handles mobile navigation, before/after sliders, credit popovers, and the
displayed year. Login and registration links point to `app.just-cook.net`.

The legal page loads the legal notice, privacy policy, terms and conditions,
and cancellation policy at runtime from external script and HTML sources. The
page therefore depends on third-party JavaScript, cross-origin fetches, and the
availability of the legal content provider.

## Nginx boundary

Nginx serves the root, `/index`, `/index.html`, `/legal`, and `/legal.html`.
The shortcuts `/impressum` and `/datenschutz` return HTTP 302 redirects to
fragments of `/legal`. Unknown paths use the internal `/404.html`. Selected
static assets receive a one-year immutable cache header.

The container exposes HTTP port `80`; Compose maps host port `8080` to it. Nginx
does not terminate TLS, proxy the API, or provide authentication.

## SEO and product claims

Canonical, Open Graph, Twitter, and JSON-LD metadata are present. No
`robots.txt`, sitemap, or defined social preview image was visible. The legal
page is `index,follow` even though its content is loaded externally.

Pricing, credits, and sharing claims in the public page are not visibly backed
by billing or entitlement enforcement in the application. Their final product
status is `OPEN`.

## Further reading

- [Project overview](../01-overview/project-overview.md)
- [Frontend overview](../05-frontend/overview.md)
- [Deployment](../08-deployment/deployment.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English Index service overview |
