# Security Policy

We take security seriously and value responsible disclosure. This document explains how to report vulnerabilities, how we triage, and how we communicate fixes.

## Supported Versions

Security fixes target **actively maintained** release lines as listed on each repository's Releases page. Older lines may receive best-effort patches only if the fix is low-risk and widely applicable.

- Runtime packages (MIT): `citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure`
- Proprietary org packages: `citomni/auth`, `citomni/testing` (distributed via certified resellers)
- Docs & org meta: `citomni/docs`, `citomni/.github` (CC BY-SA 4.0)

> Note: CitOmni has **no direct customers**; proprietary packages are delivered **only via certified resellers**.

## Reporting a Vulnerability

Please do **not** open public issues for security problems.

- Email: **security@citomni.com**
- Optional (preferred for sensitive details):
  - **PGP fingerprint:** `FILL-ME-IN`
  - **PGP key download:** https://citomni.com/security/pgp.asc

Include (as applicable):
- A clear description and potential impact
- A minimal, deterministic PoC (steps, config, payload)
- Affected package(s), versions, environment details (PHP version, OS, web server)
- Any logs, stack traces, or screenshots

### If your report involves:
- **Proprietary org packages** (`citomni/auth`, `citomni/testing`): Mention which reseller and deployment version you're testing.
- **Secrets exposure** (tokens, keys): Redact in email; use PGP; if you must provide a live sample, minimize privileges and lifetime.
- **Supply chain risk** (typosquatting, dependency confusion, compromised artifact): Provide IOC hashes, package names, and registry links.

## Triage & Timeline

We aim to:
- **Acknowledge** your report within **72 hours**
- Provide an **initial assessment** (valid/not reproducible/out of scope) within **7 days**
- For valid issues, propose a **mitigation plan** and indicative timeline

These are goals, not guarantees; complex issues may require longer investigation.

## Severity & Scoring

We use **CVSS v3.1** (and v4.0 when appropriate) to score impact and set patch priority.

Indicative response targets:
- **Critical** (e.g., RCE, auth bypass): patch or mitigation target **7-14 days**
- **High** (e.g., privilege escalation): **14-30 days**
- **Medium/Low**: included in the next scheduled release

> Where feasible, we provide **configuration-only mitigations** (e.g., tightening routes, headers, or provider maps) for immediate risk reduction.

## Fix Process

1. Reproduce, scope, and root-cause the issue.
2. Prepare patches for supported branches; add tests where possible.
3. Verify no new regressions (unit/integration, static analysis).
4. Coordinate with **certified resellers** if proprietary org packages are affected.
5. Release patched versions and a security advisory.

### Advisories & CVEs

- We publish a **GitHub Security Advisory (GHSA)** with CVSS score, affected versions, and remediation steps.
- CVE IDs are requested via GitHub or the appropriate CNA when relevant.

## Coordinated Disclosure

We support coordinated disclosure and may request a short **embargo** (typically ≤14 days for critical issues) to allow time for patches to propagate. We credit reporters by handle/name unless you prefer anonymity.

### Notification Channels

- Release notes and GHSA
- Repository SECURITY.md updates (if process-relevant)
- Direct notifications to affected resellers for proprietary packages

## Scope

**In scope** (examples):
- Authentication/authorization bypasses
- Remote code execution, injection, deserialization issues
- Cross-site scripting (XSS), request smuggling, CSRF in CitOmni components
- Privilege escalation via mis-validated configuration merges
- Supply-chain vectors impacting CitOmni packages (published artifacts, tags)

**Out of scope** (examples):
- Third-party vulnerabilities in your own applications
- Issues requiring non-default, unreasonable configurations
- Denial-of-service from unrealistic resource exhaustion without a practical exploit
- Vulnerabilities in dependencies already covered by their upstream advisories (we will track and bump)
- Marketing site content, non-security typos, or broken links

## Responsible Testing

- Do not exfiltrate real data, escalate beyond your own resources, or affect other users.
- Respect rate limits and legal boundaries. Use local or disposable test environments.
- If you inadvertently access sensitive data, **stop**, document minimally, and report privately.

## Safe Harbor

We will not pursue or support legal action for **good-faith** security research that:
- follows the guidance in this policy,
- avoids privacy violations and service disruption,
- and provides us a reasonable time to remediate.

## Hardening & Best Practices

- Keep PHP **≥ 8.2**, enable **OPcache**, avoid dynamic reflection scans.
- Prefer deterministic provider maps and explicit routes; minimize I/O in constructors.
- Set a strict **Content-Security-Policy** and sane default headers at the app/web-server layer.
- In prod/stage, define `CITOMNI_PUBLIC_ROOT_URL` **or** set absolute `http.base_url` (see README).
- Use **read-only config wrappers**; never mutate `cfg` at runtime.

## Contact & Keys

- Security: **security@citomni.com**
- Trademarks misuse: **trademarks@citomni.com**
- PGP: fingerprint `FILL-ME-IN`, key: https://citomni.com/security/pgp.asc

_Thank you for helping keep the CitOmni ecosystem safe._
