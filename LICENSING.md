# CitOmni Licensing Overview

CitOmni uses different licenses depending on repository purpose.
Always check each repository's own `LICENSE` file for the authoritative text.

## Matrix (at a glance)

| Category                              | Examples                                                                                             | License          | Notes                                                                                                           |
|---------------------------------------|------------------------------------------------------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------|
| **Official framework packages**        | `citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure`                            | **MIT**          | Core/runtime code: permissive for broad adoption and minimal friction.                                          |
| **Proprietary packages (org-managed)** | `citomni/auth`, `citomni/testing`                                                                     | **Proprietary**  | Distributed **only via certified resellers**. No direct customers and no redistribution.                        |
| **Skeletons**                          | `citomni/http-skeleton`, `citomni/cli-skeleton`, `citomni/app-skeleton`, `citomni/provider-skeleton` | **MIT**          | Intended for copying into your apps/providers; MIT avoids copyleft friction.                                    |
| **Documentation & org meta**           | `citomni/docs`, `citomni/.github` (policies/templates)                                               | **CC BY-SA 4.0** | Documentation and shared org templates. Share-alike ensures improvements propagate.                             |

> "CitOmni" and the CitOmni logo are trademarks of Lars Grove Mortensen.  
> Copyright permissions are separate from trademark permissions.

## SPDX and Composer

- **SPDX header (code files)**
  ```php
  /*
   * SPDX-License-Identifier: MIT
   */
````

* **SPDX for documentation**

  * Add a short header stating **CC BY-SA 4.0** and link to the repo's `LICENSE`.
* **Composer `license` field**

  * Official packages / skeletons: `"MIT"`
  * Docs-only repos: `"CC-BY-SA-4.0"` or omit if not published to Packagist
  * Proprietary org packages (e.g., `citomni/auth`, `citomni/testing`): `"proprietary"`

## Third-party dependencies

Prefer permissive licenses (MIT/BSD/Apache-2.0) or GPL-compatible alternatives when necessary. Avoid AGPL/SSPL unless intentionally required.

## Trademarks

See `TRADEMARKS.md` for naming and branding guidelines (including restrictions on using "citomni" in vendor/package names and domains).

*Last updated: 2025-10-12*