# Building Type-Safe Compound Components

## 🎯 Core Concepts

### 🧩 The Problem with Compound Components
While compound components (like `<Select>` and `<Option>`) provide great flexibility, they are often hard to type in TypeScript. The `children` prop doesn't "inherit" type parameters from the parent, leading to:
- Manual type casting
- Loss of type inference for values
- Verbose and error-prone APIs

### 🏭 The Component Factory Pattern
Instead of exporting components directly, export a **factory function** that ties the types together. This "locks" the child components to the parent's generic type.

```typescript
// The Factory Function
export const createRadioGroup = <T extends string>() => ({
  RadioGroup: (props: RadioGroupProps<T>) => <InternalRadioGroup {...props} />,
  RadioGroupItem: (props: ItemProps<T>) => <InternalItem {...props} />
});
```

---

## 📝 Implementation Details

### ❌ The "Bad" Way (Manual Typing)
Each child requires an explicit generic, which is tedious and easy to forget.
```tsx
<RadioGroup<ThemeValue> value={v} onChange={o}>
  <RadioGroupItem<ThemeValue> value="light">☀️</RadioGroupItem>
  <RadioGroupItem<ThemeValue> value="dark">🌑</RadioGroupItem>
</RadioGroup>
```

### ✅ The "Sharp" Way (Typed Instance)
Call the factory once at the module level. All resulting components share the same type context.
```tsx
type ThemeValue = 'light' | 'dark';
const Theme = createRadioGroup<ThemeValue>();

function ThemeSwitcher() {
  return (
    <Theme.RadioGroup value={v} onChange={o}>
      <Theme.RadioGroupItem value="light">☀️</Theme.RadioGroupItem>
      {/* value is automatically typed as 'light' | 'dark' */}
    </Theme.RadioGroup>
  );
}
```

---

## 💡 Key Takeaways

1. **Inference > Annotation** - Aim for APIs where types are inferred from context.
2. **Module Level Factories** - Create typed component instances outside of your render loops.
3. **Type Locking** - Use factory patterns to ensure siblings and parents share the exact same generic constraints.
4. **Trade-off** - You trade a direct import for a function call, but gain robust type safety for your design system.
