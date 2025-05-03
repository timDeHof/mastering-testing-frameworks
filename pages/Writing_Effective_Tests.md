---
title: Writing Effective Tests
layout: two-cols-header
---

# Writing Effective Tests
::left::
## Test Naming Conventions

- Use clear, descriptive names
- Follow a consistent pattern (e.g., "should do something")
- Include the component or feature being tested

::right::
## Test Organization

- Group related tests in describe blocks
- Use <code>beforeEach</code> for common setup
- Use <code>afterEach</code> for cleanup
- Use <code>beforeAll</code> for one-time setup
- Use <code>afterAll</code> for one-time cleanup

## Test Coverage

- Aim for 80%+ coverage
- Focus on critical paths first
- Test both happy paths and edge cases
- Test error handling
