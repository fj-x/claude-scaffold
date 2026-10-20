---
globs: "tests/**/*.php"
---

# Testing Conventions

- PHPUnit 11+ with PHP 8 attributes: `#[Test]`, `#[DataProvider]`, `#[CoversClass]`
- Test directory mirrors `src/` structure: `tests/Auth/` ↔ `src/Auth/`
- Three test categories:
  - `tests/Unit/` — pure logic, no container, no DB; fast
  - `tests/Integration/` — service with real DB (test database); uses `KernelTestCase`
  - `tests/Functional/` — HTTP request/response; uses `WebTestCase` + Symfony test client
- Naming: `{ClassName}Test.php`, methods `test_descriptive_behavior_name()` (snake_case)
- Arrange / Act / Assert pattern with blank lines between sections
- One assertion concept per test method; multiple `assert*` calls OK if testing same concept
- Use Foundry (`zenstruck/foundry`) for factories, not manual entity construction
- Integration tests use transactions with rollback (Symfony default in `KernelTestCase`)
- Mock external services with Symfony HttpClient MockResponse, not Prophecy for HTTP
- No `sleep()` in tests — use Symfony Clock component for time-dependent logic
