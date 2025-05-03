# Testing Guidelines

## Testing Stack
- Jest: Unit testing and component testing
- React Testing Library: Component testing and integration testing
- MSW (Mock Service Worker): API mocking
- Cypress: End-to-end testing

## Test Requirements

### 1. Component Testing Requirements

#### 1.1 Required Test Cases
- Initial render
- User interactions
- State changes
- Error states
- Loading states
- Edge cases
- Accessibility compliance

#### 1.2 Required Test Coverage
- All props variations
- All user interactions
- All error scenarios
- All loading states
- All conditional rendering
- All event handlers

### 2. Integration Testing Requirements

#### 2.1 Required Test Cases
- Component interactions
- Data flow
- State management
- API integration
- Route changes
- Error boundaries

#### 2.2 Required Test Coverage
- All feature workflows
- All API endpoints
- All state transitions
- All error scenarios
- All loading states
- All user interactions

### 3. E2E Testing Requirements

#### 3.1 Required Test Cases
- Critical user paths
- Authentication flows
- Form submissions
- Navigation flows
- Data persistence
- Error handling

#### 3.2 Required Test Coverage
- All main user journeys
- All authentication flows
- All form submissions
- All navigation paths
- All error scenarios
- All responsive breakpoints

### 4. Test Quality Requirements

#### 4.1 Code Quality
- No duplicate test cases
- Clear test descriptions
- Proper test isolation
- No hardcoded values
- No implementation details
- Proper error handling

#### 4.2 Performance Requirements
- Tests should run under 20 seconds
- No unnecessary waits
- Proper mocking of heavy operations
- Efficient test setup
- Proper cleanup after tests

### 5. Documentation Requirements

#### 5.1 Test Documentation
- Clear test descriptions
- Required setup steps
- Dependencies listed
- Known limitations
- Test data requirements
- Mock requirements

#### 5.2 Code Documentation
- JSDoc comments for test utilities
- Clear inline comments
- Documentation of complex logic
- Documentation of test data
- Documentation of mocks

### 6. Accessibility Testing Requirements

#### 6.1 Required Test Cases
- Keyboard navigation
- Screen reader compatibility
- ARIA attributes
- Color contrast
- Focus management
- Error announcements

#### 6.2 Required Coverage
- All interactive elements
- All form fields
- All error messages
- All dynamic content
- All navigation elements
- All modal dialogs

### 7. API Testing Requirements

#### 7.1 Required Test Cases
- All API endpoints
- Request/response handling
- Error scenarios
- Loading states
- Data transformation
- Authentication

#### 7.2 Required Coverage
- All HTTP methods
- All response types
- All error codes
- All data formats
- All authentication flows
- All rate limiting

### 8. State Management Testing Requirements

#### 8.1 Required Test Cases
- State updates
- State persistence
- State synchronization
- Error states
- Loading states
- State cleanup

#### 8.2 Required Coverage
- All state transitions
- All state dependencies
- All state mutations
- All error scenarios
- All loading scenarios
- All cleanup operations

## 1. Test File Organization

### 1.1 Directory Structure
```plaintext
src/
├── components/
│   └── ComponentName/
│       ├── ComponentName.tsx
│       ├── ComponentName.test.tsx
│       └── __mocks__/
├── hooks/
│   ├── useHook.ts
│   └── useHook.test.ts
├── utils/
│   ├── util.ts
│   └── util.test.ts
└── __tests__/
    └── integration/
        └── feature.test.tsx

cypress/
├── e2e/
│   ├── auth/
│   ├── features/
│   └── smoke/
├── fixtures/
└── support/
```

### 1.2 Naming Conventions
- Jest test files: `*.test.ts(x)`
- Cypress test files: `*.cy.ts(x)`
- Test utilities: `test-utils.tsx`
- Mock files: `__mocks__/*.ts`

## 2. Jest & React Testing Library Guidelines

### 2.1 Component Testing
```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

describe('ComponentName', () => {
  const defaultProps = {
    // Define default props
  };

  beforeEach(() => {
    // Common setup
  });

  it('renders without crashing', () => {
    render(<ComponentName {...defaultProps} />);
    expect(screen.getByRole('main')).toBeInTheDocument();
  });

  it('handles user interactions', async () => {
    const user = userEvent.setup();
    render(<ComponentName {...defaultProps} />);

    await user.click(screen.getByRole('button'));
    expect(screen.getByText('Clicked')).toBeInTheDocument();
  });
});
```

### 2.2 Custom Hooks Testing
```typescript
import { renderHook, act } from '@testing-library/react';

describe('useCustomHook', () => {
  it('should update state correctly', () => {
    const { result } = renderHook(() => useCustomHook());

    act(() => {
      result.current.update();
    });

    expect(result.current.value).toBe('updated');
  });
});
```

