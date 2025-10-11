Here's the fully updated, production-ready `CONTRIBUTING.md` with the refinements we discussed, ready to drop in as the canonical version (either in `citomni/.github/CONTRIBUTING.md` or mirrored into `citomni/docs/contribute/contributing.md` if you keep a docs copy).

---

# Contributing to CitOmni

Thanks for helping make CitOmni faster, clearer, and more sustainable. This guide explains how we structure work across the organization, what we expect in code and docs, and how to land changes without surprising anyone (including future-you).

CitOmni's philosophy: **explicit > magic, deterministic > accidental, performance > ornamentation.**

---

## Scope

These rules apply to **all repositories** in the CitOmni organization and affiliated packages (core and providers):

* Core: `citomni/kernel`, `citomni/http`, `citomni/cli`
* First-party providers under `citomni/*`
* Public skeletons, examples, and docs
* Third-party providers that integrate with CitOmni (recommended)

Where a repo has extra/local conventions, they must not contradict this document.

---

## Code of Conduct

Be kind, precise, and constructive. Focus critique on code/ideas, not people. If something's unclear, ask; if something's clever, document why. For private reports (security, conduct), see **Security**.

---

## Architecture Primer (read before changing things)

CitOmni runs in exactly **two runtime modes**: **HTTP** and **CLI**. Each mode owns its **baseline** via constants in its package (e.g., `\CitOmni\Http\Boot\Config::CFG`, `\CitOmni\Http\Boot\Services::MAP`).

**Merge order (per mode HTTP|CLI):**

1. Mode baseline (constants in `citomni/http` or `citomni/cli`)
2. Providers (in `/config/providers.php` order)
3. App base config (`citomni_http_cfg.php` or `citomni_cli_cfg.php`)
4. App env overlay (`citomni_http_cfg.{ENV}.php` or `citomni_cli_cfg.{ENV}.php`)

**Artifacts:**

* `$app->cfg` - deep, last-wins; associative arrays merged; lists replaced.
* `$app->services` - ID->class map, **left-wins at each merge step**.

**Providers** don't own baselines; they contribute overlays via `src/Boot/Services.php` (`CFG_HTTP/CFG_CLI`, `MAP_HTTP/MAP_CLI`, optional routes via `Boot/Routes::MAP`). **Constants-only; no runtime work in `Boot/*`.**

If your change violates these invariants, it's probably in the wrong layer.

---

## Language, Runtime & Style

* **Language:** English for all docs, PHPDoc, inline comments, READMEs, commit messages.
* **PHP:** 8.2+ only; **every PHP file MUST begin with** `declare(strict_types=1);`.
* **Style:** PSR-1/PSR-4; **K&R braces** (opening brace on same line); **tabs** for indentation; lean SPDX header.
* **Naming:** Classes `PascalCase`; methods/vars `camelCase`; constants `UPPER_SNAKE_CASE`.
* **Comments:** Explain **why**, not just **what**; keep them short and near the code they justify; sanitize docs (ASCII-only where required).

---

## Green Coding & Performance (first-class)

CitOmni aims for **low CPU time, low RAM footprint, minimal I/O**, and stable latencies. It's faster and greener, and fully compatible with our deterministic boot model (constant arrays, zero reflection/I/O during merge).

### Principles

* **Do less work:** Remove allocations, copies, and needless layers on hot paths.
* **Prefer constants & caches:** Use constant arrays and OPcache-friendly precompiled caches; avoid reflection/autowiring/DI containers.
* **Minimize I/O:** Batch DB calls, avoid N+1s, and never touch disk on hot paths; keep routes/config in constant maps.
* **Short code paths:** Predictable branching; early returns; avoid "helper carousels".

### Default budgets (per HTTP request, warm cache)

(Repositories may set stricter local budgets if needed.)

* Wall time (app code): ≤ **12 ms** p95
* CPU time (user+sys): ≤ **8 ms** p95
* RSS: ≤ **1.6 MB** p95
* FS ops: ≤ **2** synchronous reads (excluding OPcache)
* DB round-trips: target **1-2**; **hard cap ≤ 5** (document exceptions)

