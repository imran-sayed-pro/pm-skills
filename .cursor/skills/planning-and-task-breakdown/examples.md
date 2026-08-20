# Example — one S task

Input: approved spec for profile validation (PFA-M1).

Output in the task list target:

```markdown
### Task 2: Validate profile fields

- Short description: Pure `validateProfile` with all-errors-in-one-pass.
- Description: Implement `src/profile/validate.ts` so user input becomes a constructed `Profile` or a list of field errors. No DOM. No projection math.
- Acceptance criteria:
  - Currency is `INR` or `AED`; default `INR`.
  - `retirementAge` is required, 1–120, no default, not capped at 75.
  - Invalid fields are all reported in one pass.
- Test table:

  | Case | Input | Expected |
  |---|---|---|
  | Bad currency | `currency: "USD"` | `ok: false`, error field `currency` |
  | Valid INR | complete valid payload | `ok: true`, new object, trimmed name |

- Verification:
  - Test pass: `npx vitest run --coverage --coverage.provider=v8 --coverage.include=src/profile/validate.ts --coverage.thresholds.branches=100 tests/unit/profile-validate.test.ts`
  - Build succeeds: `npx tsc --noEmit`
  - Manual check: none (pure function)
- Dependencies: Task 1
- Files likely touched: `src/profile/validate.ts`, `tests/unit/profile-validate.test.ts`
- Estimated scope: S
```

Too large (must split): "Implement profile form AND validation AND persistence AND the graph."
