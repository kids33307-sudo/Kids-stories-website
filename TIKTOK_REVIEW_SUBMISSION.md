# Kids Stories Automation — TikTok Developer review preparation

**Prepared:** 2026-11-05  
**Status:** Draft, NOT a submitted TikTok review. The GitHub repository currently contains an informational static website only.

## Existing app to update
- App name: Kids Stories Automation
- TikTok creator account: @kidschannal2
- Website: https://kids33307-sudo.github.io/Kids-stories-website/
- Privacy Policy: https://kids33307-sudo.github.io/Kids-stories-website/privacy.html
- Terms of Service: https://kids33307-sudo.github.io/Kids-stories-website/terms.html
- Contact: kids33307@gmail.com

**Validation required:** Confirm all Pages URLs return publicly accessible HTTPS pages, not a GitHub 404, before pasting them in TikTok Developers. GitHub Pages settings cannot be modified using the currently available GitHub connector.

## Purpose / app description (English)
Kids Stories Automation is an in-development publishing workflow for the adult operator of Kids Stories educational animated-video channels. The operator creates or selects original, appropriately licensed videos, reviews the exact video and caption, chooses the destination account, and explicitly approves each publishing action. The tool intends to use TikTok's official Content Posting API only after consent and review permissions have been granted. No child account registration or child personal-data collection is planned.

## Requested TikTok product and scopes
- **Content Posting API** — request activation in the existing developer app.
- **video.upload** — request only for an actual upload or draft workflow if offered.
- **video.publish** — request only when direct posting is supported, the workflow is functional, and the app qualifies for the requisite review.
- Add Login Kit/appropriate authorization product if the current TikTok console requires it for this implementation.
- Scope names, available products, audit requirements, and eligibility must be verified in TikTok's actual console. These are requests, not granted permissions.

## Proposed review walkthrough (record *only when functionality exists*)
1. Adult operator signs in and clicks Connect TikTok.
2. Show TikTok consent screen with requested scopes.
3. Return to a real deployed HTTPS backend callback with state validation; no credentials visible in recording.
4. Select a sample original Kids Stories video and preview caption/account/visibility.
5. Show explicit per-video approval before upload or publish.
6. Show approved API interaction and status, with sensitive tokens redacted.
7. Show error handling, disconnect and deletion/revocation behavior.

Do not record a fake integration or claim features work before they are deployed.

## OAuth architecture required before submitting a functional integration
```text
Adult operator -> Backend /connect -> TikTok authorize/consent
TikTok -> HTTPS /oauth/tiktok/callback?code=...&state=...
Backend -> Verify state -> Token exchange using server-only client secret
Backend -> Encrypted per-account token/refresh-token storage
Operator -> Review exact video + destination + caption + approve
Backend -> TikTok Content Posting API using approved scopes only
TikTok -> Backend status -> Operator
```

- Exact redirect URI must refer to a real HTTPS backend endpoint. **Do not invent or register a GitHub Pages callback**: this static hosting cannot securely exchange or store OAuth tokens.
- Use server-side short-lived state and PKCE where applicable under TikTok's current specifications.
- Client Secret and access/refresh tokens must never appear in public GitHub source, frontend JavaScript, logs, screenshots, or chat.
- Metricool's TikTok/YouTube connections are not TikTok OAuth grants to this developer application.

## Remaining console actions (NOT completed)
1. Verify GitHub Pages is enabled and HTTPS URLs are publicly reachable.
2. Open the already-created Kids Stories Automation app in TikTok Developers.
3. Add the website, Privacy Policy and Terms of Service URLs.
4. Enable Content Posting API, request scopes supported by the app.
5. Deploy a secure backend and enter its **actual** redirect URI.
6. Complete any app verification, functional demo, and TikTok review submission.
7. Test auth and uploads only within allowed app/testing limits.
8. Never publish a video without explicit owner approval for that individual video.