### Required PR evidence (when touching hot paths)

If you change boot, routing, service resolution, or model I/O, add a **Perf Note** in the PR with:

* **Scenario** (e.g., "HTTP /route X with Y records")
* **Measurements:** p50/p95 wall (ms), CPU (ms), `memory_get_usage`/peak (bytes), FS/DB counts
* Cold vs warm notes (include first run separately)
* What you did to reduce CPU/RAM/I/O

### Measurement recipe (quick)

* Use `hrtime(true)` around sections; log when `?_perf` is set (dev).
* Log CPU ms (`getrusage` if available) and current/peak memory.
* Take 100-500 warm iterations (local loop or wrk/ab), report p50/p95.
* **Always measure with OPcache enabled (production-like).**
* When relevant, run `App::warmCache()` (or the mode-specific warmer) before "warm" iterations.

### DB & I/O rules

* No N+1 queries; prefer `IN (...)`, simple joins, preloading.
* Cap queries per request (see budget).
* No disk I/O on hot paths; use **constant arrays** read once (OPcache).

### PHP patterns that help

* Return arrays/scalars on hot paths; minimize transient objects.
* Prefer guard clauses and simple control flow.
* Avoid `preg_*` / `sprintf` in tight loops if a cheaper alternative exists.

### Routing & services

* Router tables are **constant maps**; no dynamic building at runtime.
* Service resolution is **ID -> class** via the map; no reflection/autowiring.

### Templates & output

* Keep templates light: no DB calls, no disk I/O; prepare data in controller/service.

---

## Providers (quick rules)

* `src/Boot/*` are **constants-only** (no side effects at require-time; **no I/O/env reads/function calls**).
* Expose overlays via `src/Boot/Services.php` constants: `CFG_HTTP`, `CFG_CLI`, `MAP_HTTP`, `MAP_CLI` (+ optional `Boot/Routes::MAP` for HTTP).
* **No** `Boot/Config.php` in providers (baselines are reserved for `citomni/http` and `citomni/cli`).
* **Constructor contract** for services:
  `__construct(\CitOmni\Kernel\App $app, array $options = [])` (options = scalars/arrays only).
* Keep constructors cheap; defer I/O to first use.
* Register provider FQCNs explicitly in **`/config/providers.php`** (array order = merge order).

**Provider repository layout (typical):**

```
/src/          PSR-4 source (see Boot/ below)
  /Boot/       Services.php (REQUIRED), Routes.php (optional, HTTP)
  /Service/    Provider services
  /Model/      Provider models (if any)
  /Controller/ HTTP controllers (if HTTP provider)
  /Command/    CLI commands (if CLI provider)
  /Exception/  Exceptions
/tests/        PHPUnit tests (recommended)
README.md
LICENSE
.github/workflows/ (optional)
```

Providers **must not** ship app-level `/config` or runtime `/var` directories; those exist only in the consuming application.

---

## Tooling & Local Setup

* **Composer:** `composer dump-autoload -o` for optimized autoloading.
* **Static analysis:** PHPStan (level **6 or higher**, unless repo specifies stricter).
* **Tests:** PHPUnit 11+; prefer small, deterministic tests.
* **Formatting:** tabs; no trailing spaces; no mass reformatting in unrelated PRs.
* **Perf-smoke:** repos SHOULD include `tools/perf-smoke.php` that:

  * boots once (cold) then ~200 warm iterations,
  * prints p50/p95 wall/CPU, current/peak memory, FS/DB counts,
  * returns non-zero exit if budgets are exceeded.

Providers should depend only on what they actually need: `citomni/kernel`, and add `citomni/http` and/or `citomni/cli` only if required. Keep the surface minimal.

---

## Branching, Commits, and PRs

### Branching

Use short, scoped prefixes:

* `feat/<slug>` - new capability
* `fix/<slug>` - bug fix
* `perf/<slug>` - performance improvement
* `refactor/<slug>` - no behavior change
* `docs/<slug>` - documentation only
* `chore/<slug>` - tooling/infra

