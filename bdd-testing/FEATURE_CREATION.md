---
name: bdd-feature-creation
description: Interview users to create Gherkin feature files with concrete acceptance tests. Guides agents through discovering requirements, edge cases, and converting conversations into immutable behavioral specifications.
---

This skill guides coding agents in interviewing users to create Gherkin feature files - immutable acceptance tests that describe what the system should do from the user's perspective.

Note that `.feature` files are immutable for coding agents working on BDD tested projects - But that this restriction is lifted for the purpose of collaboratively creating or amending the features with the human user in order to create the testing environment.

**Goal:** Transform user requirements into concrete, testable scenarios that serve as behavioral contracts.

## Pre-Interview

1. Identify which feature you're creating tests for
2. Review any existing documentation or similar features
3. One feature = one `.feature` file


## Interview Process

### Step 1: Get a Concrete Example

**Ask:** "Can you give me a specific example of someone using this feature? Use a real name and actual data."

**Bad response:** "Users can reset passwords."
**Good response:** "Sarah forgets her password, clicks 'Forgot Password', enters sarah@example.com, gets an email, clicks the link, enters a new password, and gets logged in."

Keep asking "then what?" until you have the complete flow.

### Step 2: Identify Starting State

**Ask:** "What's the state before this happens? What needs to be true?"

Examples:
- "Is the user logged in or not?"
- "Does Sarah's account already exist?"
- "What's in the database before this starts?"

These become your **Given** steps.

### Step 3: Identify Actions

**Ask:** "What does the user do?"

Focus on **one action at a time**. These become your **When** steps.

If user describes implementation ("The system POSTs to the API"), redirect: "What does the user see happening?"

### Step 4: Identify Observable Outcomes

**Ask:** "How does the user know it worked? What do they see?"

Focus on **what the user experiences**:
- Messages shown
- Page changes
- Emails received
- Items appearing in lists

Avoid internal details like database changes or API calls.

These become your **Then** steps.

### Step 5: Discover Edge Cases

Use these prompts systematically:

- "What if [precondition] isn't true?" (e.g., "What if the email doesn't exist?")
- "What could go wrong?" (invalid input, network failure, etc.)
- "What are the limits?" (max items, time limits, rate limits)
- "What happens if they do it twice?"

Each edge case becomes a separate scenario.

## Converting to Gherkin

### Structure

```gherkin
Feature: [Feature name]
  [Optional: Brief description of business value]

  Background:
    [Optional: Common setup for all scenarios]
    Given [shared precondition]

  Scenario: [Descriptive name for happy path]
    Given [initial state]
    And [additional state]
    When [user action]
    And [additional action]
    Then [observable outcome]
    And [additional outcome]

  Scenario: [Descriptive name for edge case]
    Given [different initial state]
    When [action that triggers edge case]
    Then [expected error or alternative outcome]
```

### Rules

