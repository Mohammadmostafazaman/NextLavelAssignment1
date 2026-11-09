# Type Narrowing in TypeScript: Practical Guide with Examples

## Introduction

TypeScript's static type system is powerful, but it must coexist with the reality that values often arrive as broad types — a union of `string | number`, or an unknown object from an API. Type narrowing is the process of refining a broad type into a more specific one within a conditional block, allowing TypeScript to understand exactly what you are working with at any given point in your code.

## Narrowing with `typeof`

The `typeof` operator is the most common narrowing tool. TypeScript recognizes its result and narrows the type inside each branch accordingly.

```typescript
function format(value: string | number): string {
  if (typeof value === "string") {
    return value.toUpperCase(); // TypeScript knows: string here
  }
  return value.toFixed(2); // TypeScript knows: number here
}
```

This is clean and safe. Without narrowing, calling `.toUpperCase()` on a `string | number` would be a type error.

## Narrowing with `instanceof`

When working with class instances, `instanceof` narrows the type to the specific class. This is especially useful in error handling and class hierarchies.

```typescript
class ApiError extends Error {
  statusCode: number = 0;
}

function handleError(err: Error | ApiError): string {
  if (err instanceof ApiError) {
    return `HTTP ${err.statusCode}: ${err.message}`; // ApiError here
  }
  return err.message; // plain Error here
}
```

TypeScript understands the prototype chain and uses it to refine the type inside the `instanceof` block.

## Narrowing with the `in` Operator

The `in` operator checks whether a property exists on an object. TypeScript uses this as a type guard when discriminating between object types that share some but not all properties.

```typescript
type Cat = { purr: () => void };
type Dog = { bark: () => void };

function makeSound(animal: Cat | Dog): void {
  if ("purr" in animal) {
    animal.purr(); // Cat here
  } else {
    animal.bark(); // Dog here
  }
}
```

This pattern is especially valuable with discriminated unions, where a shared property (often called a discriminant) has a unique literal value per variant.

## Conclusion

Type narrowing transforms TypeScript from a simple annotation layer into a tool that actively reasons about your code's runtime behavior. By combining `typeof`, `instanceof`, and `in` checks, you can safely handle heterogeneous data without casting or suppressing errors, writing code that is both type-safe and easy to follow.
