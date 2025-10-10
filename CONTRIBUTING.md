# Contributing to CitOmni

Thanks for your interest in improving CitOmni! We're obsessed with **low overhead**, **deterministic boot**, and **clean PHP 8.2+**.

> **Scope:** This guide applies **org-wide**. Individual repositories may add or override details in their own `CONTRIBUTING.md`. If anything here conflicts with a repo's local guide, the **repo's guide wins**.

---

## Repository types & licenses

CitOmni uses different licenses depending on purpose. Always check the repo's `LICENSE` (and `LICENSE-EXCEPTIONS` if present).

* **Runtime packages** (e.g. `citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure`)
  **License:** GPL-3.0-or-later **with the CitOmni Plugin Exception 1.0**
  *The Plugin Exception (GPL §7 "additional permission") allows separate "plugins/providers" to link against our public APIs under the author's chosen terms (including proprietary) so long as no substantial CitOmni code is copied.*

* **Provider skeletons** (e.g. `citomni/provider-skeleton`)
  **License:** GPL-3.0-or-later **with the CitOmni Template Exception 1.0**
  *The Template Exception lets you create providers from the skeleton and license the **resulting provider** under your chosen terms (including proprietary). The skeleton itself remains GPL.*

* **App/HTTP/CLI skeletons** (e.g. `citomni/http-skeleton`, `citomni/cli-skeleton`, `citomni/app-skeleton`)
  **License:** MIT
  *Skeleton code is intended to be copied into applications; MIT avoids copyleft friction.*

* **Private, proprietary repositories** (e.g. `citomni/auth`, `citomni/admin`)
  **License:** Proprietary (no redistribution).
  *These repos usually do **not** accept public contributions. See the repo README/`LICENSE`.*

If you contribute, you agree your contribution is made under the **specific repo's license** (and any stated exception). If you cannot accept that, please don't submit contributions.

---

## Developer Certificate of Origin (DCO)

We use the **DCO**. Sign your commits:

```
git commit -s -m "feat: ..."
```

This adds a `Signed-off-by: Your Name <you@example.com>` line declaring you have the right to submit the work.
DCO v1.1: [https://developercertificate.org/](https://developercertificate.org/)

---

## Contribution workflow (public repos)

1. **Discuss first (optional but helpful)**
   Open an issue or comment on an existing one to confirm scope and approach.

2. **Branch**
   Create a feature/fix branch from the latest default branch.

3. **Code with our standards** (see below)

4. **Validate locally**

   * `composer validate`
   * `composer dump-autoload -o`
   * If present:

     * `vendor/bin/phpunit`
     * `vendor/bin/phpstan analyse`

5. **Commit style**

   * Prefer **Conventional Commits**: `feat:`, `fix:`, `perf:`, `refactor:`, `docs:`, `test:`, `chore:`
   * Keep commits focused; write clear messages (What + Why).

6. **Open a Pull Request**

   * Fill out the PR template.
   * Confirm **license acceptance** and **DCO** checkboxes.
   * Describe any BC breaks or migrations.

7. **Review**

   * Maintainers may request changes for performance, determinism, or style.
   * We aim for small, mergeable PRs.

---

## Coding standards (all PHP repos)

* **Language:** PHP **8.2+** only
* **Autoloading:** PSR-4
* **Naming:** PSR-1 (PascalCase classes, camelCase methods/vars, UPPER_SNAKE_CASE constants)
* **Braces/Indentation:** **K&R** braces, **tabs** for indentation
* **Docs:** PHPDoc + inline comments in **English**
* **Error handling:** Do **not** swallow exceptions; let the global handler log (fail fast)
* **Determinism & performance:**

  * Prefer explicit maps/constants over scanning/reflection
  * Keep constructors/services light (lazy singletons)
  * Honor the kernel's deterministic merge model (vendor -> providers -> app -> env overlay)

---

## Tests & static analysis

* Add or update tests in `/tests` where applicable.
* Keep tests deterministic and minimal-no hidden I/O or time/date surprises.
* If the repo includes tooling:

  * `vendor/bin/phpunit` must pass
  * `vendor/bin/phpstan analyse` should pass at the configured level

---

## Security

* **Do not** file public issues for vulnerabilities.
* Report privately to: **[security@citomni.com](mailto:security@citomni.com)** (PGP/TLS optional)
* Include: affected repo, version/commit, reproduction steps, impact

We aim to acknowledge within 72 hours and provide a fix/mitigation timeline.

---

## Private/proprietary repositories

Some CitOmni repositories are **private** and **proprietary** (e.g. paid customer providers).
These typically **do not accept public contributions** and may have different internal workflows (Jira/Helpdesk).

* License: usually **proprietary (no redistribution)**
* Issues/PRs: disabled or restricted
* Security reports: use private channels listed in the repo

---

## Governance

Maintainers may edit for consistency, request changes, or decline contributions that increase complexity, reduce determinism, or regress performance. We value **clarity, determinism, and lean code**.

---

## Thank you

Every improvement-docs, tests, code, examples-helps the whole ecosystem.
Thank you for keeping CitOmni **fast, deterministic, and production-minded**.