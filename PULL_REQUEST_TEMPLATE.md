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
- Official framework packages (`citomni/kernel`, `citomni/http`, `citomni/cli`, `citomni/infrastructure`): **MIT**
- Skeletons (`citomni/http-skeleton`, `citomni/cli-skeleton`, `citomni/app-skeleton`, `citomni/provider-skeleton`): **MIT**
- Documentation & org meta (`citomni/docs`, `citomni/.github`): **CC BY-SA 4.0**
- Proprietary org packages (e.g., `citomni/auth`, `citomni/testing`): **Proprietary**  
  *(Distributed via certified resellers; CitOmni has no direct customers.)*

## DCO
- [ ] All commits are signed (`git commit -s`) and include a valid `Signed-off-by` line.

## Notes
<!-- Optional: risks, roll-back plan, follow-ups, or related issues/links. -->
