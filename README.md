# CitOmni

**Low overhead. High performance. Ready for anything.**

CitOmni is a modular PHP framework (PHP 8.2+) designed for ultra-fast execution, ultra-low overhead, and no surprises.
It is built around small Composer packages - HTTP, CLI, Kernel, Infrastructure, Auth, Testing, and more - so you only install what you need.

---

## Why CitOmni?

* 🚀 **Deterministic boot** - predictable "last wins" config merges (vendor -> providers -> app).
* 🧩 **Composable design** - slim packages you can combine freely.
* 🛡️ **Transparent, no magic** - no reflection, no runtime/hidden scanning, just clean PSR-4.
* 🛠️ **Developer-friendly** - deep read-only config, instant service maps, cache-ready.
* 🔒 **Privacy by default** - no telemetry, no phone-home; only what you explicitly enable runs.
* ♻️ **Green by design** - lower memory use and CPU cycles -> less server load, more requests per watt, better scalability, smaller carbon footprint.

---

### Green by design

CitOmni's "Green by design" claim is empirically validated at the framework level.

The core runtime achieves near-floor CPU and memory costs per request on commodity shared infrastructure, sustaining hundreds of RPS per worker with extremely low footprint.

See the full test report here:
[CitOmni Docs -> /reports/2025-10-02-capacity-and-green-by-design.md](https://github.com/citomni/docs/blob/main/reports/2025-10-02-capacity-and-green-by-design.md)

---

## Get Started


### Quick start (HTTP app)

```bash
# 1) Scaffold a new app from the HTTP skeleton
composer create-project citomni/http-skeleton my-app
cd my-app

# 2) Ensure entrypoint defines the standard CitOmni constants (public/index.php)
<?php
declare(strict_types=1);

define('CITOMNI_START_NS', hrtime(true));
define('CITOMNI_ENVIRONMENT', 'dev');            // dev | stage | prod
define('CITOMNI_PUBLIC_PATH', __DIR__);          // no trailing slash
define('CITOMNI_APP_PATH', \dirname(__DIR__));   // app root, no trailing slash

// In stage/prod: either define CITOMNI_PUBLIC_ROOT_URL OR set absolute http.base_url in config.
// In dev: Kernel will auto-detect if not defined.
if (\defined('CITOMNI_ENVIRONMENT') && \CITOMNI_ENVIRONMENT !== 'dev') {
	define('CITOMNI_PUBLIC_ROOT_URL', 'https://www.example.com');
}

require __DIR__ . '/../vendor/autoload.php';

// Pass the public path; Kernel resolves app root + /config internally.
\CitOmni\Http\Kernel::run(__DIR__, [
	// options reserved (none required)
]);

// Alternative: call with a config directory (Kernel resolves app root/public)
// \CitOmni\Http\Kernel::run(CITOMNI_APP_PATH . '/config');

# 3) Run the built-in PHP server
php -S 127.0.0.1:8080 -t public

# 4) Visit http://127.0.0.1:8080
```

Tip: append ?_perf to any URL in dev to print execution time, memory, and included files.

**PUBLIC_ROOT_URL rule**

- **dev**: may be auto-detected (you can omit CITOMNI_PUBLIC_ROOT_URL).
- **stage/prod**: you **must** either:
  1) define `CITOMNI_PUBLIC_ROOT_URL`, **or**
  2) set an absolute `http.base_url` in `/config/citomni_http_cfg.{ENV}.php`.

**Locale setup**

Kernel reads `locale.timezone` and `locale.charset` from your config
(`/config/citomni_http_cfg*.php`) and applies them at boot
(`date_default_timezone_set`, `ini_set('default_charset', ...)`).


### Quick start (CLI app)

```bash
# 1) Create a new project (using the kernel + cli packages)
composer init -n && composer require citomni/kernel citomni/cli

# 2) Make a minimal bin entrypoint
cat > bin/app <<'PHP'
#!/usr/bin/env php
<?php
require __DIR__ . '/../vendor/autoload.php';
\CitOmni\Cli\Kernel::run(__DIR__ . '/../config', $argv);
PHP
chmod +x bin/app

# 3) Create a minimal config/ folder
mkdir -p config && printf "<?php\nreturn [];" > config/citomni_cli_cfg.php
printf "<?php\nreturn [];" > config/services.php
printf "<?php\nreturn [];" > config/providers.php

# 4) Run
./bin/app list
```

> Issues: use the prefilled templates under **New Issue** (bug, feature, docs, perf, refactor). Blank issues are disabled.


---

## Documentation

