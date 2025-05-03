---
title: Jest Testing Framework
name: JestTestingFramework
layout: two-cols-header
---

# Jest Testing Framework
::left::
## What is Jest?

- JavaScript testing framework
- Created by Facebook
- Built-in:
  - Test runner
  - Assertion library
  - Mocking capabilities
  - Code coverage
  - Snapshot testing

## Setting Up Jest

```bash
npm install --save-dev jest @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

::right::

## Basic Test Structure

```typescript
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

describe("ComponentName", () => {
  const defaultProps = {
    // Define default props
  };

  beforeEach(() => {
    // Common setup
  });

  it("renders without crashing", () => {
    render(<ComponentName {...defaultProps} />);
    expect(screen.getByRole("main")).toBeInTheDocument();
  });

  it("handles user interactions", async () => {
    const user = userEvent.setup();
    render(<ComponentName {...defaultProps} />);

    await user.click(screen.getByRole("button"));
    expect(screen.getByText("Clicked")).toBeInTheDocument();
  });
});
```

