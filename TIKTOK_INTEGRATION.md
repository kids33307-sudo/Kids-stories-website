# Kids Stories Automation — TikTok integration guide

## Current implementation (5 November 2026)

This repository is a **static informational website**, not an OAuth implementation or a publishing service.

| Component | File / deployment | Current state |
|---|---|---|
| Project homepage | `index.html` | Present |
| Privacy policy | `privacy.html` | Present |
| Terms of service | `terms.html` | Present |
| OAuth authorization endpoint | Backend (not in this repo) | Not implemented |
| OAuth redirect handler | Backend HTTPS endpoint (not in this repo) | Not implemented |
| Encrypted token storage | Backend (not in this repo) | Not implemented |
| Video upload/publish worker | Backend (not in this repo) | Not implemented |
| Explicit creator approval gate | Backend (not in this repo) | Not implemented |

## Expected GitHub Pages URLs

Pages must be enabled separately in repository settings, using the `main` branch and `/(root)` directory. Confirm each URL loads via HTTPS before entering it into TikTok Developers.

- Website: https://kids33307-sudo.github.io/Kids-stories-website/
- Privacy: https://kids33307-sudo.github.io/Kids-stories-website/privacy.html
- Terms: https://kids33307-sudo.github.io/Kids-stories-website/terms.html

## Secure OAuth authorization flow (backend required)

1. An **adult account owner** starts an authorized connection on the backend.
2. Backend constructs the TikTok authorization URL with registered `client_key`, exact HTTPS `redirect_uri`, requested scopes, cryptographically random `state`, and PKCE parameters where required by the platform's current documentation. Store state / verifier securely with short expiration.
3. Browser redirects the creator to TikTok, where they authenticate and approve permissions.
4. TikTok redirects to the exact registered callback, returning `code` and `state`.
5. Backend verifies `state` before exchanging the short-lived code at TikTok's token endpoint. Keep `client_secret` solely in backend secret storage, not this public GitHub repo, HTML, client-side JavaScript, logs, or chat.
6. Backend encrypts and stores token metadata and refresh tokens scoped to the authorized TikTok account. Do not treat authentication tokens as a publishing authorization.
7. Backend refreshes tokens server-side as needed, handles revocation/expiry, and uses minimum requested scopes.
8. When a video is ready, the account owner explicitly approves the exact media, caption, target account, visibility/options, and posting action **before each publish**.
9. The backend checks actual approved app permissions and current TikTok Content Posting API requirements, then calls the permitted upload/direct-post endpoints and records the status. No publishing should occur if approval or scope is absent.
10. Provide a disconnect function that revokes authorization where supported and deletes locally held credentials.

### Request flow

```text
Adult creator --> Backend /connect --> TikTok authorize/consent
TikTok --> HTTPS backend callback ?code&state
Backend --> Validate state --> TikTok token exchange
Backend --> Encrypted token store
Creator --> Review specific video + explicit approval
Backend --> TikTok Content Posting API (only with approved scopes)
TikTok --> Publishing/upload status --> Backend status log
```

## TikTok Developers checklist

- Use the **existing** Kids Stories Automation app, not a duplicate.
- Verify GitHub Pages URL accessibility and configure website, Privacy Policy and Terms of Service.
- Configure an actual HTTPS backend callback and register its **exact** redirect URI. **Do not use the static website as a token-exchange backend.**
- Add Content Posting API product if offered.
- Request `video.upload` for upload/draft workflows, and `video.publish` only if direct posting is supported and the app qualifies. Scope availability and approval are controlled by TikTok.
- Provide honest product use case, screen recording / walkthrough, and required review materials. Do not claim unimplemented features work.
- Confirm current TikTok rules for testing, audit, unaudited client restrictions, permitted visibility and publishing experience. No production direct posting until approved.
- Ensure no post goes live without human approval of the specific video.

## Credentials and security rules

- Never commit `client_secret`, access tokens, refresh tokens, authorization codes, service-account credentials or plaintext environment files.
- Use a secrets manager / protected backend environment. Restrict credentials to the backend, encrypt at rest, limit access, audit token usage, and rotate/revoke on compromise.
- Do not put OAuth code exchange, refresh tokens or platform API secrets in GitHub Pages JavaScript.
- Metricool's existing connection to TikTok/YouTube is separate from granting OAuth permissions to this TikTok Developer app; it does not automatically provide this app with tokens or scopes.

## Next dependency

The current repository has **no backend**, so a real callback URL and functional TikTok Content Posting integration cannot be completed by editing these static pages alone. Deploy a secure backend, then use its exact redirect URI in the developer console. Submission for platform review remains subject to TikTok's developer console and human approval where required.