- **Runtime / Execution Mode Layer** - architectural rationale for HTTP vs CLI, baseline ownership, deterministic config/service merging, and why CitOmni deliberately supports only two execution modes.  
  _Doc:_ [`concepts/runtime-modes.md`](https://github.com/citomni/docs/blob/main/concepts/runtime-modes.md)

- **Provider Packages: Design, Semantics, and Best Practices** - explains how provider packages contribute `MAP_*` and `CFG_*` definitions, routes, precedence rules, and versioning; includes guidance on testing, consistency, and conflict avoidance.  
  _Doc:_ [`concepts/services-and-providers.md`](https://github.com/citomni/docs/blob/main/concepts/services-and-providers.md)

---

### Ecosystem map

| Package                                                      | Purpose (one-liner)                              |
|--------------------------------------------------------------|--------------------------------------------------|
| [`citomni/kernel`](https://github.com/citomni/kernel)        | Deterministic core: config + service map         |
| [`citomni/http`](https://github.com/citomni/http)            | Lean HTTP runtime (router, request/response)     |
| [`citomni/cli`](https://github.com/citomni/cli)              | Fast CLI runtime (commands, scheduling)          |
| [`citomni/infrastructure`](https://github.com/citomni/infrastructure) | Cross-mode services (shared utilities)   |
| [`citomni/auth`](https://github.com/citomni/auth)            | Authentication & account management              |
| [`citomni/testing`](https://github.com/citomni/testing)      | Integration testing in a real CitOmni boot       |
| [`citomni/http-skeleton`](https://github.com/citomni/http-skeleton) | Starter template for HTTP apps          |


More packages are available under the [CitOmni organization](https://github.com/citomni).

---

## Architecture at a glance

```

App (your code)
├─ /config
│   ├─ providers.php           # whitelist of provider boot classes
│   ├─ services.php            # app overrides for service map
│   ├─ routes.php              # app routes (included from citomni_http_cfg.php)
│   ├─ citomni_http_cfg*.php   # app & env overlays (HTTP)
│   └─ citomni_cli_cfg*.php    # app & env overlays (CLI)
└─ /src

Vendor packages
├─ citomni/kernel              # builds config + services (deterministic)
├─ citomni/http                # HTTP baseline config + services
├─ citomni/cli                 # CLI baseline config + services
└─ providers (auth, etc.)      # contribute MAP_*/CFG_* constants

Runtime
├─ Mode::HTTP -> Http\Boot\Config::CFG + Services::MAP
└─ Mode::CLI  -> Cli\Boot\Config::CFG  + Services::MAP

```

Routes live in `/config/routes.php` and are included from `citomni_http_cfg.php`. 
Providers may also contribute route maps via their `Boot\Services::CFG_HTTP`.

---

## Versioning & Support

- **PHP:** CitOmni targets **PHP ≥ 8.2** (no support for older versions).
- **SemVer:** Public APIs (class names, method signatures, config/merge behavior, service IDs, top-level config keys) follow **Semantic Versioning**.
- **Stability:** Core packages aim for conservative changes; breaking changes are announced in release notes with migration guidance.

---

## Security

If you discover a security issue, please **do not** open a public issue.  
See [SECURITY.md](SECURITY.md).  
Email: **security@citomni.com** (PGP optional). We acknowledge within ~72h and coordinate a fix.

---

## Telemetry & Privacy

CitOmni performs **no telemetry** and ships **no phone-home** code.  
What runs is exactly what you install and opt-in via `/config/providers.php`.

---

## Philosophy

CitOmni is built around three principles:

1. **Low overhead** - no bloat, no wasted CPU, memory, or I/O.
2. **High performance** - predictable runtime, cache-friendly design, production-ready.
3. **Ready for anything** - build webapps, APIs, intranet tools, or CLI automation without fighting the framework.

---

## 💚 Green by Design

CitOmni is engineered for **software-level efficiency**: it reduces CPU cycles, memory allocations, and redundant I/O at the framework layer. The architecture favors **deterministic execution paths**, **static service/config maps**, and **OPcache-friendly compiled arrays** so the CPU does only the work you explicitly request. This design aligns with best-available findings that software efficiency remains a first-order lever for controlling data-centre energy growth and infrastructure needs [1,2,3].

### Architectural pillars (technical)

1. **Deterministic boot (no runtime scanning/reflection):** predictable control flow and cacheable hot paths minimize instruction count and branch entropy.
2. **Memory-lean resolution:** static maps (IDs -> classes/options) instead of dynamic discovery reduce allocations, hash lookups, and GC pressure.
3. **OPcache-centric design:** compiled arrays and preload-friendly structures cut repeated parsing and improve instruction locality.
4. **Side-effect-free boot:** no implicit I/O, no telemetry, no background tasks; each request is explicit and reproducible.

### Why this matters (operational & embodied)

Even small per-request savings **compound at scale**. Lower framework overhead reduces:

* **Operational energy** (CPU time -> joules), improving *work per watt* and server density [1,2].
* **Cooling load**, as less heat is produced per unit work [1].
* **Hardware churn (embodied carbon)**, because higher density and longer useful lifetimes defer new procurements [4].

These effects are widely noted in sector overviews and meta-analyses: efficiency gains at the software/runtime layer have historically offset demand growth and remain essential as workloads (including AI) expand [1-3].

### Measured outcomes (qualified)

On modest shared hosting with PHP 8.2+ and OPcache enabled, CitOmni's framework-layer overhead exhibits consistently **low CPU time** and **low memory footprint** (including template render), with request-level measurements clustering near the platform's timing floor under warm-cache conditions. Exact figures depend on environment (CPU, TLS, payload, network). Full methodology, scripts, and reproducible datasets are provided here:

-> **Report:** *Capacity & Green-by-Design* (2025-10-02)
[https://github.com/citomni/docs/blob/main/reports/2025-10-02-capacity-and-green-by-design.md](https://github.com/citomni/docs/blob/main/reports/2025-10-02-capacity-and-green-by-design.md)

### Scope & reproducibility

* Metrics: wall-time (p50/p95), CPU time (where available), RSS/peak memory, included files.
* Conditions: warm caches, OPcache on, no telemetry, fixed payload sizes; environment parameters documented in the report.
* Repro: scripts and configs included; results are meant to be re-run on your target hardware.

> **Bottom line:** Fast software is green software. By minimizing overhead at the framework layer and favoring deterministic, cache-friendly execution, CitOmni translates design choices directly into **measurable operational efficiency** and **lower embodied impact over time**.

---

### References

[1] **IEA (2022)** - *Data Centres and Data Transmission Networks*: sector energy use, efficiency trends, and drivers.
[https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks](https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks)

[2] **Masanet et al. (2020, Science)** - *Recalibrating global data center energy-use estimates*: shows how efficiency gains (including software) historically constrained energy growth despite demand.
[https://doi.org/10.1126/science.aba3758](https://doi.org/10.1126/science.aba3758)

[3] **IEA 4E (2025)** - *Data Centre Energy Use: Critical Review of Models and Results*: methodological review highlighting model spread and the ongoing role of efficiency at multiple layers.
[https://www.iea-4e.org/wp-content/uploads/2025/05/Data-Centre-Energy-Use-Critical-Review-of-Models-and-Results.pdf](https://www.iea-4e.org/wp-content/uploads/2025/05/Data-Centre-Energy-Use-Critical-Review-of-Models-and-Results.pdf)

[4] **Meta (2024)** - *Estimating embodied carbon in data-centre hardware*: why extending hardware life and reducing churn matters (embodied emissions).
[https://sustainability.atmeta.com/blog/2024/09/10/estimating-embodied-carbon-in-data-center-hardware-down-to-the-individual-screws](https://sustainability.atmeta.com/blog/2024/09/10/estimating-embodied-carbon-in-data-center-hardware-down-to-the-individual-screws)

[5] **IEA (2024)** - *Energy and AI (Executive Summary)*: workload growth outlook and efficiency context for digital infrastructure.
[https://www.iea.org/reports/energy-and-ai/executive-summary](https://www.iea.org/reports/energy-and-ai/executive-summary)

[6] **LBL/DOE (resource hub)** - *Data Center Efficiency Research*: background on measurement, workloads, and efficiency levers (including software).
[https://datacenters.lbl.gov/](https://datacenters.lbl.gov/)

---

## Community & Governance

- **Contributing:** see [CONTRIBUTING.md](CONTRIBUTING.md)
- **Code of Conduct:** see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- **Security:** report privately via [SECURITY.md](SECURITY.md)
- **Licensing overview:** see [LICENSING.md](LICENSING.md)
- **Trademarks:** see [TRADEMARKS.md](TRADEMARKS.md)

---

## Contributing

* Code style: PHP 8.2+, PSR-1/PSR-4, **tabs**, K&R braces.
* Naming: PascalCase classes, camelCase methods/variables, UPPER_SNAKE_CASE constants.
* Documentation: PHPDoc and inline comments must be in **English**.
* SPDX headers in code files (e.g., `SPDX-License-Identifier: MIT`).
* Keep vendor files side-effect free (OPcache-friendly).
* Don't swallow exceptions in core; let the global error handler log.

---

## Coding & Documentation Conventions

All CitOmni projects follow the shared conventions documented here:
[CitOmni Coding & Documentation Conventions](https://github.com/citomni/docs/blob/main/contribute/CONVENTIONS.md)

---

## License

CitOmni uses different licenses depending on repository purpose. See each repo's `LICENSE`.

- **Official framework packages** (kernel, http, cli, infrastructure, testing, auth): **MIT**  
- **Skeletons** (HTTP/CLI/app/provider skeletons): **MIT**  
- **Documentation** (this repo and `.github` policies/templates): **CC BY-SA 4.0**  
- **Private or client-specific extensions**: **Proprietary** (no redistribution)

Details and rationale: see [LICENSING.md](LICENSING.md).

---

## Trademarks

"CitOmni" and the CitOmni logo are trademarks of **Lars Grove Mortensen**.  
You may make factual references to "CitOmni", but do not modify the marks, create confusingly similar logos,  
or imply sponsorship, endorsement, or affiliation without prior written permission.  
Do not register or use "citomni" (or confusingly similar terms) in company names, domains, social handles, or top-level vendor/package names.  
For details, see the project's [NOTICE](NOTICE).

---

## Author

Developed and maintained by **[Lars Grove Mortensen](https://github.com/LarsGMortensen)** © 2012-present
Contributions and pull requests are welcome!

---

Built with ❤️ on the CitOmni philosophy: **low overhead**, **high performance**, and **ready for anything**.
