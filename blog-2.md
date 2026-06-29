# How Generics Let You Write Reusable Code Without Losing Type Safety

## Introduction

One of the first things you run into when writing TypeScript is this tension: you want your functions and components to work with different types of data, but you also want the compiler to catch mistakes. If you use `any` to make things flexible, you throw away type checking. If you hard-code specific types, you end up duplicating logic for every data shape you need.

Generics solve this problem cleanly. They let you write a piece of code once and have it work with many different types — while keeping full type safety the entire time.

## The Core Idea

A generic is basically a type variable. Instead of deciding the type when you write the function, you let the caller decide it when they use the function. The syntax uses angle brackets, and by convention most people use `T` as the placeholder name.

Here is a dead-simple example. Say you want a function that takes a value and wraps it in an array:

```ts
function wrapInArray<T>(value: T): T[] {
  return [value];
}

const nums = wrapInArray(5);        // type: number[]
const words = wrapInArray("hello");  // type: string[]
```

Notice that we never told TypeScript what `T` is. It figured it out from the argument we passed. The return type adjusts automatically too. If you pass a number, you get back `number[]`. If you pass a string, you get back `string[]`. No duplication, no loss of type information.

## A More Realistic Example

Let us say you are building a function that finds the first item in an array matching some condition:

```ts
function findFirst<T>(items: T[], predicate: (item: T) => boolean): T | undefined {
  for (let i = 0; i < items.length; i++) {
    if (predicate(items[i])) {
      return items[i];
    }
  }
  return undefined;
}
```

Now this function works for any type of array:

```ts
const numbers = [10, 20, 30, 40];
const firstBig = findFirst(numbers, (n) => n > 25);  // type: number | undefined

interface User {
  id: number;
  name: string;
}

const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

const bob = findFirst(users, (u) => u.name === "Bob");  // type: User | undefined
```

In both cases the compiler knows exactly what type you are working with. You get full autocomplete on `n` and `u` inside those callbacks. Try misspelling a property and TypeScript will flag it immediately.

## Constraining Generics

Sometimes you need a generic to be flexible but not completely open-ended. You can use the `extends` keyword to set boundaries. For instance, if your function needs to access a `.length` property, you can require it:

```ts
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): number {
  return item.length;
}

logLength("hello");       // works, strings have length
logLength([1, 2, 3]);     // works, arrays have length
// logLength(42);          // error, numbers do not have length
```

This gives you flexibility within limits. The function accepts strings, arrays, or any custom object with a `length` property — but it rejects things that do not fit.

## Generics in Interfaces and Classes

Generics are not limited to functions. You can use them in interfaces and classes too, which is especially handy for data structures:

```ts
interface ApiResponse<T> {
  status: number;
  data: T;
  message: string;
}

const userResponse: ApiResponse<User> = {
  status: 200,
  data: { id: 1, name: "Alice" },
  message: "Success",
};

const numberResponse: ApiResponse<number[]> = {
  status: 200,
  data: [1, 2, 3],
  message: "Success",
};
```

The `ApiResponse` interface adapts to whatever you put in the angle brackets. You write the shape once and reuse it everywhere with different payload types. The compiler still checks that the `data` field matches what you declared.

Here is a class example — a basic stack data structure:

```ts
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}

const numberStack = new Stack<number>();
numberStack.push(10);
numberStack.push(20);
const top = numberStack.peek();  // type: number | undefined
```

One class definition, usable with any type, fully type-checked every time.

## Why Not Just Use `any`?

You could technically achieve "reusability" with `any`. But then you lose everything that makes TypeScript worth using. Compare:

```ts
function wrapInArrayAny(value: any): any[] {
  return [value];
}

const result = wrapInArrayAny("hello");
result[0].toFixed(2);  // no error at compile time, crashes at runtime
```

With `any`, the compiler has no idea what is inside the array. It cannot warn you when you call a method that does not exist on the actual value. Generics give you the same flexibility without this trade-off.

## Conclusion

Generics are one of those features that feel intimidating at first but quickly become second nature. The pattern is always the same: replace a hard-coded type with a type variable, and let the caller fill it in. You end up with code that is both reusable and safe — which is exactly the sweet spot TypeScript is designed for. Start using them in your utility functions and data structures, and you will notice how much cleaner and more maintainable your code becomes.
