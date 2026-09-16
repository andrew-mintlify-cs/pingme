# PingMe docs — Mintlify enablement demo environment

A fictional notifications-platform company ("PingMe") used to demonstrate Mintlify features
in customer enablement sessions. Nothing here is a real product; all URLs, keys, and companies are invented.

## Deploy

1. Create a new GitHub repository and push these files to its default branch.
2. In the Mintlify dashboard, create a new organization (or deployment), connect the GitHub app,
   and select the repository. Mintlify builds from `docs.json` at the repo root.
3. Every push to the default branch deploys; every PR gets a preview deployment.

Local preview:

```bash
npm i -g mint
mint dev          # http://localhost:3000
mint validate     # strict build check, includes OpenAPI validation
mint broken-links
```

## Structure

```
docs.json                 Navigation, theme, API settings
index.mdx                 Landing page (Product tab)
product/                  Buyer-facing page (plans, security, FAQ)
learn/                    Explorer-facing pages (concepts guide, glossary, learn-more anchor)
v2/                       Developer pages for API v2 (default version)
v1/                       Developer pages for API v1 (legacy version)
openapi/core-v{1,2}.yaml  Core API specs -> auto-generated API reference pages
openapi/webhooks-v{1,2}.yaml  Webhook event specs (OpenAPI 3.1 `webhooks`) -> rendered via v*/webhook-events/*.mdx
v*/webhook-events/        One-line MDX stubs (`openapi: "openapi/webhooks-vX.yaml webhook <key>"`) per event
changelog.mdx             Reached via the global "Changelog" anchor; uses <Update>
internal/                 Two hidden pages (hidden: true): release runbook, internal pricing
images/, logo/, favicon.svg
TODO.md                   Planted imperfections for live-edit demos (do not fix before the session)
```

## Personas → where they land

| Persona | Entry point | Tab |
| --- | --- | --- |
| Developer implementing PingMe | `/v2/quickstart` → `/v2/build-a-welcome-sequence` → API reference playground | Developers |
| Buyer evaluating PingMe | `/` → `/product/plans-security-faq` | Product |
| Non-technical explorer | `/learn/how-notification-platforms-work` → glossary → "Learn more" anchor | Learn |

## Mintlify features exercised (screenshot checklist)

- Tabs (Product / Learn / Developers) and anchors inside a tab (Learn)
- Versions dropdown scoped to the Developers tab (v2 default/Latest, v1 Legacy)
- Two OpenAPI specs per version: Core API (paths, auto-generated group with `directory`) and Webhook Events (`webhooks`, wired page-by-page through frontmatter) — the two ways to attach a spec
- Interactive API playground with request/response examples from the spec (no mock server needed)
- Global anchors: Changelog (internal page), Status, Support, GitHub
- Hidden pages (`hidden: true`) for internal content
- Components: Card/CardGroup/Columns, Frame, Steps, Tabs, CodeGroup, Accordion/AccordionGroup, Note/Tip/Warning/Info/Check, Update (changelog with RSS)
- Docs-as-code: web editor → PR, IDE (`mint dev`) → PR, Claude + Admin MCP → PR (see TODO.md)
- AI: assistant, contextual menu, `/llms.txt`, Search MCP at `/mcp`, Admin MCP at `https://mcp.mintlify.com`

## Notes

- API playground `display` is `interactive`. Sends will fail (there is no server at api.pingme.dev); the
  request builder and the spec's response examples still render. Switch to `"display": "simple"` in
  `docs.json` if you prefer no Send button in screenshots.
- Generated API reference URLs follow `/<directory>/<tag>/<summary-slug>`, for example
  `/v2/api-reference/messages/send-a-message`. If Mintlify changes slug generation, update the links in
  `index.mdx` and the `v*/` guides (run `mint broken-links`).
