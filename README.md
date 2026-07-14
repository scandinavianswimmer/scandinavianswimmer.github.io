# DATEMAXXING Legal Site Bundle

Status: draft / founder-reviewed / public-host pending
Updated: 2026-06-26

This directory is a static legal-page bundle for the release-managed public
legal host configured in the app:

- `/terms`
- `/privacy`
- `/support`
- `/child-safety`

The files are plain HTML and CSS so they can be deployed by any static host
without a build step. The App Store release gate treats this as content
prepared, not hosted. The separate `public_legal_urls` gate must remain blocked
until the configured public legal host returns HTTP 200 for `/terms`,
`/privacy`, `/support`, and `/child-safety`.

`_headers` and `_redirects` are included for static hosts that support
Cloudflare Pages/Netlify-style deployment metadata. Hosts that do not support
those files can ignore them, but the four public paths still need to resolve.

## Policy Sources Checked

- Apple App Review Guideline 1.2 requires UGC apps to include filtering,
  reporting, blocking, and published contact information:
  https://developer.apple.com/app-store/review/guidelines/
- Apple App Privacy Details require a publicly accessible privacy policy URL:
  https://developer.apple.com/app-store/app-privacy-details/
- Google Play Child Safety Standards require dating/social apps to publish CSAE
  standards, provide in-app feedback, address CSAM, comply with law, and name a
  child-safety point of contact:
  https://support.google.com/googleplay/android-developer/answer/14747720

## Deployment Shape

Deploy the contents of `legal-site/` as the site root for the current public
legal host.

Expected paths:

- `legal-site/index.html` -> `<legal-host>/`
- `legal-site/terms/index.html` -> `<legal-host>/terms`
- `legal-site/privacy/index.html` -> `<legal-host>/privacy`
- `legal-site/support/index.html` -> `<legal-host>/support`
- `legal-site/child-safety/index.html` -> `<legal-host>/child-safety`

## Verification

After deployment, run:

```bash
node scripts/verify-legal-urls.mjs
```

To test a staging host or local server:

```bash
LOOKS_LEAGUE_LEGAL_BASE_URL=http://127.0.0.1:8765 node scripts/verify-legal-urls.mjs
```

## Before Publication

- Attorney review of Terms and Privacy Policy.
- Confirm company legal name/address.
- Confirm final provider DPA, retention, subprocessors, model-training, and data
  residency terms for Supabase, FaceTec, Anthropic, OpenAI, and Apple.
- Confirm the named child-safety point of contact in the relevant store console.
- Re-run the release gate and public URL verification after deployment.
