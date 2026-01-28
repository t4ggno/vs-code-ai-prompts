---
agent: agent
---
Task: Perform a comprehensive security check of a Next.js codebase and report findings.

Scope
- Runtime-reachable paths only: routes, API routes, server actions, middleware, edge functions, webhooks, cron/queues.
- Authentication/authorization, session/token handling, RBAC/ABAC, multi-tenant isolation.
- Input validation, output encoding, and data shaping across server/client boundaries.
- Data access, ORM usage, raw queries, file uploads/downloads, storage integrations.
- External calls (fetch/HTTP clients, webhooks, third-party APIs), SSRF exposure.
- Configuration: headers, CSP, CORS, Next config, environment variables, secrets.
- Dependency/security posture: known vulnerable packages, lockfiles, build pipeline.

Requirements
- Enumerate the public attack surface: routes, methods, params, auth requirements, and data types.
- Verify every externally reachable endpoint enforces authn/authz and object-level access control.
- Validate all untrusted inputs (query, body, headers, cookies, file uploads) using allow-lists.
- Ensure output is minimized and sanitized; prevent excessive data exposure in API responses.
- Check for SSRF patterns in any user-supplied URLs or fetch calls.
- Check for open redirects, path traversal, and unsafe file handling.
- Verify secure cookie flags, session rotation, token validation, and CSRF protections where applicable.
- Review error handling for sensitive information leakage and stack traces.
- Confirm rate limiting, pagination caps, and resource limits for heavy endpoints.
- Verify Next.js-specific security posture (headers, CSP, middleware, route handlers, server actions, edge/runtime constraints).
- Identify improper inventory: undocumented endpoints, deprecated routes, debug endpoints.

Next.js-specific focus areas
- Route Handlers in `app/` and API routes in `pages/api/`.
- Server Actions and usage of `"use server"`.
- Middleware (`middleware.ts`) and its authn/authz enforcement.
- `next.config.*` security headers, image domains, redirects/rewrites.
- SSR/ISR/SSG data leakage through props, caching headers, and revalidation.
- `app` vs `pages` directory interaction, and shared utilities.
- Edge runtime restrictions and storage of secrets.
- `fetch` usage with credentials, redirects, and timeouts.
- Sensitive data in `public/` assets or generated JSON.

Constraints
- Do not modify code. Only report issues and recommended fixes.
- Be exhaustive and systematic; no high-risk area can be skipped.
- Use OWASP Secure Coding Practices checklist and OWASP API Security Top 10 (2023) as validation lenses.
- Reference Next.js best practices where relevant.

Deliverables (required format)
1) Summary: overall security posture and themes.
2) Findings: numbered list with severity, location, evidence, impact, recommendation, verification.
3) Coverage Matrix: checklist categories reviewed (OWASP SCP + API Top 10).
4) Unreviewed Areas: with rationale.
5) Assumptions & Open Questions: info needed to finalize risk.

Success Criteria
- Every OWASP Secure Coding Practices category is addressed.
- OWASP API Top 10 (2023) risks are explicitly checked against relevant endpoints.
- All findings include evidence, impact, and actionable remediation.
- No sensitive exposure is left unassessed (logs, errors, configs, endpoints).
