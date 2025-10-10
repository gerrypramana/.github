# CitOmni Licensing Overview

CitOmni uses different licenses depending on repository purpose.
Always check each repository's own `LICENSE` (and `LICENSE-EXCEPTIONS` if present).

## Matrix (at a glance)

| Category                             | Examples                                                                  | License                                                   | Notes                                                                                                                                                                                                                               |
| ------------------------------------ | ------------------------------------------------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Runtime packages**                 | `citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure` | **GPL-3.0-or-later** **+ CitOmni Plugin Exception 1.0**   | GPL core with an **Additional Permission** (GPLv3 §7) that allows separate "plugins/providers" to link against public APIs under the author's chosen terms (including proprietary), provided no substantial CitOmni code is copied. |
| **Provider skeletons**               | `citomni/provider-skeleton`                                               | **GPL-3.0-or-later** **+ CitOmni Template Exception 1.0** | A **template** meant to be copied. The Template Exception lets you license **your resulting provider** under your own terms (incl. proprietary). The **skeleton itself remains GPL**.                                               |
| **App skeletons**                    | `citomni/http-skeleton`, `citomni/cli-skeleton`, `citomni/app-skeleton`   | **MIT**                                                   | Skeleton code is intended to be copied into applications. MIT avoids copyleft friction for end-user apps.                                                                                                                           |
| **Private/proprietary repositories** | e.g., paid customer providers                                             | **Proprietary**                                           | Not redistributable. Typically not on Packagist. Use VCS/Private Packagist/GitHub Packages. See repo `LICENSE`.                                                                                                                     |

---

## What the exceptions mean

### CitOmni Plugin Exception 1.0 (runtime packages)

* A GPLv3 §7 **Additional Permission**.
* Allows **separate and independent** "plugins/providers" to **link** against public APIs in runtime packages (kernel/http/cli/infrastructure) and be licensed under **the author's chosen terms (including proprietary)**.
* Does **not** allow re-licensing the runtime packages themselves.
* Does **not** allow copying **substantial CitOmni source** into your plugin/provider.

### CitOmni Template Exception 1.0 (provider-skeleton)

* Also a GPLv3 §7 **Additional Permission**-but for **templates**.
* You can create a **new provider** from the skeleton and license that **resulting provider** under your own terms (including proprietary).
* The **skeleton code itself remains GPL**.

---

## Picking the right license (decision guide)

* **You're building/maintaining a runtime layer used by apps/providers**
  -> Use **GPL-3.0-or-later + CitOmni Plugin Exception 1.0**.

* **You're providing a template to help others start a *provider***
  -> Use **GPL-3.0-or-later + CitOmni Template Exception 1.0**.

* **You're providing a template to start an *application***
  -> Use **MIT**.

* **You're publishing a closed-source, paid provider** (customer-specific/private)
  -> Use **Proprietary** license (EULA). Keep it out of Packagist; use private distribution.

---

## Compliance notes

* **SPDX header (runtime packages)**

  ```php
  /*
   * SPDX-License-Identifier: GPL-3.0-or-later
   * Additional Permission: CitOmni Plugin Exception 1.0 (see LICENSE-EXCEPTIONS)
   */
  ```

* **SPDX header (provider-skeleton)**

  ```php
  /*
   * SPDX-License-Identifier: GPL-3.0-or-later
   * Additional Permission: CitOmni Template Exception 1.0 (see LICENSE-EXCEPTIONS)
   */
  ```

* **SPDX header (app skeletons)**

  ```php
  /*
   * SPDX-License-Identifier: MIT
   */
  ```

* **Composer `license` field**

  * Runtime packages / provider-skeleton: `"GPL-3.0-or-later"`
  * App skeletons: `"MIT"`
  * Proprietary repos: `"proprietary"`

* **Distribute the exception text**
  Keep a `LICENSE-EXCEPTIONS` file in repos that use an exception, and mention it in the README **License** section.

* **`.gitattributes`**
  Do **not** mark `LICENSE` or `LICENSE-EXCEPTIONS` as `export-ignore`. They must ship in distribution archives.

---

## Third-party dependencies

* Ensure dependencies are **GPLv3-compatible** for runtime packages. Avoid AGPL/SSPL unless you intend the stronger copyleft.
* Proprietary providers should use dependencies compatible with proprietary distribution (MIT/BSD/Apache-2.0/LGPL/GPL-with-exception, etc.). If in doubt, consult the dependency's license.

---

## Packagist / distribution

* Public open-source packages (GPL+exceptions, MIT) -> publish to Packagist normally.
* Proprietary packages -> **do not** publish to Packagist. Use:

  * VCS repositories in Composer (`"type": "vcs"`)
  * Private Packagist / GitHub Packages
  * Self-hosted Satis if needed

---

## FAQs

**Q: Can I build proprietary providers for CitOmni?**
Yes. That's the purpose of the **Plugin Exception** on runtime packages. Use public APIs, do not copy substantial CitOmni code, and license your provider as you wish.

**Q: If I copy code from provider-skeleton, does my provider have to be GPL?**
No. The **Template Exception** allows you to license your resulting provider under your own terms (including proprietary). The **skeleton** remains GPL.

**Q: Do modifications to CitOmni runtime packages remain GPL?**
Yes. Changes to runtime packages themselves must remain under **GPL-3.0-or-later**.

**Q: Why are app skeletons MIT?**
Because skeleton files are intended to be **copied into end-user apps**. MIT avoids copyleft friction so production apps can be licensed as needed.

**Q: Where do I find the exact exception text?**
In each repo's `LICENSE-EXCEPTIONS` (only present where an exception applies).

---

## Trademarks

"CitOmni" and the CitOmni logo are trademarks of **Lars Grove Mortensen**.
Open-source licenses grant copyright permissions, not trademark rights.
See [`TRADEMARKS.md`](TRADEMARKS.md) for usage guidelines.

---

## Contact

* Licensing & legal questions: **[legal@citomni.com](mailto:legal@citomni.com)**
* Security reports (private): **[security@citomni.com](mailto:security@citomni.com)**

---

*This document summarizes CitOmni's licensing approach. In case of conflict, the text in each repository's actual `LICENSE` (and `LICENSE-EXCEPTIONS`, if any) prevails.*
