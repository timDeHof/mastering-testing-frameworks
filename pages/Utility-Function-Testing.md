---
hideInToc: true
title: Utility Function Testing
---
# Utility Function Testing

```typescript
import { formatDate } from "./dateUtils";

describe("formatDate", () => {
  it("should format date correctly", () => {
    expect(formatDate("2024-03-21")).toBe("March 21, 2024");
  });

  it("should handle invalid dates", () => {
    expect(() => formatDate("invalid")).toThrow();
  });
});
```