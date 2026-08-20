---
id: sp-learning-2026-08-19-aws-subdomain-deploy
client_id: spatial-port
record_type: knowledge
service_path: software
status: proposed
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: internal
source_ref: manual://2026-08-19-knowledge-sync
schema_version: 1.1.0
created_at: 2026-08-19
updated_at: 2026-08-19
---
# Learning — Reusable AWS static-site deploy pattern (`*.spatial-port.io` subdomains)

- **Source class:** internal engineering practice (own deploy scripts + internal
  dashboard deploy, 2026). Sanitization check: contains no client-confidential
  data, no secrets, no account identifiers — only architecture and AWS-generic
  constants. Accepted-candidate reusable knowledge; requires Alex's acceptance
  before reuse.

## The pattern

Every demo, sales asset, or internal tool gets its own subdomain under
`spatial-port.io`, provisioned by a **one-time idempotent `setup-domain.sh`**
and published by a **re-runnable `deploy.sh`**. Time from folder of HTML to
live HTTPS subdomain: minutes of work plus CloudFront propagation.

**Canonical (hardened) architecture — used for the internal dashboard:**

1. **S3 bucket, private** (block public access ON) — one bucket per site.
2. **CloudFront distribution** with **Origin Access Control (OAC)** as the only
   reader of the bucket; viewer protocol `redirect-to-https`, compression on.
3. **ACM certificate in `us-east-1`** — mandatory region for CloudFront viewer
   certificates regardless of where the bucket lives; DNS-validated, the
   validation CNAME is UPSERTed into Route53 by the script.
4. **Route53 A-record ALIAS** subdomain → CloudFront domain (alias hosted zone
   `Z2FDTNDATAQYW2`, the global CloudFront constant).
5. **SPA fallback:** custom error responses mapping **403 and 404 →
   `/index.html` with HTTP 200**, so client-side routes deep-link correctly
   (S3 REST origins return 403, not 404, for missing keys — map both).

**Lightweight variant — public demo sites:** S3 static-website hosting +
public-read policy as a CloudFront *custom origin* (http-only to the website
endpoint), same ACM/Route53 steps. Simpler, fine for throwaway demos; prefer
the OAC variant for anything lasting.

## Idempotency rules (what makes the scripts safe to re-run)

- Look up existing resources before creating: cert by domain name in ACM,
  distribution by alias, hosted zone by apex name; cache the distribution id
  in a local `.distribution-id` file.
- All DNS writes are `UPSERT`, never `CREATE`.
- `deploy.sh` only syncs (`aws s3 sync --delete`, excluding scripts/dotfiles),
  re-asserts content-types (`text/html; charset=utf-8`, css, js — S3 sync can
  mis-detect), sets short `cache-control`, and invalidates `/*`.
- Plain-ASCII bash: macOS bash 3.2 mis-parses `$VAR` followed by unicode.

## Pitfalls (paid for once, do not repeat)

- **Dotless bucket rule:** a bucket named like `sub.domain.tld` breaks HTTPS on
  the S3 REST endpoint (the wildcard TLS cert `*.s3.amazonaws.com` cannot match
  dotted names). For OAC/REST origins use a **dotless bucket name** and point
  the CloudFront alias at the subdomain instead; the dashboard had to be
  migrated to a dotless bucket for exactly this reason (conversation record
  2026-08).
- **WAF pitfall:** WAF web ACLs for CloudFront must be created with
  **CLOUDFRONT scope in `us-east-1`**; and an IP-allowlist ACL is only for
  internal tools — attached to a public demo it silently serves 403 to every
  prospect. Verify from an off-allowlist network after attaching (internal
  waf-allowlist tooling, 2026; conversation record 2026-08).
- ACM in the wrong region is the most common first-run failure: the cert
  "exists" but CloudFront cannot see it.
- After changing origins or certs, always invalidate `/*` and re-test with a
  hard refresh; CDN caching hides both fixes and regressions for minutes.
