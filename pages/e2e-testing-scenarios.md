---
name: e2eScenariosTypes
title: Testing Different Types of End-to-End Scenarios
hideInToc: true 
---

### Testing Different Types of End-to-End Scenarios

#### Authentication Testing

```typescript
describe("Authentication Tests", () => {
  it("should allow user to log in", () => {
    // Visit login page
    cy.visit("/login");

    // Fill out form
    cy.get('input[name="email"]').type("test@example.com");
    cy.get('input[name="password"]').type("password123");

    // Submit form
    cy.get('button[type="submit"]').click();

    // Verify redirect
    cy.url().should("include", "/dashboard");

    // Verify welcome message
    cy.contains("Welcome, Test User").should("exist");
  });

  it("should show error for invalid credentials", () => {
    // Visit login page
    cy.visit("/login");

    // Fill out form with invalid credentials
    cy.get('input[name="email"]').type("invalid@example.com");
    cy.get('input[name="password"]').type("wrongpassword");

    // Submit form
    cy.get('button[type="submit"]').click();

    // Verify error message
    cy.contains("Invalid credentials").should("exist");
  });
});
```

#### Component Interaction Testing

```typescript
describe("Component Interaction Tests", () => {
  beforeEach(() => {
    // Visit the page with the component
    cy.visit("/dashboard");
  });

  it("should allow user to create a new event", () => {
    // Click create event button
    cy.get('button[data-testid="create-event"]').click();

    // Fill out form
    cy.get('input[name="title"]').type("New Event");
    cy.get('input[name="date"]').type("2025-06-15");

    // Submit form
    cy.get('button[type="submit"]').click();

    // Verify event is created
    cy.contains("New Event").should("exist");
  });

  it("should allow user to edit their profile", () => {
    // Click profile button
    cy.get('button[data-testid="profile-button"]').click();

    // Click edit profile
    cy.get('button[data-testid="edit-profile"]').click();

    // Update bio
    cy.get('textarea[name="bio"]').clear().type("Updated bio text");

    // Save changes
    cy.get('button[type="submit"]').click();

    // Verify changes are saved
    cy.contains("Profile updated successfully").should("exist");
  });
});
```

### Using Cypress with React Applications

```typescript
// cypress/support/component.ts
import "@testing-library/cypress/add-commands";

// Example component test
describe("Component Tests", () => {
  it("should render the component", () => {
    cy.mount(<MyComponent />);
    cy.get('[data-testid="my-component"]').should("exist");
  });

  it("should update when props change", () => {
    cy.mount(<MyComponent value="initial" />);
    cy.get('[data-testid="value"]').should("have.text", "initial");

    cy.mount(<MyComponent value="updated" />);
    cy.get('[data-testid="value"]').should("have.text", "updated");
  });
});
```
