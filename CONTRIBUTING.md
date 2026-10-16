# Contributing

Use an up-to-date PHP 8.4 or 8.5 release and Composer 2. The test suite uses Orchestra Testbench; no Laravel application or `.env` file is required.

## Install and check

```bash
composer install
composer check
```

`composer check` validates `composer.json`, checks formatting with Pint, runs Larastan at level 9, and runs the feature and architecture suites against in-memory SQLite. PostgreSQL, Redis, and Docker are optional for this command.

```bash
composer test -- --filter=WildcardMatcher
composer test:arch
composer format -- --dirty
composer test:benchmark
```

Benchmarks are excluded from ordinary tests. To collect coverage, enable PCOV or Xdebug and run `composer test:coverage`; the minimum is 90%.

## Test PostgreSQL

The Compose services expose PostgreSQL and Valkey on localhost ports 5432 and 6379. The PostgreSQL tests migrate and reset the disposable `marque_test` database using the `test` user and password. Keep those ports free before starting the services.

```bash
composer test:services
composer test:pgsql
composer test:down
```

`composer test:all` runs SQLite followed by PostgreSQL and expects the services to be running. `composer test:mysql` is available for exploratory testing against a separately configured local MySQL database; MySQL is outside the supported CI matrix.

## Update dependencies

This library keeps `composer.lock` local so CI exercises the declared dependency ranges. After changing constraints, run:

```bash
composer update
composer check
composer audit --locked
```

Composer selects Pest 5 for Laravel 13 and Pest 4 for Laravel 12 because their Symfony Process requirements differ. Keep both Pest majors available while Laravel 12 is supported.

CI checks PHP 8.4/8.5 with Laravel 12/13, plus the lowest compatible stable dependencies on PHP 8.4 and Laravel 12. A scheduled dependency audit and weekly Dependabot updates catch new releases and advisories between code changes.

Shared coding-agent instructions live in [AGENTS.md](AGENTS.md). `CLAUDE.md` imports that file so both tools use the same project guidance.
