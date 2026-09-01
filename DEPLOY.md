# Deploying csmsynergy.com

The site is hosted on **Cloudflare Workers** (static assets, no build step) on the
CSM Synergy account, Worker name `csmsynergy-site`.

## Deploy

```bash
npx wrangler deploy
```

That's it — but note:

> **Pushing to GitHub ships nothing.** There is no CI. Every deploy is manual,
> from a checkout of `main`, via `wrangler deploy`. Push AND deploy, always.

Config lives in `wrangler.jsonc`. URL rules are in `_redirects`, response
headers in `_headers`, files excluded from upload in `.assetsignore`.

## Adding a blog post

1. Copy an existing `blog-<slug>.html` at the repo root and edit it
   (title, meta description, canonical URL, body).
2. Add a rewrite line in `_redirects` (targets are extensionless — see the
   comments in that file):
   `/blog/<slug>    /blog-<slug>    200`
3. Add the post card/link to `blog.html`.
4. Commit, push, `npx wrangler deploy`.

## What about /admin?

`admin/` is a Decap CMS setup that depended on Netlify git-gateway. It is
parked: kept in the repo, excluded from deploy via `.assetsignore`. If
self-serve editing is ever wanted, it needs the GitHub backend plus a small
OAuth Worker.

## History

Migrated from Netlify on 2026-08-15. `netlify.toml` was removed; `_redirects`
intentionally mirrors the OBSERVED Netlify behavior, not a verbatim port
(see comments in `_redirects`). The old Netlify site was left up as a rollback
fallback and **was never deleted** — as of 2026-08-31 the `csmsynergy` Netlify site
still exists and still auto-builds `main` into a deploy nobody reads. It is scheduled
for deletion along with the rest of the Netlify account once CSM Studio's own
migration finishes its soak; csmsynergy.com itself has served from Cloudflare since
2026-08-15 and is unaffected either way.
