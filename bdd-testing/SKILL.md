---
name: bdd-agent-rules
description: Critical constraints for AI coding agents working with BDD acceptance tests. Defines immutability rules, workflow, and the principle that .feature files are read-only behavioral contracts.
---

When working on projects with BDD acceptance tests, you must follow strict constraints to prevent fragility. Acceptance tests define **what** the system must do; you implement **how**.

## Immutability Rules

**CRITICAL:** You may NEVER edit `.feature` files unless explicitly instructed to do so by a human user.

- `.feature` files are read-only behavioral contracts
- If acceptance tests fail, fix the CODE, not the tests
- You may implement/modify step definitions that execute the tests
- You may freely edit unit tests

**File Permissions:**
`.feature` files are typically `chmod 444` or `chattr +i` - attempting to edit them will fail.

## BDD Syntax Reminder

Acceptance tests use Gherkin:

```gherkin
Feature: [Capability name]

  Scenario: [Specific behavior]
    Given [initial state]
    When [action]
    Then [observable outcome]
```

- **Given** = preconditions (state before action)
- **When** = user action (what triggers the behavior)
- **Then** = observable result (what user sees/experiences)

Focus on **user-observable behavior**, not implementation details.

## Workflow

1. **Read the `.feature` file** to understand required behavior
2. **Implement or modify code** to satisfy scenarios
3. **Update step definitions** if needed (the code that maps Gherkin to tests)
4. **Run acceptance tests** to verify behavior
5. **If tests fail:** debug and fix implementation
6. **Never:** modify the `.feature` file itself

## What You Can Edit

✅ Source code (application logic)
✅ Step definitions (test execution code)
✅ Unit tests
✅ Integration tests
✅ Documentation

❌ `.feature` files (acceptance tests)

## Key Principles

**Acceptance tests are the contract:**
- They define "done"
- They describe behavior from user perspective
- They are written in collaboration with stakeholders
- They must not contain implementation details

**Your job:**
- Make the scenarios pass
- Refactor freely as long as scenarios stay green
- Add unit tests for implementation details
- Keep code clean while respecting the behavioral contract

## Example

**Given this immutable acceptance test:**
```gherkin
Scenario: User adds item to cart
  Given user "john@example.com" is logged in
  When John adds "Laptop" to cart
  Then John should see "Item added to cart"
  And cart should contain 1 item
```

**You may:**
- Implement `Cart` class however you want (array, Set, database)
- Rename internal methods
- Refactor data structures
- Change how cart is stored (session, database, Redis)
- Modify step definitions to test the new implementation

**You may not:**
- Change the scenario steps
- Remove or modify assertions
- Alter the expected behavior

## When Tests Fail

**Acceptance test failure = behavioral regression**

1. Read the failing scenario
2. Understand what user-observable behavior is broken
3. Fix the code to restore that behavior
4. Do NOT modify the test to match broken code

## Common Mistakes to Avoid

❌ Modifying `.feature` files to make tests pass
❌ Implementing based on assumptions rather than reading scenarios
❌ Focusing on implementation over observable behavior

✅ Read scenarios first to understand requirements
✅ Implement to satisfy the behavioral contract
✅ Refactor freely while keeping tests green
✅ Add unit tests for implementation details
