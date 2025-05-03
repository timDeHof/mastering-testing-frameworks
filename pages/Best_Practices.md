---
title: Best Practices
layout: center
transition: fade
---

# Best Practices

---
layout: two-cols-header
transition: slide-left
hideInToc: true
---

# Jest: Test Structure
::left::
```javascript
describe('Component', () => {
  // Shared setup for all tests in this suite
  beforeAll(() => {
    // Initialize database connection or heavy resources
  });
  // Teardown after all tests complete
  afterAll(() => {
    // Close database connection
  });
  // Fresh context for each test
  beforeEach(() => {
    // Reset mocks, create fresh component instances
    jest.clearAllMocks();
  });
  // Cleanup after each test
  afterEach(() => {
    // Clean up DOM elements
  });
  test('should render default state', () => {
    // Test implementation
  });
});
```
::right::
<ul>
<li>Nested <code>describe</code> blocks for hierarchical test organization</li>
<li>Lifecycle hooks with resource cleanup prevention leaks</li>
<li><strong>Performance:</strong> Shared setup via beforeAll + isolated contexts with beforeEach</li>
<li><strong>Safety:</strong> Automatic mock cleanup with clearAllMocks</li>
<li><strong>TypeScript:</strong> Strongly typed mocks and spies</li>
</ul>

---
layout: two-cols-header
transition: slide-left
hideInToc: true
---

# Jest: Mocking Techniques
::left::
```javascript
// Function mock with typed interface
const mockFn = jest.fn<() => boolean>()
  .mockReturnValue(true)
  .mockName('validationFn');

// Spy on method with cleanup
const fetchDataSpy = jest.spyOn(API, 'fetchData')
  .mockResolvedValue({ data: {} });

// Mock module with error handling
jest.mock('axios', () => ({
  get: jest.fn()
    .mockResolvedValue({ data: {} })
    .mockRejectedValue(new Error('Network error'))
}));

// Clear mocks after each test
afterEach(() => {
  jest.clearAllMocks();
});
```
::right::
<ul>
<li>Automatic mock cleanup in afterEach hook</li>
<li>Prefer spyOn for existing methods with proper typing</li>
<li>Named mocks for better error messages</li>
<li>Error case testing with mockRejectedValue</li>
<li>Type-safe mocks</li>
</ul>
---
layout: two-cols
transition: slide-left
---

## Jest: Edge Cases
- Test empty/null inputs
- Validate error messages
- Test boundary values
- Use `test.concurrent` safely
- Temporary focus with `.only`

---
layout: two-cols-header
transition: slide-left
hideInToc: true
---

# Cypress Practices
::left::
```javascript
cy.get('[data-cy="submit-button"]')
  .should('be.visible')
  .and('be.enabled')
  .click({ force: true });

cy.intercept('GET', '/api', (req) => {
  req.reply((res) => {
    if (res.statusCode >= 400) {
      throw new Error(`API request failed with status ${res.statusCode}`);
    }
    return { fixture: 'test-data.json' };
  });
}).as('apiRequest');

cy.wait('@apiRequest').its('response.statusCode').should('eq', 200);
```
::right:: 
- Use dedicated test attributes
- Mock network requests
- Leverage fixtures for test data
- Chain assertions fluently


