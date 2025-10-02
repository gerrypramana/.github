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
* ♻️ **Green by design** - lower memory use and CPU cycles -> less server load, more requests per watt, better scalability, smaller carbon footprint.


---

### Green by design

CitOmni's "Green by design" claim is empirically validated at the framework level.

The core runtime achieves near-floor CPU and memory costs per request on commodity shared infrastructure, sustaining hundreds of RPS per worker with extremely low footprint.

See the full test report here:
https://github.com/citomni/.github/blob/main/docs/CitOmni_Framework_-Capacity_and_Green_by_Design_Test_Report-2025-10-02.md

---

## Get Started

* [**citomni/http-skeleton**](https://github.com/citomni/http-skeleton) - ready-to-use starter skeleton for webapps, APIs, or SaaS.
* [**citomni/http**](https://github.com/citomni/http) - lean HTTP runtime.
* [**citomni/kernel**](https://github.com/citomni/kernel) - the tiny, deterministic core.
* [**citomni/cli**](https://github.com/citomni/cli) - fast CLI runtime.
* [**citomni/infrastructure**](https://github.com/citomni/infrastructure) - shared, cross-mode services.
* [**citomni/auth**](https://github.com/citomni/auth) - authentication & account management.
* [**citomni/testing**](https://github.com/citomni/testing) - framework-integrated environment for the systematic evaluation of CitOmni applications.


More packages are available under the [CitOmni organization](https://github.com/citomni).

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

All CitOmni core packages are released under the **GNU General Public License v3.0 or later**.
See individual repositories for details.

---

## Trademarks

"CitOmni" and the CitOmni logo are trademarks of Lars Grove Mortensen; factual references are allowed, but do not modify the marks, create confusingly similar logos, or imply endorsement.

---

## Author

Developed and maintained by **[Lars Grove Mortensen](https://github.com/LarsGMortensen)** © 2012-2025
Contributions and pull requests are welcome!

---

Built with ❤️ on the CitOmni philosophy: **low overhead**, **high performance**, and **ready for anything**.
