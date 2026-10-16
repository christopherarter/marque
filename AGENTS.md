# Marque

`dynamik-dev/marque` is a Laravel authorization library for PHP 8.4+ and Laravel 12/13.
The public namespace is `DynamikDev\Marque\`. See `README.md` and `docs/` for usage.

## Development

- Install dependencies with `composer install`. This library intentionally does not commit `composer.lock`; use `composer update` to refresh a local install.
- Run `composer check` before finishing a change. It validates the manifest, checks Pint formatting, runs Larastan at level 9, and runs feature and architecture tests on in-memory SQLite.
- Run a focused test with `composer test -- --filter=Name`. Run architecture checks with `composer test:arch`; benchmarks are opt-in with `composer test:benchmark`.
- PostgreSQL integration tests: `composer test:services`, `composer test:pgsql`, then `composer test:down`. These use a disposable `marque_test` database on localhost. Docker services are optional for the default checks.
- Coverage requires PCOV or Xdebug: `composer test:coverage` enforces 90%.
- Format PHP changes with `composer format -- --dirty`. Add regression coverage for behavior changes using Pest and Orchestra Testbench.

## Package conventions

- Keep traits, middleware, commands, and the facade thin. Delegate authorization and persistence to contracts; bind default implementations in `MarqueServiceProvider`.
- Preserve authorization semantics: boundaries limit grants, explicit deny overrides allow, and unmatched requests deny. Scoped assignments and cache invalidation must remain isolated between subjects and scopes.
- Use strict types and PHP 8.4-compatible type declarations. Keep Laravel 12 and 13 compatibility; CI tests both on PHP 8.4 and 8.5.
- Keep Pest 4/5 compatibility in test tooling: Laravel 12 requires Pest 4's Symfony Process range, while Laravel 13 can use Pest 5.
- Use Eloquent models instead of the `DB` facade, explicit arrays instead of `compact()`, and event classes' `dispatch()` methods instead of `event()`.
- Treat migrations as published package interfaces. Add a new migration for schema changes instead of rewriting released migrations.
- Document public API changes in the relevant scenario-based guide under `docs/`.
- Prefer Context7 for current dependency documentation when available. Use codebase-memory graph tools for code discovery when available, and fall back to focused file searches when they are not.
