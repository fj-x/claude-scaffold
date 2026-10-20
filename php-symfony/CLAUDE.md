# Project

Symfony 7.x application with PHP 8.3+. PostgreSQL database. API-first (JSON responses).

## Architecture

Bounded contexts as top-level `src/` subdirectories. Each BC owns its entities, repositories, services, and controllers.

```
src/
  Auth/           # Authentication and authorization
  ...             # Add BCs here as the project grows
```

Cross-BC communication via Symfony Messenger events only. No direct imports between BCs.

## Conventions

- PSR-12 coding standard, enforced by PHP-CS-Fixer
- Strict types declared in every file: `declare(strict_types=1);`
- Type hints on all parameters, return types, and properties
- Final classes by default; extend only with explicit reason
- Constructor property promotion preferred
- Doctrine ORM with migrations via `doctrine:migrations:*`
- Symfony Messenger for async jobs and cross-BC events
- Conventional commits: `feat(auth): add login endpoint`

## Verify commands

```
make test         # PHPUnit tests
make lint         # PHP-CS-Fixer dry-run + PHPStan
make stan         # PHPStan level 8
make fix          # PHP-CS-Fixer auto-fix
make migrate      # Doctrine migrations
```

## Guardrails

- Do NOT modify `.env` or `.env.local` — copy `.env.example` and ask user to configure
- Do NOT run raw SQL outside migrations — use Doctrine DQL/QueryBuilder
- Do NOT skip type declarations on any public method
- Do NOT create circular dependencies between bounded contexts
- Do NOT use `dd()`, `dump()`, or `var_dump()` in committed code
- After every code change, run `make test` and `make lint` before considering done
