# Validate the `@NoNull()` decorator

Every DTO depends on the `@NoNull()` decorator. Confirm it exists before generating DTOs.

Glob for `src/app/_decorators/class.decorator.ts`.

- **If present** (the normal case): reuse it. From any DTO at
  `src/app/services/http/http-[service-name]/models/`, the import is exactly:
  ```typescript
  import { NoNull } from '../../../../_decorators/class.decorator';
  ```
  This import path is identical for the local example (`services/http/weather/models/`) and
  for the new `http-[name]/models/` layout — the directory depth is the same.

- **If missing**: vendor it before continuing. WebFetch the decorator source from
  `https://github.com/gilvam/typescript-utils/tree/main/src/decorators` (use the raw file
  URL) and Write it to `src/app/_decorators/class.decorator.ts`. Then proceed.

## What `@NoNull()` does (and does not) do

It converts `null` → `undefined` for **constructor arguments** and for the static
**`create`** / **`createArray`** arguments, which lets the TypeScript default parameter values
take over. As a result, `null`, `{}`, missing keys, and partial payloads all collapse to safe
defaults.

It does **not** recurse into nested objects or arrays. Parent DTOs must map their children
**explicitly** via `Child.create(...)` / `Child.createArray(...)` — see
[dto.md](dto.md).

## Reference

The decorator's behavior is covered by `src/app/_decorators/class.decorator.spec.ts`: it
verifies null→undefined for constructor args and for `create`/`createArray`, that other static
methods are untouched, and that the prototype chain / static properties are preserved.
