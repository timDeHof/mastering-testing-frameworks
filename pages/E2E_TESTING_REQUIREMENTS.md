---
hideInToc: true
title: Cypress E2E Testing Requirements
layout: center
---

# Cypress E2E Testing Requirements

---
title: file structure and organization
layout: two-cols-header
hideInToc: true
---
## File Structure and Organization

::left::
### Directory Structure
```plaintext
cypress/
├── e2e/
│   ├── auth/
│   │   ├── login.cy.ts
│   │   ├── signup.cy.ts
│   │   └── forgot-password.cy.ts
│   ├── features/
│   │   ├── profile.cy.ts
│   │   └── dashboard.cy.ts
│   └── smoke/
│       └── app.cy.ts
├── fixtures/
│   └── test-data/
├── support/
│   ├── commands.ts
│   └── e2e.ts
```

::right::

### Naming Conventions
- Test files: `*.cy.ts`
- Feature files: `feature-name.cy.ts`
- Support files: `command-name.ts`
- Fixtures: `data-type.json`
---
hideInToc: true
---
## 2. Test Structure Requirements

### 2.1 Basic Test Structure
```typescript
describe('Feature Name', () => {
  beforeEach(() => {
    // Setup and common interceptors
    cy.intercept('GET', '/api/*', { fixture: 'data.json' });
    cy.disableAnimations();
  });

  describe('Sub-feature', () => {
    it('should perform specific action', () => {
      // Test implementation
    });
  });
});
```
---

### 2.2 Required Test Categories

#### 2.2.1 Authentication Flows
```typescript
// Login Flow
it('should handle successful login', () => {
  cy.intercept('POST', '/api/v1/auth/login', { fixture: 'login-success.json' });
  cy.login(email, password);
  cy.url().should('include', '/dashboard');
});

// Error Handling
it('should handle authentication errors', () => {
  cy.intercept('POST', '/api/v1/auth/login', {
    statusCode: 401,
    body: { message: 'Invalid credentials' }
  });
});
```

#### 2.2.2 Navigation Flows
```typescript
it('should navigate through main user journey', () => {
  cy.visit('/');
  cy.get('[data-testid="nav-item"]').click();
  cy.url().should('include', '/target-page');
});
```

#### 2.2.3 Form Submissions
```typescript
it('should handle form submission', () => {
  cy.intercept('POST', '/api/submit', { fixture: 'submit-response.json' });
  cy.get('form').within(() => {
    cy.get('input[name="field"]').type('value');
    cy.get('button[type="submit"]').click();
  });
});
```
---

## 3. Testing Requirements

### 3.1 Responsive Testing
```typescript
const viewports = ['iphone-6', 'ipad-2', [1280, 720]];

viewports.forEach(viewport => {
  it(`should work on ${viewport}`, () => {
    cy.viewport(viewport);
    // Test implementation
  });
});
```

### 3.2 Network Handling
```typescript
// API Interceptors
cy.intercept('GET', '/api/**', (req) => {
  req.reply({
    statusCode: 200,
    body: { /* response data */ }
  });
});

// Error States
cy.intercept('POST', '/api/**', {
  statusCode: 500,
  delayMs: 100
});
```
---

### 3.3 State Management
```typescript
// Session Management
beforeEach(() => {
  cy.session('user-session', () => {
    cy.login(email, password);
  });
});

// Local Storage
cy.window().then((win) => {
  win.localStorage.setItem('key', 'value');
});
```
---

## 4. Best Practices

### 4.1 Selector Priority
1. `data-testid` attributes (preferred)
2. Semantic HTML elements
3. ARIA roles
4. Class names (last resort)

### 4.2 Custom Commands
```typescript
// In commands.ts
Cypress.Commands.add('customAction', (param) => {
  // Implementation
});

// In tests
cy.customAction(param);
```

### 4.3 Error Handling
```typescript
cy.on('uncaught:exception', (err) => {
  console.error(err);
  return false;
});
```
---
layout: two-cols-header
---

## Performance Requirements

::left::
### Test Optimization
- Use `cy.session()` for login state
- Avoid unnecessary waiting
- Group related tests
- Use proper test isolation

::right::
### Time Constraints
- Individual test should complete within 10 seconds
- Test suite should complete within reasonable time
- Use `cy.clock()` for time-dependent tests

---
layout: two-cols-header
hideInToc: true
---

# Documentation Requirements
::left::
### Test Documentation
```typescript
/**
 * @description Tests the user authentication flow
 * @requires fixtures/user-data.json
 * @requires commands/auth.ts
 */
describe('Authentication', () => {
  // Test implementation
});
```
::right::
### 6.2 Required Documentation Elements
- Test purpose
- Prerequisites
- Required fixtures
- Known limitations
- Special setup requirements
---
layout: two-cols-header
---

# Quality Assurance
::left::
### Code Quality
- No hardcoded waits
- Proper error handling
- Clear test descriptions
- DRY principles
- Consistent formatting

::right::
### Test Reliability
- No flaky tests
- Proper cleanup after tests
- Isolated test cases
- Deterministic assertions
---
layout: two-cols-header
---

## CI/CD Integration
::left::
### Docker Support
```yaml
services:
  cypress:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - CYPRESS_baseUrl=http://app:3000
```
::right::
### Reporting Requirements
- Generate test reports
- Screenshot on failure
- Video recording (optional)
- Test metrics collection
