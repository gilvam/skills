# Code style — control flow

Statement-level style (verb-led names, early returns, no boolean-flag params, CQS, method/class sizing, one
binding per statement, minimal comments) is owned by the **`code-standards-en`** skill. This file pins the
one TypeScript-formatting rule the house standards call out explicitly.

## Always use braces on `if` — never a single-line `if`

Every `if` (and `else`, `for`, `while`) uses a braced block on its own lines, even for a single statement.

```ts
// ✅ good
if (user.isActive) {
  notify(user);
}

// ❌ bad — single-line / brace-less
if (user.isActive) notify(user);
if (user.isActive) { notify(user); }
```

Why: a braced block prevents the classic "second statement silently outside the `if`" bug, keeps diffs clean
when a branch grows, and reads consistently. This matches the Google TypeScript Style Guide, which requires
braces for all control-flow statements.

The same applies to `else` / `else if`, loops, and `do/while`:

```ts
// ✅
if (isPaid) {
  ship();
} else {
  hold();
}

for (const item of items) {
  process(item);
}
```

> Guarded **early returns** (preferring them over deep `if/else` nesting) are governed by `code-standards-en`
> — they still use braces.

## Avoid

- Brace-less single-statement `if`/`else`/`for`/`while`.
- Putting the body on the same line as the condition.
