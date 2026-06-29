# TypeScript Problem Solving & Blog Assignment

This repository contains my solutions for the Advanced Problem Solving with TypeScript & OOP assignment.

## Project Structure

```
├── solutions.ts    # All 7 coding problem solutions
├── blog-1.md       # Blog: Why `any` is dangerous and how `unknown` + type narrowing help
├── blog-2.md       # Blog: How Generics enable reusable yet type-safe code
└── README.md       # You are here
```

## Problems Covered

| # | Problem | Key Concept |
|---|---------|-------------|
| 1 | `filterEvenNumbers` | Array filtering |
| 2 | `reverseString` | String manipulation |
| 3 | `checkType` | Union types & type guards |
| 4 | `getProperty` | Generics with constraints |
| 5 | `toggleReadStatus` | Interfaces & object spreading |
| 6 | `Person` / `Student` | Classes, inheritance, methods |
| 7 | `getIntersection` | Array intersection logic |

## How to Run

Make sure you have TypeScript installed:

```bash
npm install -g typescript
```

Compile and run:

```bash
tsc solutions.ts
node solutions.js
```

Or use `ts-node` to run directly:

```bash
npx ts-node solutions.ts
```

## Blog Topics

1. **Blog 1** — Explains why `any` is called a "type safety hole," why `unknown` is safer for unpredictable data, and breaks down type narrowing with practical examples.

2. **Blog 2** — Covers how Generics allow you to build reusable functions, interfaces, and classes that stay strictly typed no matter what data you pass in.

## Technologies

- TypeScript
- Node.js
