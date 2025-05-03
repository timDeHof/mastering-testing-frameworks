---
hideInToc: true
title: Cypress Component Testing Requirements
---

# Cypress Component Testing Requirements

## 1. File Structure
- Test files must be located in the same directory as the component
- Use `.cy.tsx` extension for component tests
- Follow naming convention: `[ComponentName].cy.tsx`

## 2. Basic Test Structure
```typescript
import ComponentName from './ComponentName';

describe('ComponentName', () => {
  const mountComponent = (props = {}) => {
    cy.mount(<ComponentName {...props} />);
  };

  beforeEach(() => {
    // Reset viewport or other common setup
    cy.viewport(1024, 768);
  });

  // Test cases...
});
```

## 3. Required Test Categories

### 3.1 Rendering Tests
- Test initial render
- Test with different prop combinations
- Test responsive behavior (mobile, tablet, desktop)
- Verify all important elements are visible

### 3.2 Interaction Tests
- Test all clickable elements
- Test form inputs and submissions
- Test keyboard navigation
- Test hover states and animations

### 3.3 State Management
- Test loading states
- Test error states
- Test empty states
- Test data-populated states

### 3.4 Accessibility Tests
- Test ARIA attributes
- Test keyboard navigation
- Test color contrast
- Test screen reader compatibility

### 3.5 Integration Tests
- Test component integration with providers
- Test data fetching behavior
- Test event handling
- Test callbacks and parent communication

## 4. Best Practices

### 4.1 Selectors Priority
1. data-testid (preferred): `[data-testid="component-name"]`
2. Role: `cy.get('[role="button"]')`
3. Semantic HTML: `cy.get('button')`
4. Class/ID (last resort): `cy.get('.class-name')`

### 4.2 Setup and Cleanup
```typescript
beforeEach(() => {
  // Reset any mocks/stubs
  cy.clock();
  cy.disableAnimations();
});

afterEach(() => {
  // Cleanup after each test
});
```

### 4.3 Async Operations
```typescript
it('handles async operations', () => {
  cy.intercept('GET', '/api/data', { fixture: 'data.json' });
  mountComponent();
  cy.wait('@apiCall');
  cy.get('[data-testid="result"]').should('be.visible');
});
```

### 4.4 Responsive Testing
```typescript
it('is responsive', () => {
  mountComponent();

  // Mobile
  cy.viewport('iphone-6');
  cy.get('[data-testid="mobile-menu"]').should('be.visible');

  // Tablet
  cy.viewport('ipad-2');
  cy.get('[data-testid="tablet-layout"]').should('be.visible');

  // Desktop
  cy.viewport(1024, 768);
  cy.get('[data-testid="desktop-nav"]').should('be.visible');
});
```

## 5. Documentation Requirements
- Document all test cases with clear descriptions
- Document any test data or fixtures
- Document any special setup requirements
- Document any known limitations or edge cases

## 6. Quality Checks
- No flaky tests
- No unnecessary waits
- Proper error handling
- Clear failure messages
- Consistent naming conventions
- DRY test code
****