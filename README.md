# CitOmni

**Low overhead. High performance. Ready for anything.**

CitOmni is a modular PHP framework (PHP 8.2+) designed for ultra-fast execution, zero overhead, and no surprises.
It is built around small Composer packages - HTTP, CLI, Kernel, Infrastructure, Auth, Testing, and more - so you only install what you need.

---

## Why CitOmni?

* 🚀 **Deterministic boot** - predictable "last wins" config merges (vendor -> providers -> app).
* 🧩 **Composable design** - slim packages you can combine freely.
* 🛡️ **Transparent, no magic** - no reflection, no hidden scanning, just clean PSR-4.
* 🛠️ **Developer-friendly** - deep read-only config, instant service maps, cache-ready.
* 🔒 **Privacy by default** - no telemetry, no phone-home; only what you explicitly enable runs.
* ♻️ **Green by design** - lower memory use and CPU cycles -> less server load, more requests per watt, better scalability, smaller carbon footprint.

---

### Green by design

CitOmni's "Green by design" claim is empirically validated at the framework level.

The core runtime achieves near-floor CPU and memory costs per request on commodity shared infrastructure, sustaining hundreds of RPS per worker with extremely low footprint.

See the full test report here:
[CitOmni Capacity & Green-by-Design Test Report (2025-10-02)](https://github.com/citomni/.github/blob/main/docs/CitOmni_Framework_-Capacity_and_Green_by_Design_Test_Report-2025-10-02.md)

---

## Get Started


### Quick start (HTTP app)

```bash
# 1) Scaffold a new app from the HTTP skeleton
composer create-project citomni/http-skeleton my-app
cd my-app

# 2) Run the built-in PHP server
php -S 127.0.0.1:8080 -t public

# 3) Visit http://127.0.0.1:8080
```

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

- **Runtime / Execution Mode Layer** - architectural rationale for HTTP vs CLI, baseline ownership, merge order.  
  -> [citomni/kernel · docs/CitOmni_Runtime_(aka_Execution)_Mode_Layer.md](https://github.com/citomni/kernel/blob/main/docs/CitOmni_Runtime_(aka_Execution)_Mode_Layer.md)

- **Provider Packages: Design, Semantics, and Best Practices** - how providers contribute `MAP_*` / `CFG_*`, routes, precedence rules, testing.  
  -> [citomni/kernel · docs/CitOmni_Provider_Packages_(Design_Semantics_and_Best_Practices).md](https://github.com/citomni/kernel/blob/main/docs/CitOmni_Provider_Packages_(Design_Semantics_and_Best_Practices).md)

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

### Sustainable by Design

Software efficiency is not only a matter of speed - it is a matter of sustainability. Every instruction executed and every byte allocated ultimately consumes electricity. When frameworks impose unnecessary overhead, the cost is multiplied at scale: thousands of servers, millions of requests, gigawatts of energy. This translates directly into higher operational costs and a measurable environmental impact.

CitOmni is deliberately engineered to minimize waste:

1. **Deterministic execution paths** - predictable config merges and no runtime scanning mean the CPU is doing only what is strictly required.
2. **Memory-lean architecture** - services and configuration are resolved via static maps, not reflection or discovery mechanisms, keeping allocations minimal.
3. **Cache-friendly design** - compiled arrays and preloaded maps make full use of OPcache and modern CPU caches, reducing redundant parsing and lookups.
4. **Side-effect-free boot** - the framework loads only what you ask for; no hidden I/O, no implicit background tasks.

From a sustainability perspective, this has profound implications:

* **More work per watt**: by reducing framework overhead, each server can handle more requests per unit of energy consumed.
* **Higher server density**: applications can be consolidated on fewer machines without compromising performance, directly lowering hardware demand and embodied emissions.
* **Lower cooling requirements**: efficient CPU and memory usage reduces thermal load, decreasing the energy footprint of data centers.
* **Scalable environmental benefit**: what saves a few milliseconds and kilobytes in a local test translates into massive resource savings when multiplied across global deployments.

The philosophy is simple but rigorous: **an efficient framework does not merely serve developers - it serves the planet.** In a world where digital infrastructure accounts for a growing share of global energy use, CitOmni's focus on low overhead is both a performance strategy and an ecological commitment.

---

> ### Research note: why software efficiency matters (and scales)
>
> * **Baseline today.** Best-available estimates put data centres at roughly **1-1.3% of global electricity demand (2022)** (IEA, 2022 [1]), excluding crypto. Ranges vary due to methodology and scope.  
> * **Growth outlook.** Multiple analyses project **electricity use more than doubling by ~2030** (≈945 TWh in one IEA scenario) with AI a key driver of load growth (IEA, 2024 [2]). Even if the overall share remains a few percent, the absolute TWh rise is significant.  
> * **Methodological caution.** Literature reviews show **wide spreads** in model outputs (from ~200 TWh to >1,000 TWh for 2022), explained by different boundary choices (what counts as "in" the data centre), extrapolation methods, and assumptions. This underlines the value of focusing on *verifiable efficiency at the software/runtime layer* (IEA 4E, 2025 [3]).  
> * **Embodied vs operational.** The footprint isn't only electricity for workloads. **Embodied emissions** from **hardware manufacturing and refresh cycles** (servers, accelerators, storage) are a material part of tech companies' total footprints; reducing server counts and extending lifetimes via efficient software cuts both operational and embodied carbon (Meta, 2024 [4]).  
> * **Why framework overhead matters.** At scale, **milliseconds per request** and **unnecessary allocations/I/O** aggregate into **MWh and hardware demand**. Deterministic boot paths, cache-friendly maps, and avoiding runtime scanning/reflection lower CPU cycles and memory pressure - yielding **more work per watt**, **higher app density per server**, **lower cooling load**, and **fewer machines procured over time**. These micro-efficiencies compound across fleets. Historical data show efficiency as the lever that kept data-centre energy flat amid demand growth (Masanet et al., 2020 [5]).  
>
> **Bottom line:** Even as absolute data-centre electricity use rises, *software-level efficiency* remains a first-order lever. By minimizing overhead in the framework itself, CitOmni converts engineering choices directly into **fewer joules per request**, **fewer servers per workload**, and **lower embodied and operational emissions** - benefits that scale with traffic.
>
> ---
>
> **References**  
> 1. [IEA 2022 - Data Centres and Data Transmission Networks][1]  
> 2. [IEA 2024 - Energy and AI (Executive Summary)][2]  
> 3. [IEA 4E 2025 - Data Centre Energy Use: Critical Review of Models and Results][3]  
> 4. [Meta 2024 - Estimating embodied carbon in data center hardware][4]  
> 5. [Masanet et al. 2020 - Recalibrating global data center energy-use estimates (Science)][5]  

[1]: https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks "IEA 2022 - Data Centres and Data Transmission Networks"  
[2]: https://www.iea.org/reports/energy-and-ai/executive-summary "IEA 2024 - Energy and AI (Executive Summary)"  
[3]: https://www.iea-4e.org/wp-content/uploads/2025/05/Data-Centre-Energy-Use-Critical-Review-of-Models-and-Results.pdf "IEA 4E 2025 - Data Centre Energy Use: Critical Review of Models and Results"  
[4]: https://sustainability.atmeta.com/blog/2024/09/10/estimating-embodied-carbon-in-data-center-hardware-down-to-the-individual-screws "Meta 2024 - Estimating embodied carbon in data center hardware"  
[5]: https://datacenters.lbl.gov/sites/default/files/Masanet_et_al_Science_2020.full_.pdf "Masanet et al. 2020 - Recalibrating global data center energy-use estimates (Science)"  

---

## Community & Governance

- **Contributing:** see [CONTRIBUTING.md](CONTRIBUTING.md)
- **Code of Conduct:** see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- **Security:** report privately via [SECURITY.md](SECURITY.md)
- **Licensing overview:** see [LICENSING.md](LICENSING.md)
- **Trademarks:** see [TRADEMARKS.md](TRADEMARKS.md)

---

## Contributing

* Code style: PHP 8.2+, PSR-4, **tabs**, K&R braces.
* Keep vendor files side-effect free (OPcache-friendly).
* Don't swallow exceptions in core; let the global error handler log.

---

## Coding & Documentation Conventions

All CitOmni projects follow the shared conventions documented here:
[CitOmni Coding & Documentation Conventions](https://github.com/citomni/kernel/blob/main/docs/CONVENTIONS.md)

---

## License

CitOmni uses different licenses depending on repository purpose. See each repo’s `LICENSE`.

- **Runtime packages** (kernel, http, cli, infrastructure): **GPL-3.0-or-later with the CitOmni Plugin Exception 1.0**  
- **Provider skeletons**: **GPL-3.0-or-later with the CitOmni Template Exception 1.0**  
- **App/HTTP/CLI skeletons**: **MIT**  
- **Private repositories**: **Proprietary** (no redistribution)

Details: [LICENSING.md](LICENSING.md)

---

## Trademarks

"CitOmni" and the CitOmni logo are trademarks of Lars Grove Mortensen; factual references are allowed, but do not modify the marks, create confusingly similar logos, or imply endorsement.

---

## Author

Developed and maintained by **[Lars Grove Mortensen](https://github.com/LarsGMortensen)** © 2012-2025
Contributions and pull requests are welcome!

---

Built with ❤️ on the CitOmni philosophy: **low overhead**, **high performance**, and **ready for anything**.
