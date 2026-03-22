# Rules by Language

> **TL;DR**: Community-curated .mdc rules organized by programming language. Source: awesome-cursorrules and community.

## Python
**Source**: awesome-cursorrules, cursor.directory
- Type hints on all functions
- Pydantic models for data validation
- ruff for linting/formatting
- pathlib.Path over os.path
- f-strings for formatting
- Context managers for resources

**Install**: Browse `PatrickJS/awesome-cursorrules` → Python section → copy .mdc files to `.cursor/rules/`

## TypeScript
- Strict mode (strict: true)
- Interfaces for objects, types for unions
- Avoid `any` — use `unknown`
- Discriminated unions for state
- const assertions for literals
- Barrel exports in index.ts

## Go
- gofmt before commit
- Error wrapping with fmt.Errorf + %w
- Context propagation
- Table-driven tests
- Interface segregation
- Goroutine leak prevention

## Java
- Immutable objects where possible
- Builder pattern for complex construction
- Optional instead of null returns
- Stream API for collections
- JUnit 5 + Mockito for testing

## C# / .NET
- Async/await for I/O
- Nullable reference types enabled
- Record types for DTOs
- LINQ for collections
- xUnit + Moq for testing

## Rust
- Ownership and borrowing conventions
- Error handling with Result/Option
- Clippy for linting
- Documentation tests
- Trait-based abstraction

## Where to Find More

| Language | Best Source |
|----------|-----------|
| All | [PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) |
| TypeScript/JS | [blefnk/awesome-cursor-rules](https://github.com/blefnk/awesome-cursor-rules) |
| Security (all) | [matank001/cursor-security-rules](https://github.com/matank001/cursor-security-rules) |
| Browse online | [cursor.directory](https://cursor.directory) |