### 2.3 Utility Functions Testing
```typescript
describe('formatDate', () => {
  it('should format date correctly', () => {
    expect(formatDate('2024-03-21')).toBe('March 21, 2024');
  });
});
```

## 3. MSW Guidelines

### 3.1 Setup
```typescript
// src/__tests__/mocks/handlers.ts
import { rest } from 'msw';

export const handlers = [
  rest.get('/api/users', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({ users: [] })
    );
  }),
];
```

### 3.2 Usage in Tests
```typescript
import { setupServer } from 'msw/node';
import { handlers } from './mocks/handlers';

const server = setupServer(...handlers);

describe('ComponentWithAPI', () => {
  beforeAll(() => server.listen());
  afterEach(() => server.resetHandlers());
  afterAll(() => server.close());

  it('should fetch and display data', async () => {
    render(<ComponentWithAPI />);
    await waitFor(() => {
      expect(screen.getByText('Data')).toBeInTheDocument();
    });
  });
});
```

## 4. Cypress Guidelines

### 4.1 Component Testing
```typescript
// src/components/ComponentName/ComponentName.cy.tsx
describe('ComponentName', () => {
  beforeEach(() => {
    cy.mount(<ComponentName />);
  });

  it('renders correctly', () => {
    cy.get('[data-testid="component"]').should('be.visible');
  });

  it('handles interactions', () => {
    cy.get('button').click();
    cy.get('[data-testid="result"]').should('contain', 'Clicked');
  });
});
```

### 4.2 E2E Testing
```typescript
// cypress/e2e/features/feature.cy.ts
describe('Feature', () => {
  beforeEach(() => {
    cy.intercept('GET', '/api/data', { fixture: 'data.json' });
    cy.visit('/feature');
  });

  it('completes user flow', () => {
    cy.get('form').within(() => {
      cy.get('input[name="email"]').type('test@example.com');
      cy.get('button[type="submit"]').click();
    });
    cy.url().should('include', '/success');
  });
});
```

## 5. Testing Best Practices

### 5.1 Selectors Priority
1. `getByRole` (preferred)
2. `getByLabelText`
3. `getByText`
4. `getByTestId` (last resort)

### 5.2 Test Isolation
```typescript
// Good
beforeEach(() => {
  jest.clearAllMocks();
  cleanup();
});

// Bad
let sharedState;
```

### 5.3 Async Testing
```typescript
// Good
await waitFor(() => {
  expect(screen.getByText('Data')).toBeInTheDocument();
});

// Bad
await new Promise(resolve => setTimeout(resolve, 1000));
```

### 5.4 Error Handling
```typescript
// Good
it('handles errors gracefully', async () => {
  server.use(
    rest.get('/api/data', (req, res, ctx) => {
      return res(ctx.status(500));
    })
  );

  render(<Component />);
  expect(screen.getByText('Error')).toBeInTheDocument();
});
```

## 6. Performance Guidelines

### 6.1 Test Speed
- Keep individual tests under 1 second
- Use `jest.setTimeout()` sparingly
- Mock heavy operations

### 6.2 Test Coverage
- Aim for 80% coverage
- Focus on critical paths
- Test error scenarios

## 7. Documentation Requirements

### 7.1 Test Documentation
```typescript
/**
 * @description Tests the user authentication flow
 * @requires mocks/auth-handlers.ts
 */
describe('Authentication', () => {
  // Test implementation
});
```

### 7.2 Required Documentation Elements
- Test purpose
- Dependencies
- Setup requirements
- Known limitations

## 8. CI/CD Integration

### 8.1 Test Scripts
```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:e2e": "cypress run",
    "test:e2e:dev": "cypress open"
  }
}
```

### 8.2 CI Configuration
```yaml
test:
  script:
    - npm run test
    - npm run test:e2e
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
```

## 9. Common Pitfalls to Avoid

1. Testing Implementation Details
```typescript
// Bad
expect(component.state.value).toBe('test');

// Good
expect(screen.getByText('test')).toBeInTheDocument();
```

2. Relying on Test IDs
```typescript
// Bad
cy.get('[data-testid="submit-button"]');

// Good
cy.get('button[type="submit"]');
```

3. Hard-coded Waits
```typescript
// Bad
await new Promise(resolve => setTimeout(resolve, 1000));

// Good
await waitFor(() => {
  expect(element).toBeInTheDocument();
});
```

## 10. Testing Checklist

Before submitting a PR:
- [ ] Unit tests for new components
- [ ] Integration tests for new features
- [ ] E2E tests for critical paths
- [ ] Error scenarios covered
- [ ] Accessibility tests
- [ ] Performance considerations
- [ ] Documentation updated
- [ ] No flaky tests
- [ ] All tests passing