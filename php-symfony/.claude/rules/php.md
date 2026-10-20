---
globs: "**/*.php"
---

# PHP / Symfony Conventions

- PHP 8.3+ features: readonly properties, enums, match expressions, named arguments, fibers
- `declare(strict_types=1);` at top of every file
- Classes are `final` by default; use `abstract` or remove `final` only with reason
- Constructor property promotion for DTOs and value objects
- Return types on every method, including `void`
- No `mixed` type unless genuinely polymorphic — prefer union types
- Use PHP 8 attributes over annotations: `#[Route]`, `#[ORM\Entity]`, `#[Assert\NotBlank]`

## Symfony specifics

- Controllers extend `AbstractController`, are `final`, have `#[Route]` attribute on class
- Services auto-wired; explicit wiring only when interface has multiple implementations
- Use `#[Autowire]` attribute for scalar parameters, not `services.yaml` binds
- DTOs for request validation with Symfony Validator constraints
- Response format: always `JsonResponse` with proper HTTP status codes
- Use Symfony HttpKernel exceptions (`NotFoundHttpException`, etc.) — not manual status codes

## Doctrine ORM

- Entities use PHP 8 attributes, not XML/YAML mapping
- Repository pattern: custom repository class per entity
- UUID v7 for primary keys (ordered, timestamp-embedded)
- Soft deletes only when business requires audit trail; hard delete by default
- Use `doctrine:migrations:diff` to generate migrations, review before applying