### Commit messages

* Imperative: "Add router warm cache", "Fix 503 headers".
* Keep diffs focused; avoid drive-by cleanups.
* Reference issues where relevant.

### Pull Request checklist

Before requesting review, verify:

* [ ] PHP 8.2+, `declare(strict_types=1);` at top of every file.
* [ ] Tabs/K&R/PSR-1/PSR-4 respected; lean SPDX header present.
* [ ] **No side effects** in `src/Boot/*` (constants-only; no I/O/env/func calls).
* [ ] **No reflection/autowiring or directory scans** in runtime paths.
* [ ] **Perf Note** added for hot-path changes; budgets respected (or variance justified).
* [ ] Config/service changes follow merge rules; migration notes included if user-visible.
* [ ] Public IDs/keys unchanged, or treated as **breaking** with SemVer rationale (see below).
* [ ] Tests updated/added and pass locally; static analysis passes.
* [ ] Documentation updated (README, PHPDoc) and sanitized (ASCII where required).

---

## Backwards Compatibility & SemVer

**Public surface (ABI):**

* **Service IDs** (map keys)
* **Top-level config keys** (first-level keys under `$app->cfg->*`)
* **Route path keys** (exact path strings in the routes map)

Changing or removing any of the above is a **MAJOR** change.
For deprecations: introduce the new key/ID first; keep the old working with a warning on **first use**; document migration; remove in the next major.

---

## Testing Guidance

Keep tests fast and deterministic; avoid filesystem/network in unit tests.

For providers, test:

* Service resolution (incl. app overrides) and constructor contract.
* Route maps (controller class/methods, metadata) and path-level overrides.
* "No side effects on require" for `Boot/Services.php` (constants-only).

See the CitOmni Testing Guide for structure and helpers.

---

## Documentation Contributions

* Update READMEs and guides together with code changes.
* When referencing architecture/behavior, link to the canonical docs (runtime modes, provider semantics, conventions).
* Keep examples copy-pasteable and valid PHP.

---

## Security

**Do not** open public issues for vulnerabilities.

Email the maintainers at **[security@citomni.com](mailto:security@citomni.com)** (or the private address in the org profile) with:

* Clear description and minimal PoC
* Affected packages/versions
* Impact assessment (confidentiality/integrity/availability)
* Suggested remediation if known

We'll coordinate a private fix and disclosure.

---

## Licensing

* Each repo declares its license via a `LICENSE` file and SPDX headers; keep headers **short** and accurate as per Conventions.
* Each repository declares its license via LICENSE and SPDX headers. By contributing, you agree your contribution is licensed under that repository’s LICENSE.
* Third-party providers must declare their own licenses and ensure compatibility with CitOmni usage and trademarks.

---

## Getting Review

* Small PRs get reviewed faster.
* If your change touches core boot/merge paths or public IDs/keys, expect deeper review and possibly a short design note (a few paragraphs is fine).
* Be responsive to feedback; if a discussion stalls, summarize options with pros/cons to move forward.

---

## Release Notes

When your change is user-visible (new service IDs, config keys, routes, CLI commands, etc.), add a concise changelog entry:

* **Added/Changed/Fixed/Deprecated/Removed**
* One-line description
* Example config/service override if relevant

---

## FAQ

**Q:** Can a provider define `Boot/Config.php`?
**A:** No. Baselines live only in mode packages (`citomni/http`, `citomni/cli`). Providers contribute overlays via constants on `Boot/Services`.

**Q:** Can I use reflection/autowiring for service resolution?
**A:** No. Use explicit **service IDs -> classes** in the map; the constructor contract is fixed for determinism.

**Q:** How do I override a provider service?
**A:** Reuse the same service ID in the application’s service map overlay (e.g., **/config/services.php**). Merge is deterministic: the application layer overrides upstream (left-wins per merge step).

---

**Two modes, one philosophy: explicit, deterministic, fast.** CitOmni does not guess. It knows.
