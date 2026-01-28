---
description: Perform a comprehensive React app security review
agent: agent
---
# Task: React Security Check (Comprehensive)

Perform a thorough security sanity check of the React application. Confirm that security coverage is complete, edge cases are handled, and nothing sensitive is exposed.

Use these best‑practice references as guidance:
- OWASP XSS Prevention Cheat Sheet (React escape hatches, safe sinks, DOM sanitization).
- OWASP CSRF Prevention Cheat Sheet (cookie‑to‑header for SPA, SameSite guidance).
- OWASP Content Security Policy Cheat Sheet (defense‑in‑depth against XSS).
- OWASP JWT Cheat Sheet (token storage, rotation, secret strength, revocation concepts).
- OWASP Top 10 (web app risks relevant to SPAs).

## Scope to Cover (React‑focused)
Review all runtime‑reachable client code paths and configuration, including:
- Components, hooks, state management, and routing (public vs protected views).
- Authentication, JWT handling, token refresh flows, and logout behavior.
- API clients (fetch/axios), headers, CSRF handling, and error handling.
- DOM rendering and sanitization (dangerouslySetInnerHTML, markdown, WYSIWYG).
- Build/runtime config (env vars, feature flags, public config exposure).
- Dependencies, bundler plugins, and third‑party scripts/widgets.
- Storage (localStorage, sessionStorage, cookies, in‑memory) and caching.

## Step‑by‑Step Process
1. **Attack Surface & Trust Boundaries**
	- Inventory public routes, protected routes, and admin views.
	- Identify where user‑supplied data enters the UI and how it is rendered.
	- List all external integrations (analytics, chat widgets, CDNs, APIs).

2. **Authentication & Session Design**
	- Confirm authentication flow uses HTTPS only.
	- Verify tokens are not hard‑coded in the app or exposed in public config.
	- Check login, logout, and session invalidation behavior.
	- Ensure access tokens are short‑lived and refresh logic exists.

3. **JWT Storage & Refresh**
	- Prefer HttpOnly, Secure, SameSite cookies for refresh tokens.
	- Avoid storing access tokens in localStorage if possible.
	- If access tokens are in memory or sessionStorage, ensure refresh rotation and replay protections.
	- Verify refresh flow handles expired/invalid tokens and clears state reliably.

4. **View Access & Route Guards**
	- Ensure protected routes enforce auth on every render (not only initial load).
	- Verify role/permission checks at the UI layer and match server rules.
	- Check for hidden‑UI‑only protections (must still enforce on server).

5. **XSS & DOM Safety**
	- Identify any usage of `dangerouslySetInnerHTML` and verify sanitization (e.g., DOMPurify).
	- Ensure markdown/WYSIWYG rendering is sanitized and locked down.
	- Avoid `eval`, `new Function`, and inline event handlers.
	- Validate all dynamic URLs (no `javascript:` or `data:` unless explicitly safe).

6. **CSRF & Cookie Security**
	- If cookies are used for auth, confirm CSRF protection (cookie‑to‑header pattern).
	- Ensure SameSite, Secure, and HttpOnly flags are set appropriately.
	- Verify state‑changing requests include CSRF headers.

7. **Content Security Policy (CSP)**
	- Check that CSP is defined (prefer response header; meta only if needed).
	- Ensure CSP is not overly permissive (`unsafe-inline`/`unsafe-eval` minimized).
	- Confirm third‑party scripts are allow‑listed and audited.

8. **API Client Hardening**
	- Ensure `withCredentials` is used only when necessary.
	- Verify request timeouts and retry policies avoid infinite loops.
	- Confirm sensitive headers are not logged or exposed in client errors.

9. **Error Handling & Logging**
	- Ensure client error messages do not leak secrets or internal API details.
	- Avoid logging tokens, cookies, PII, or full server responses.

10. **Dependency & Supply‑Chain Risks**
	- Check for vulnerable packages and unused dependencies.
	- Ensure lockfile integrity and restrict risky postinstall scripts.

11. **Edge Cases & Abuse Scenarios**
	- Validate behavior under token expiry, clock skew, and refresh failures.
	- Confirm race conditions in token refresh (single‑flight refresh logic).
	- Test navigation to protected routes with stale state or cached auth.

12. **Verification & Coverage**
	- Map findings to OWASP Top 10 and React‑specific risks (XSS, CSRF, token handling).
	- Ensure every area above was reviewed.

## Required Output Format
Provide results in this order:
1. **Summary**: Risk posture and key React‑specific themes.
2. **Findings**: Numbered list, each with:
	- Severity (Critical/High/Medium/Low)
	- Location (file/component/hook/route)
	- Evidence (exact behavior or code path)
	- Impact (attacker outcome)
	- Recommendation (clear fix steps)
	- Verification (how to validate the fix)
3. **React Coverage Matrix**: Auth flow, JWT storage/refresh, route guarding, XSS/DOM safety, CSRF, CSP, API client, logging, dependencies.
4. **Unreviewed Areas**: Anything not covered and why.
5. **Assumptions & Open Questions**: Info needed to finalize risk.

## Constraints
- Do not modify code unless explicitly instructed; report issues and fixes instead.
- Be exhaustive and systematic. No high‑risk React area can be skipped.

## Success Criteria
- All React security areas above are assessed and recorded.
- OWASP XSS/CSRF/CSP/JWT guidance is explicitly checked where applicable.
- Findings include evidence, impact, and actionable remediation steps.
- No sensitive exposure path (tokens, DOM rendering, storage, logs, configs) remains unchecked.
