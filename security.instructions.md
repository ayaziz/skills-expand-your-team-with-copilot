
# Security Instructions

This document contains brief, actionable security guidance for the repository. Focus areas: avoiding hardcoded secrets, preventing SQL injection, and reducing common application risks.

## Avoid Hardcoding Tokens / Passwords
- Never commit tokens, API keys, passwords, or private certificates into the repository.
- Use environment variables or a secrets manager (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, Google Secret Manager).
- For local development, keep secrets in a `.env` file and add `.env` to `.gitignore`.
- Do not print secrets to logs or expose them in error messages.
- Rotate credentials immediately if a secret is accidentally committed; revoke and reissue keys/tokens.
- To remove secrets from history use tools like `git filter-repo` or the BFG Repo-Cleaner, and rotate credentials afterwards.

## Prevent SQL Injection
- Never construct SQL queries by concatenating or interpolating raw user input.
- Always use parameterized queries / prepared statements. Example (psycopg2 style):

  Bad:

  SELECT * FROM users WHERE username = '" + username + "'

  Good:

  cursor.execute("SELECT * FROM users WHERE username = %s", (username,))

- Use your database library's parameter binding (the placeholder syntax varies by driver).
- Prefer using a well-maintained ORM or query builder that parameterizes queries automatically.
- Apply least privilege to DB accounts: use a specific user with only required permissions for the app.
- Validate and constrain inputs (types, length, allowed characters) — use whitelisting rather than blacklisting.
- Use stored procedures with parameters where appropriate and reviewed.

## Input Validation and Output Encoding
- Validate inputs on server-side (never rely only on client-side checks).
- Use strict types, length limits, and whitelists for fields such as IDs, filenames, emails, etc.
- Encode output for the target context (HTML escape for pages, JSON encode for APIs) to prevent XSS.

## Other Common Risks and Mitigations
- Cross-Site Scripting (XSS): escape user content before rendering, use Content Security Policy (CSP).
- Authentication & Session: use secure, HttpOnly cookies for session tokens, implement proper session expiration and rotation.
- Transport Security: enforce HTTPS/TLS for all external and internal traffic.
- Sensitive Data in Logs: redact or omit sensitive fields (passwords, tokens, PII) from logs and monitoring traces.

## CI/CD and Secret Management
- Store secrets in the CI runner's secure variable store (do not echo them in build logs).
- Add automated secret scanning in CI (tools like `git-secrets`, `truffleHog`, or GitHub's secret scanning).
- Include a pre-commit hook to detect accidental secrets before commits are pushed.

## Dependency & Configuration Hygiene
- Keep dependencies up to date and monitor CVEs for used libraries.
- Run SCA (software composition analysis) regularly.

## Response Plan for Exposed Secrets
1. Revoke the exposed secret immediately (tokens, keys, credentials).
2. Replace with a rotated secret and update dependent systems.
3. Identify scope of exposure (which commits, branches, services) and remove secret from history.
4. Audit access logs for suspicious activity and notify stakeholders.

## Helpful Practices
- Use principle of least privilege everywhere (services, DB users, cloud IAM roles).
- Prefer managed services for key management if operationally possible.
- Review security-relevant code during PR reviews and include security checklist items.

## Applies to 
applyTo: "**"