---
agent: agent
---
# Task: General Security Check (Comprehensive)

Perform a thorough security sanity check of the codebase. Confirm that the security coverage is complete, edge cases are handled, and nothing sensitive is exposed.

Use these best‑practice references as guidance and ensure your review aligns with them:
- OWASP Secure Coding Practices Quick Reference Checklist (input validation, output encoding, authn/authz, session management, crypto, logging, data protection, config, DB, file handling, general coding).
- OWASP Code Review Guide (methodology and checklist-driven manual review).
- OWASP API Security Top 10 (2023) for API-specific risks (BOLA, broken auth, resource consumption, SSRF, misconfig, improper inventory, unsafe consumption, etc.).
- Secure code review best practices (prioritize high-risk code, integrate automated scans, dependency analysis, continuous feedback).

## Scope to Cover
Review all runtime‑reachable code paths, including:
- Public and internal API endpoints, controllers, handlers, webhooks, scheduled jobs, CLI tasks.
- Authentication, authorization, and role/permission checks.
- Data access layers, ORM/DB queries, migrations, raw SQL.
- File upload/download, static assets, templating, serialization.
- External integrations (HTTP clients, queues, storage, third‑party APIs).
- Configuration, environment variables, secrets, and infrastructure‑as‑code.

## Step‑by‑Step Process
1. **Preparation & Threat Model**
	- Identify primary assets (PII, credentials, tokens, financial data, business‑critical operations).
	- Map trust boundaries (client ↔ API, API ↔ DB, API ↔ third parties, internal services).
	- List high‑risk modules (auth, admin, billing, user management, file handling, integrations).
	- Define abuse cases and attacker goals (data exfiltration, privilege escalation, DoS, SSRF).

2. **Attack Surface Inventory**
	- Enumerate all externally reachable routes and operations.
	- Confirm each route has correct authn/authz.
	- Ensure deprecated/debug/internal endpoints are not exposed.
	- Confirm API inventory is complete and documented.

3. **Input Validation & Canonicalization**
	- Validate all untrusted inputs server‑side (headers, query, body, files).
	- Use allow‑lists for types, ranges, lengths, and formats.
	- Normalize/encode inputs before validation to prevent obfuscation.
	- Verify pagination, sorting, and filtering parameters are bounded.

4. **Authorization & Access Control**
	- Verify object‑level and function‑level authorization for every endpoint.
	- Check for BOLA/BFLA: access by ID must enforce ownership/permission.
	- Ensure role/permission checks are centralized and consistent.
	- Confirm multi‑tenant isolation (no cross‑tenant reads/writes).

5. **Authentication & Session/Token Security**
	- Confirm strong authentication flows and secure password handling.
	- Validate token/session lifetime, rotation, revocation, and reuse prevention.
	- Ensure sensitive operations require re‑auth or step‑up auth.
	- Disallow auth tokens in URLs; enforce secure cookie attributes where applicable.

6. **Injection & Output Handling**
	- Check for SQL/NoSQL/LDAP/OS command injection risks.
	- Ensure parameterized queries and safe ORM APIs are used.
	- Verify safe output encoding for any data reflected to clients.
	- Ensure template rendering and serialization cannot leak secrets or internals.

7. **Data Protection & Secrets**
	- Ensure sensitive data is encrypted in transit and at rest.
	- Check for hard‑coded secrets or secrets in logs.
	- Verify least‑privilege DB/service accounts and scoped credentials.
	- Ensure data retention, deletion, and masking policies exist where needed.

8. **Error Handling & Logging**
	- Confirm errors do not leak stack traces or sensitive info.
	- Ensure logs include security‑relevant events (auth failures, access denials).
	- Validate logs do not include secrets/PII; ensure log integrity.

9. **Rate Limiting & Resource Abuse**
	- Check for protections against brute force, enumeration, and DoS.
	- Validate request size limits, pagination caps, and timeouts.
	- Ensure expensive operations have safeguards (queues, throttles, quotas).

10. **SSRF & External Calls**
	- Validate and restrict outbound requests (allow‑list domains/IPs).
	- Enforce timeouts, DNS rebinding protection, and block internal IP ranges.

11. **File Handling & Serialization**
	- Validate file type by content, not extension; scan uploads when required.
	- Prevent path traversal; store uploads outside executable paths.
	- Review deserialization for unsafe types and gadget chains.

12. **Configuration & Dependency Risks**
	- Check for insecure defaults, debug flags, overly permissive CORS.
	- Verify TLS configuration, security headers, and secure cookies.
	- Review dependencies for known vulnerabilities and license risks.

13. **Edge Cases & Business Logic**
	- Test negative, boundary, and large inputs.
	- Validate state transitions and workflows cannot be bypassed.
	- Ensure idempotency where required and protect against replay.

14. **Verification & Follow‑up**
	- Correlate findings with OWASP Secure Coding Practices checklist.
	- Ensure OWASP API Top 10 risks are explicitly addressed.
	- Provide remediation steps and, where possible, test ideas to verify fixes.

## Required Output Format
Provide results in this order:
1. **Summary**: High‑level risk posture and notable themes.
2. **Findings**: A numbered list, each with:
	- Severity (Critical/High/Medium/Low)
	- Location (file/module/function/route)
	- Evidence (exact behavior or code path)
	- Impact (what an attacker gains)
	- Recommendation (clear fix steps)
	- Verification (how to validate the fix)
3. **Coverage Matrix**: Show which checklist categories were reviewed.
4. **Unreviewed Areas**: Anything not covered and why.
5. **Assumptions & Open Questions**: Info needed to finalize risk.

## Constraints
- Do not modify code unless explicitly instructed; report issues and fixes instead.
- Be exhaustive and systematic. No high‑risk area can be skipped.

## Success Criteria
- Every OWASP Secure Coding Practices checklist category is addressed.
- OWASP API Top 10 (2023) risks are explicitly checked for relevant endpoints.
- All findings include evidence, impact, and actionable remediation.
- No sensitive exposures are left unassessed (logs, errors, configs, endpoints).
