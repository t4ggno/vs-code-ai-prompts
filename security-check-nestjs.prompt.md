---
description: Perform a comprehensive NestJS security review
agent: agent
---
# Task: NestJS Security Check (Comprehensive)

Perform a thorough security sanity check of the NestJS codebase. Confirm that security coverage is complete, edge cases are handled, and nothing sensitive is exposed.

Use these NestJS‑specific and general references as guidance:
- NestJS docs: Controllers, Pipes/Validation, Guards, Authentication, Authorization, Exception Filters, Interceptors, Middleware.
- NestJS security docs: Helmet, CORS, CSRF, Rate Limiting, Encryption & Hashing.
- NestJS techniques: Serialization, Cookies, Session, File Upload.
- OWASP Secure Coding Practices and OWASP API Security Top 10 for API risks.

## Scope to Cover (NestJS‑focused)
Review all runtime‑reachable code paths and framework configuration, including:
- **Controllers**: routing, params, query, body, headers, @Res/@Req usage, passthrough behavior.
- **DTOs & Validation**: class‑validator rules, ValidationPipe options, pipes (Parse*), custom pipes.
- **Authentication**: JWT/session, password hashing, token lifetimes, secrets handling.
- **Authorization**: guards, roles/permissions, object‑level checks, metadata usage.
- **Guards/Interceptors/Filters/Middleware**: global vs scoped behavior, error mapping, logging.
- **Serialization**: ClassSerializerInterceptor, @Exclude/@Expose to avoid sensitive data leakage.
- **Security middleware**: Helmet, CORS, CSRF, rate limiting, proxy trust.
- **Files & uploads**: FileInterceptor, ParseFilePipe validators, storage & path safety.
- **External calls**: HTTP module usage, SSRF protections, timeouts.
- **Config & secrets**: ConfigModule, environment variables, secret stores, no hard‑coded keys.

## Step‑by‑Step Process
1. **NestJS Surface Inventory**
	- List all controllers, route prefixes, and HTTP methods.
	- Identify routes using `@Res()`/`@Req()`; verify `passthrough` where needed.
	- Confirm no debug or internal routes are exposed.

2. **DTOs & Validation (Pipes)**
	- Ensure DTOs are **classes**, not interfaces (runtime validation needs classes).
	- Verify `ValidationPipe` is applied globally or consistently per controller.
	- Check `ValidationPipe` options:
	  - `whitelist: true` and `forbidNonWhitelisted: true` for mass‑assignment defense.
	  - `transform: true` or explicit Parse* pipes for type coercion.
	  - `forbidUnknownValues: true` where applicable.
	  - `disableErrorMessages` for production if required.
	- Validate arrays with `ParseArrayPipe` or wrapper DTOs.
	- Validate custom decorators with `validateCustomDecorators: true` when needed.

3. **Authentication**
	- Verify password storage uses `bcrypt`/`argon2` (never plain text).
	- Ensure JWT secrets are **not** hard‑coded; sourced from config/secret store.
	- Check token expiry, refresh, rotation, and revocation strategy.
	- Confirm login endpoints return minimal data (no sensitive fields).

4. **Authorization & Guards**
	- Confirm object‑level authorization for all ID‑based routes (BOLA risk).
	- Verify function‑level authorization for admin/privileged endpoints.
	- Check guards use `Reflector` metadata correctly (`@Roles`, `@Public`, policies).
	- Ensure global guard behavior is intentional and public routes are explicit.

5. **Interceptors & Serialization**
	- Ensure `ClassSerializerInterceptor` (or equivalent) is used for DTO/entity output.
	- Verify `@Exclude()` on sensitive fields (passwords, tokens, secrets).
	- Check response mapping does not leak internal models or full entities.

6. **Exception Filters & Error Handling**
	- Confirm exceptions do not expose stack traces or secrets.
	- Ensure consistent error schema (custom filter if needed).
	- Validate security events are logged (auth failures, access denials).

7. **Security Middleware**
	- Helmet applied **before** routes (Express or Fastify setup).
	- CORS is explicit; avoid wildcard origins in production.
	- CSRF protection enabled where cookies/sessions are used.
	- Rate limiting with `@nestjs/throttler`; verify proxy `trust proxy` config.

8. **Sessions & Cookies**
	- Use secure cookie flags (Secure, HttpOnly, SameSite).
	- Ensure session store is not in‑memory in production.
	- Validate session secrets and rotation policy.

9. **File Uploads**
	- Enforce size/type limits via `ParseFilePipe` validators.
	- Validate file type by content (magic number), not extension.
	- Ensure uploads are stored outside executable paths.

10. **External Calls & SSRF**
	- Check HTTP clients enforce timeouts and allow‑lists.
	- Block internal IP ranges, metadata endpoints, and DNS rebinding.

11. **Edge Cases & Business Logic**
	- Validate pagination bounds, sorting allow‑lists, and filters.
	- Check request payloads with nested objects and arrays.
	- Confirm workflow state transitions are enforced server‑side.

12. **Verification & Coverage**
	- Map findings to OWASP API Top 10 (2023) and NestJS controls.
	- Ensure every NestJS security area above was reviewed.

## Required Output Format
Provide results in this order:
1. **Summary**: Risk posture and key NestJS‑specific themes.
2. **Findings**: Numbered list, each with:
	- Severity (Critical/High/Medium/Low)
	- Location (file/module/controller/route)
	- Evidence (exact behavior or code path)
	- Impact (attacker outcome)
	- Recommendation (clear fix steps)
	- Verification (how to validate the fix)
3. **NestJS Coverage Matrix**: Pipes/Validation, Guards, Authn/Authz, Filters, Interceptors/Serialization, Middleware, CORS/CSRF/Helmet, Rate Limiting, Files, Cookies/Sessions, External Calls.
4. **Unreviewed Areas**: Anything not covered and why.
5. **Assumptions & Open Questions**: Info needed to finalize risk.

## Constraints
- Do not modify code unless explicitly instructed; report issues and fixes instead.
- Be exhaustive and systematic. No high‑risk NestJS area can be skipped.

## Success Criteria
- All NestJS security areas above are assessed and recorded.
- OWASP API Top 10 (2023) risks are explicitly checked where applicable.
- Findings include evidence, impact, and actionable remediation steps.
- No sensitive exposure path (DTOs, serialization, logs, configs, endpoints) remains unchecked.
