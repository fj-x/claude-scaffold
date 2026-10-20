---
globs: "migrations/**/*.php"
---

# Migration Safety Rules

- NEVER use `DROP TABLE` or `DROP COLUMN` in production migrations
- Destructive changes require a two-step migration:
  1. First migration: add new column/table, dual-write
  2. Second migration (after deploy): drop old column/table
- Always provide `down()` method with reverse operation
- Review generated SQL before applying: `bin/console doctrine:migrations:diff` then inspect
- One logical change per migration file
- Migration class names are auto-generated timestamps — do not rename
- Test migrations on a fresh database: `bin/console doctrine:database:create --env=test && bin/console doctrine:migrations:migrate --env=test`
- Index naming convention: `idx_{table}_{columns}`, unique: `uniq_{table}_{columns}`
