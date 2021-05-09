---
description: >-
  The page describes the linting rules that shall be enforced by the KK
  compiler.
---

# Linting Rules

## 1. Function should not take more than two arguments

This is to improve readability at function call site.

```typescript
// Bad
let slice = \(
  string: String,
  start: Int,
  end: Int
) => null

do "Hello".slice(0, 1)

// Good
let slice = \(
  string: String,
  {
    start: Int,
    end: Int
  }
) => null

do "Hello".slice({start = 0, end = 1})
```

## 2. Naming conventions

<table>
  <thead>
    <tr>
      <th style="text-align:left"><code>snake_case</code>
      </th>
      <th style="text-align:left"><code>PascalCase</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <ul>
          <li>variables</li>
          <li>record property name</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li>type alias</li>
          <li>enum constructors</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

