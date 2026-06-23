# Testing Anti-Patterns

**Load this reference when:** writing or changing tests, adding mocks, or tempted to add test-only methods to production code.

## The Iron Laws

```
1. NEVER test mock behavior
2. NEVER add test-only methods to production classes
3. NEVER mock without understanding dependencies
```

## Anti-Pattern 1: Testing Mock Behavior

Test what the code does, not what the mocks do.

```typescript
// BAD: Testing mock exists
test('renders sidebar', () => {
  render(<Page />);
  expect(screen.getByTestId('sidebar-mock')).toBeInTheDocument();
});

// GOOD: Test real component
test('renders sidebar', () => {
  render(<Page />);
  expect(screen.getByRole('navigation')).toBeInTheDocument();
});
```

## Anti-Pattern 2: Test-Only Methods in Production

Never add methods to production classes that are only used by tests. Put them in test utilities instead.

## Anti-Pattern 3: Mocking Without Understanding

Before mocking, understand what side effects the real method has and what the test depends on. Mock at the correct level.

## Anti-Pattern 4: Incomplete Mocks

Mock the COMPLETE data structure as it exists in reality, not just fields your immediate test uses. Partial mocks fail silently.

## Anti-Pattern 5: Tests as Afterthought

TDD cycle: Write failing test → Implement to pass → Refactor → THEN claim complete.

## Anti-Pattern 6: Over-Complex Mocks

When a test needs a tangle of mocks to run, the mock setup has become harder than the code under test. That's a signal the unit is over-coupled or the test is fighting the design. Consider replacing the mocked unit test with an integration test that exercises the real collaborators — often the setup gets simpler and the test catches more.

## Quick Reference

| Anti-Pattern | Fix |
|--------------|-----|
| Assert on mock elements | Test real component or unmock |
| Test-only methods in production | Move to test utilities |
| Mock without understanding | Understand dependencies first |
| Incomplete mocks | Mirror real API completely |
| Tests as afterthought | TDD — tests first |
| Over-complex mocks | Consider integration tests |

## The Bottom Line

**Mocks are tools to isolate, not things to test.** If TDD reveals you're testing mock behavior, you've gone wrong.
