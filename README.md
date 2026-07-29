# gate-site

Validation landing page for **Gate** — the multi-model AI gateway with one
private memory that follows you across every model.

**Live:** https://gate.jamgate.dev

This repo is the public marketing surface only. The product design, roadmap,
decision log and full backlog live in the private product repo
[`amirj4m/gate`](https://github.com/amirj4m/gate) under `docs/`.

> **Status (2026-07-29): live, but not yet ready for paid traffic.** The page
> works, the demo works, and the pre-order and waitlist are wired — but there is
> **no analytics or conversion pixel on the site**, the Stripe account is still
> KYC-pending, and the legal pages are unreviewed. See
> [Before running ads](#before-running-ads).

---

## What's here

| File | Purpose |
| --- | --- |
| `index.html` | The whole landing page — markup, styles and logic in one file, including the `CONFIG` object. |
| `demo.html` | Interactive **scripted** demo: switch models mid-chat, watch the memory persist. This is how the moat is sold. |
| `privacy.html` | Privacy policy. Real text (placeholders removed) but **not legally reviewed**. |
| `terms.html` | Terms. Same caveat. |
| `og-image.png` | Social preview card for link shares. |
| `CNAME` | `gate.jamgate.dev` — required by GitHub Pages for the custom domain. |

There is no build step, no framework and no dependencies. Open `index.html` in a
browser and what you see is what ships.

## Configuration

Everything tunable lives in **one `CONFIG` object** near the bottom of
`index.html`. This is deliberate — it is the only place to edit, so a rebrand or
a link swap is a one-line change and never a find-and-replace across the page.

| Key | What it does |
| --- | --- |
| `BRAND` | Product name, applied to every `[data-brand]` element at runtime. |
| `DOMAIN` | Public domain; also the fallback for the contact address. |
| `STRIPE_PAYMENT_LINK` | Stripe Payment Link for the pre-order. Leave `""` and the buttons stay visible but inert — safe by default. |
| `WAITLIST_FORM_ENDPOINT` | Formspree POST endpoint. Leave `""` and the form falls back to opening the visitor's mail client. |
| `CONTACT_EMAIL` | Contact address; falls back to `hello@DOMAIN`. |
| `TWITTER_URL` | Optional; the social link stays hidden while empty. |
| `PRICE` | Pre-order price shown on the pricing card. |

No secrets belong in this repo — every value above is public by nature, since
the file is served to visitors as-is.

## Deployment

Push to `main`. GitHub Pages serves the repo root; there is nothing to build.

**DNS:** `gate.jamgate.dev` is a Cloudflare record in **DNS-only mode** (grey
cloud), so GitHub issues and validates the TLS certificate itself rather than
Cloudflare proxying it. Proxying it (orange cloud) would break GitHub's
certificate validation — leave it grey.

Verified 2026-07-29: valid HTTPS, and `http://` returns a 301 to `https://`.

## Before running ads

Ad spend is blocked on the items below. The authoritative, detailed checklist —
with acceptance criteria for each — is
[`docs/BACKLOG.md`](https://github.com/amirj4m/gate/blob/main/docs/BACKLOG.md) in
the product repo. Summary, site-scoped:

1. **No analytics or pixels.** Nothing is measured. Needs an analytics tool plus
   at least one ad pixel, added to **both** `index.html` and `demo.html`, with
   `preorder_click`, `waitlist_submit` and `demo_model_switch` instrumented.
2. **Formspree owner confirmation not clicked** — waitlist notifications may not
   reach the inbox. Confirm, test end-to-end, then delete the test submissions.
3. **Stripe account provisional / KYC-pending** — the wired link may be
   test-mode and take no real money. Don't advertise a paid pre-order until a
   real transaction has been proven.
4. **The founding price is not stated.** The card promises a "discounted founding
   rate, locked for life" without a number; only the pre-order price is concrete.
5. **Legal pages unreviewed** — and they must be updated in the same change that
   adds analytics, since we sell privacy.
6. **Claude reseller position unresolved.** Claude appears ~8× in `index.html`
   and in the demo's model picker, but Anthropic's commercial terms restrict
   reselling without a contract. Either sort the terms or mark Claude
   "coming soon" before charging anyone for it.

## Changelog

- **2026-07-28** — Interactive demo added at `/demo.html`; Claude added to the
  model lineup across the copy and the demo picker; pre-ads QA pass (OG image,
  fixed a fake "Last updated" date, replaced placeholder legal text).
- **2026-07-27** — Rebranded Continuum → **Gate**; `CNAME` set to
  `gate.jamgate.dev`; Stripe pre-order link and Formspree waitlist endpoint
  wired into `CONFIG`.

## Conventions

Docs-scale changes are committed straight to `main` — this is a single-author,
no-CI static site. Substantive product decisions are not recorded here; they go
in `DECISIONS.md` in the product repo.
