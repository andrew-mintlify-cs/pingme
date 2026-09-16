# Planted imperfections for live-edit demos

These are intentional. Each maps to one contribution surface in the enablement deck.
Do not fix them before the session. Reset the repo from the initial commit to reuse.

## 1. Stale rate-limit numbers — fix via Claude + Admin MCP

**Where:** `v2/errors-and-rate-limits.mdx`, section "Rate limits" (table, headers example, and 429 example).
**Current (stale) values:** Starter 30, Growth 300, Enterprise "Custom — typically 3,000+";
`RateLimit-Limit: 300`; error message "exceeded 300 requests per minute".
**Correct values** (already correct on `product/plans-security-faq.mdx` and in `openapi/core-v2.yaml`):

```json
{
  "effective": "2026-09-01",
  "limits_per_minute": { "starter": 60, "growth": 600, "enterprise": "custom, typically 6,000+" },
  "example_headers": { "RateLimit-Limit": 600, "RateLimit-Remaining": 587, "RateLimit-Reset": 41 },
  "example_error_message": "You have exceeded 600 requests per minute. Retry after 12 seconds."
}
```

Demo: save the JSON above as `rate-limits-2026-09.json`, attach it in Claude with the Admin MCP connected,
and ask: "Update the rate-limit table and examples in v2/errors-and-rate-limits.mdx to match this file and open a PR."
Point out that the same page in `v1/` intentionally keeps the old table (v1 limits did not change).

## 2. Typos in the Quickstart — fix via the web editor

**Where:** `v2/quickstart.mdx`
- "Set your **kay** as an environment variable" → key
- "You'll use it in **the the** next step" → the
- "You'll **recieve** a `202 Accepted`" → receive
- "reports progress **seperately**" → separately

Demo: open the page in the Mintlify web editor, fix, publish → PR → preview deployment → merge.

## 3. Missing TypeScript example — add via IDE (`mint dev`)

**Where:** `v2/build-a-welcome-sequence.mdx`, step "Send the welcome message immediately".
The CodeGroup has `curl` and `Python` tabs only. Add a `TypeScript` tab:

```ts TypeScript
const res = await fetch("https://api.pingme.dev/v2/messages", {
  method: "POST",
  headers: {
    Authorization: `Bearer ${process.env.PINGME_API_KEY}`,
    "Content-Type": "application/json",
    "Idempotency-Key": `welcome-${user.id}`,
  },
  body: JSON.stringify({
    recipient_id: recipient.id,
    template_id: templates.welcome,
    channels: ["email", "push"],
    data: { first_name: user.firstName, workspace: user.workspaceName },
  }),
});
const welcome = await res.json();
```

Demo: clone, `mint dev`, edit the file in VS Code/Cursor, show hot reload, commit on a branch, open PR.
Optionally repeat on `v2/quickstart.mdx` step 2 to show the same change across pages.

## Optional extras

- `docs.json` → `api.examples.languages` is `["curl", "python"]`. Adding `"javascript"` regenerates
  code samples on every endpoint page — a one-line change with a large visible effect, good for the Admin MCP demo.
- The changelog is reached only through the global anchor. Adding it as a fourth tab is a 6-line `docs.json` change
  that demonstrates navigation editing.
