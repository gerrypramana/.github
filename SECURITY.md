# Security Policy

**Please do not file public issues for security vulnerabilities.**  
Report privately so we can triage and fix quickly.

---

## Reporting a Vulnerability

- Email: **security@citomni.com**
- Subject: `SECURITY: <project> <version/commit>`
- Include:
  - Affected repository (e.g., `citomni/kernel`, `citomni/http`, ...)
  - Version/commit hash
  - Impact summary (what can an attacker do?)
  - Reproduction steps / PoC
  - Environment details (PHP version, OS, web server)
  - Your contact for follow-up

Optional (preferred for sensitive details):
- **PGP**: (Public key fingerprint + download link here if/when available)

We acknowledge reports within **72 hours** and aim to provide a fix or mitigation plan within **14 days**, depending on severity and complexity.

---

## Scope

This policy covers repositories in the **CitOmni** GitHub organization.

- **Public, open-source repos** (e.g., `citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure`, skeletons) - report via **security@citomni.com**.
- **Private/proprietary repos** (e.g., paid customer providers) - if you have access, use the **private channels** listed in the repo's README or your support contract; otherwise email **security@citomni.com**.

---

## Coordinated Disclosure

We follow **coordinated disclosure**:
1. We confirm receipt and begin triage.
2. We work with you to reproduce and assess impact.
3. We develop and test a fix/mitigation.
4. We release patched versions and **credit the reporter** (if desired).
5. We may publish an advisory with remediation guidance.

Please do not publicly disclose the issue until we release a fix or agree on a timeline together.

---

## Safe Harbor

We will not pursue legal action for **good-faith** security research that:
- respects privacy and data protection laws,
- avoids data destruction, service disruption, or degradation,
- and gives us a reasonable time to remediate before public disclosure.

Testing **must not** involve social engineering, physical access, or use of stolen credentials.

---

## Supported Versions

We generally provide security fixes for the **latest minor** of each maintained major line.  
At minimum, expect security support for:
- the current **stable** series, and
- the most recent **previous** series, where applicable.

Exact support windows may vary by repository; see each repo's README or release notes.

---

## Out of Scope

- Vulnerabilities in **third-party dependencies** (report upstream where appropriate).
- Issues without a security impact (please open a normal GitHub issue).
- Findings that require unrealistic preconditions (e.g., full admin on host).

---

## Contact & Updates

- Primary: **security@citomni.com**
- Project updates: follow the repositories' release notes or changelogs.

Thank you for helping keep CitOmni secure.
