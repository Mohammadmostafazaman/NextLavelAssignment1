# Type Aliases vs Interfaces in TypeScript: When to Use Which

## Introduction

TypeScript provides two powerful tools for describing the shape of data: `type` aliases and `interface` declarations. While they often appear interchangeable on the surface, they have meaningful differences that influence which one you should reach for in a given situation. Understanding these distinctions leads to cleaner, more intentional code.

## Core Differences

The most fundamental difference is that interfaces are open and extendable, while type aliases are closed and final. An interface can be declared multiple times and TypeScript will merge the declarations automatically — a pattern called declaration merging. This makes interfaces ideal for defining contracts that libraries or modules need to extend.

```typescript
interface User {
  name: string;
}
interface User {
  age: number;
}
// TypeScript merges these: User now has both name and age
```

Type aliases do not support this. Redeclaring a type alias is a compile-time error. However, type aliases are significantly more flexible in what they can express. They can describe union types, intersection types, primitive aliases, tuples, and conditional types — things an interface simply cannot do.

```typescript
type ID = string | number;
type Pair = [string, number];
type Status = "active" | "inactive" | "pending";
```

Both support extension, but through different syntax. Interfaces use `extends`, while type aliases use the intersection operator `&`.

```typescript
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }

type AnimalType = { name: string };
type DogType = AnimalType & { breed: string };
```

## When to Choose Each

Prefer `interface` when you are defining the shape of objects, especially in class-based designs or public library APIs where downstream code may need to extend or augment your types. The `implements` keyword in classes works seamlessly with interfaces.

Prefer `type` when you need unions, intersections, mapped types, conditional types, or any construct that goes beyond plain object shapes. Type aliases are also preferable for utility-type compositions and function signatures that involve complex generics.

## Conclusion

Neither tool is universally superior. Interfaces are best for open, extendable object contracts; type aliases are best for expressive, composable type logic. In practice, many teams default to `interface` for objects and `type` for everything else — a convention that keeps codebases consistent and readable.
