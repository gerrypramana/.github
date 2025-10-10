## Why
<!-- Short rationale / problem statement. What user pain or goal does this solve? -->

## What
<!-- Concise summary of changes. Keep it to essentials. -->

## How to test
<!-- Minimal, deterministic steps or commands to validate the change. -->
- [ ] `composer validate`
- [ ] `composer dump-autoload -o`
- [ ] Run tests / static analysis (if present):
      `vendor/bin/phpunit` • `vendor/bin/phpstan analyse`

## Backward compatibility
- [ ] No BC breaks
- [ ] (If any) Documented BC notes / migration steps in the PR description

## Performance & determinism
- [ ] No unnecessary I/O in constructors
- [ ] Deterministic behavior (no environment-dependent magic)
- [ ] No reflection/autowiring scans introduced

## Docs
- [ ] Public API and configuration updated (README / docs)
- [ ] Inline PHPDoc/comments added or adjusted (English)

## License confirmation
I confirm my contribution is under this repository's license:
- Runtime packages: **GPL-3.0-or-later + CitOmni Plugin Exception 1.0**
- Provider skeleton: **GPL-3.0-or-later + CitOmni Template Exception 1.0**
- App/HTTP/CLI skeletons: **MIT**
- Private repos: **proprietary**

## DCO
- [ ] All commits are signed (`git commit -s`) and include a valid `Signed-off-by` line.

## Notes
<!-- Optional: risks, roll-back plan, follow-ups, or related issues/links. -->
