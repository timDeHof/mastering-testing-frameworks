---
hideInToc: true
---
# Custom Hook Testing

```typescript
import { act, renderHook } from "@testing-library/react";

describe("useCustomHook", () => {
  it("should update state correctly", () => {
    const { result } = renderHook(() => useCustomHook());

    act(() => {
      result.current.update();
    });

    expect(result.current.value).toBe("updated");
  });
});
```