**Given** - Setup only, no actions
- ✅ `Given user "sarah@example.com" exists`
- ✅ `Given Sarah is logged in`
- ❌ `Given Sarah clicks login` (that's an action - use When)

**When** - Actions only, no assertions
- ✅ `When Sarah clicks "Add to Cart"`
- ✅ `When Sarah enters password "SecurePass123"`
- ❌ `When the cart has 5 items` (that's state - use Given)

**Then** - Observable outcomes only
- ✅ `Then Sarah should see "Item added"`
- ✅ `Then cart icon should show "1 item"`
- ❌ `Then database should have 1 row` (internal detail)
- ❌ `Then API returns 201` (implementation detail)

### Use Concrete Data

**Vague:**
```gherkin
Given a user exists
When they add an item
Then it works
```

**Concrete:**
```gherkin
Given user "john@example.com" is logged in
When John adds "Laptop" to cart
Then John should see "Laptop added to cart"
And cart icon should show "1 item"
```

### One Scenario = One Behavior

Each scenario tests exactly one thing.

**Too much in one scenario:**
```gherkin
Scenario: Complete user journey
  Given I am not logged in
  When I register with "user@example.com"
  And I verify my email
  And I log in
  And I add item to cart
  And I checkout
  Then order should be placed
```

**Split into separate scenarios:**
- `Scenario: User registration`
- `Scenario: Email verification`
- `Scenario: Add item to cart`
- `Scenario: Checkout process`

## Common Pitfalls

### User Describes Implementation

**User says:** "The system should POST to /api/auth and return a JWT token."

**Redirect:** "What does the user see happening?"

**User clarifies:** "They see 'Login successful' and get redirected to their dashboard."

### Vague Descriptions

**User says:** "The system should handle errors properly."

**Probe:** "Can you give me a specific example of an error? What does the user do and what should they see?"

### Missing Edge Cases

If user only describes happy path, use the systematic prompts:
- What if preconditions aren't met?
- What could go wrong?
- What are the limits?
- What happens if done twice?

### Internal Details Leak In

**Bad:**
```gherkin
Then database should have reset_token
And token should be hashed with bcrypt
```

**Good:**
```gherkin
Then user should receive password reset email
And email should contain a reset link
```

## Output Format

### File Naming

`features/<feature_name>.feature`

Examples:
- `features/user_registration.feature`
- `features/password_reset.feature`
- `features/shopping_cart.feature`

### Complete Example

```gherkin
Feature: Password Reset
  As a user who has forgotten their password
  I want to reset it via email
  So that I can regain access to my account

  Background:
    Given the application is running
    And email service is available

  Scenario: User successfully resets password
    Given user "sarah@example.com" exists with password "OldPass123"
    And Sarah is not logged in
    When Sarah requests password reset for "sarah@example.com"
    Then Sarah should receive email at "sarah@example.com"
    And email subject should be "Reset Your Password"
    
    When Sarah clicks the reset link from email
    Then Sarah should see password reset form
    
    When Sarah enters new password "NewPass456"
    And Sarah submits the form
    Then Sarah should see "Password updated successfully"
    And Sarah should be logged in automatically
    And old password "OldPass123" should no longer work

  Scenario: Reset request for non-existent email
    Given no user exists with email "unknown@example.com"
    When password reset is requested for "unknown@example.com"
    Then response should be "If that email exists, you'll receive a link"
    And no email should be sent

  Scenario: Expired reset link
    Given user "sarah@example.com" requested password reset 25 hours ago
    When Sarah clicks the expired reset link
    Then Sarah should see "This link has expired"
    And Sarah should see "Request new reset link" button
    And Sarah should not be able to change password

  Scenario: Rate limiting on reset requests
    Given user "sarah@example.com" has requested 3 resets in last hour
    When Sarah requests another password reset
    Then Sarah should see "Too many reset requests. Try again in 1 hour."
    And no reset email should be sent
```

## After Creating the Feature File

1. **Make it immutable:**
   ```bash
   chmod 444 features/password_reset.feature
   ```

2. **Review with user:**
   - Read scenarios back to them
   - Confirm edge cases are covered
   - Note any open questions

3. **Mark as acceptance criteria:**
   - This file defines "done"
   - Implementation must satisfy all scenarios
   - AI agents cannot modify this file

## Quick Reference

**Interview Questions:**
1. "Give me a specific example with real names and data"
2. "What's the starting state?"
3. "What does the user do?"
4. "How do they know it worked?"
5. "What if [precondition] isn't true?"
6. "What could go wrong?"
7. "What are the limits?"
8. "What happens if done twice?"

**Gherkin Pattern:**
- Feature: High-level capability
- Background: Shared setup (optional)
- Scenario: One specific behavior
- Given: Initial state (setup only)
- When: User action (no assertions)
- Then: Observable outcome (no actions)

**Quality Checks:**
- [ ] Uses concrete data (names, emails, values)
- [ ] No implementation details (APIs, database, internals)
- [ ] Each scenario tests one thing
- [ ] Happy path is clear
- [ ] Edge cases identified
- [ ] All steps are observable to user
- [ ] User has reviewed and approved
