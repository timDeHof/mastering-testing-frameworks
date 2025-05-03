---
title: Cypress Testing Framework
name: cypressTestingFramework
layout: two-cols-header
---

# Cypress Testing Framework

::left:: 
## What is Cypress?

- End-to-end testing framework
- Created by Cypress.io
- Built-in:
  - Test runner
  - Assertion library
  - Time travel capabilities
  - Automatic waiting
  - Network stubbing

## Setting Up Cypress

```bash
npm install --save-dev cypress
npx cypress open
```
::right::
## Basic Test Structure

```typescript
describe("Example Test Suite", () => {
  beforeEach(() => {
    // Visit the page before each test
    cy.visit("/");
  });

  it("should display the correct title", () => {
    cy.title().should("include", "TeamForward");
  });

  it("should allow user to sign up", () => {
    // Click sign up button
    cy.get('a[href="/signup"]').click();

    // Fill out form
    cy.get('input[name="email"]').type("test@example.com");
    cy.get('input[name="password"]').type("password123");

    // Submit form
    cy.get('button[type="submit"]').click();

    // Verify redirect
    cy.url().should("include", "/dashboard");
  });
});
```
