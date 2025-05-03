---
hideInToc: true
---

# Component Testing

```typescript
import { render, screen } from "@testing-library/react";

describe("Button Component", () => {
  it("renders with default text", () => {
    render(<Button />);
    expect(screen.getByText("Click me")).toBeInTheDocument();
  });

  it("renders with custom text", () => {
    render(<Button text="Submit" />);
    expect(screen.getByText("Submit")).toBeInTheDocument();
  });
});
```