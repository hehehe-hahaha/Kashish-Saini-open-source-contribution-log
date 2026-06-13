# Contribution 200: Add compatibility test for $maxN (array element) 

**Contribution Number:** 200
**Student:** Kashish Saini  
**Issue:** [GitHub issue link](https://github.com/documentdb/functional-tests/issues/200)
**Status:** Phase II complete

---

## Why I Chose This Issue

I chose this issue because it's a well-scoped, beginner-friendly task that focuses on writing compatibility tests for the $maxN array expression operator in DocumentDB. As someone early in my open source journey, I wanted something where I could focus on learning the contribution workflow without getting overwhelmed by complex feature work. Writing tests felt like the right entry point.

This issue matches my learning goals because I want to get comfortable with Python testing patterns and understand how real-world projects structure their test suites. I'm hoping to learn how to read existing code to understand conventions, write meaningful edge case tests, and go through a full PR review cycle with actual maintainers.

---

## Understanding the Issue

### Problem Description

DocumentDB is a database that is designed to be compatible with MongoDB, meaning developers can use their MongoDB code and run it on DocumentDB without rewriting anything. To make sure both databases behave the same way, the project has a test suite that checks every operator works correctly.

The `$maxN` array expression operator only had one basic smoke test. That single test only covered a simple happy path where the array was clean, the numbers were positive, and `n` was a hardcoded number smaller than the array size. There was no coverage for edge cases that real applications would actually encounter.

### Expected Behavior

The `$maxN` operator should have a full compatibility test suite that proves it works correctly in all realistic scenarios including edge cases like empty arrays, null values, duplicate numbers, negative numbers, `n` being larger than the array, `n` being a dynamic field reference, and the operator being used inside a `$group` stage.

### Current Behavior

Only one smoke test exists that inserts two documents with clean arrays and checks that `$maxN` returns the 2 largest values. Nothing else is tested.

### Affected Components

The test file lives at:
`documentdb_tests/compatibility/tests/core/operator/expressions/array/maxN-array-element/`
The new compatibility test file was added to that same folder alongside the existing smoke test.

---

## Reproduction Process

### Environment Setup

Setting up the environment involved a few steps that were not immediately obvious.

Forked and cloned the repo locally, then installed the project dependencies using pip3. The scripts installed successfully but were not on PATH, so pre-commit had to be run using its full file path instead of the command directly.

MongoDB was not installed so it was installed via Homebrew. It runs as a background process on this machine rather than a system service, so it needs to be started manually after a Mac restart.

Overall the setup took some troubleshooting around PATH and MongoDB not being pre-installed, but the CONTRIBUTING.md was detailed enough to follow without getting stuck.

### Steps to Reproduce

1. Fork and clone the repo, then install dependencies
2. Start MongoDB and confirm it is running on port 27017
3. Create a working branch off of main
4. Navigate to the `maxN-array-element` folder and run the existing smoke test to confirm the basic setup works
5. The smoke test passes confirming the operator is functional in the basic case, but browsing the folder shows only one file exists with no edge cases covered

### Reproduction Evidence

- Branch link: https://github.com/hehehe-hahaha/functional-tests/tree/fix-issue-200
- My findings: The existing smoke test passes which confirms the operator is functional in the basic case. However browsing the test folder showed only one file existed covering one scenario. Compared to sibling operators like `firstN` and `lastN`, the `maxN` folder was clearly missing coverage. The issue is not that something is broken, it is that the test coverage is incomplete and real edge cases are untested.

---

## Solution Approach

### Analysis

The root cause is not a bug in the operator itself. The issue is that the test suite only has one smoke test that proves the operator does not crash in the simplest case. There is no compatibility test file that verifies the operator handles edge cases correctly. Without these tests, a regression or compatibility difference between MongoDB and DocumentDB could go unnoticed.

### Proposed Solution

Create a new compatibility test file in the same folder as the existing smoke test. The file covers 8 scenarios that go beyond the basic happy path and test realistic situations a developer would actually encounter.

### Implementation Plan

**Understand:**  
The `$maxN` array expression operator is missing compatibility test coverage. Only one smoke test exists. A full compatibility suite needs to prove the operator handles edge cases correctly and behaves the same way as MongoDB in every scenario.

**Match:**  
The existing smoke test at `maxN-array-element/test_smoke_expression_maxN-array-element.py` was used as the direct template for structure, imports, and conventions. The `minN-array-element` folder was also referenced as a parallel example since it is the sister operator with the same pattern.

**Plan:**
1. Create a new file `test_compatibility_expression_maxN.py` in the `maxN-array-element` folder
2. Add `@pytest.mark.aggregate` tag to every test function
3. Write 8 test functions covering the following scenarios:
   - `n` larger than the array size, should return all elements without crashing
   - `n = 1`, should return a single element array not a scalar value
   - Arrays with duplicate values, should keep duplicates and not deduplicate
   - Arrays with negative numbers, should handle sign correctly
   - Empty array input, should return an empty array not an error
   - Null values mixed into the array, nulls should be ignored
   - `n` as a dynamic field reference instead of a hardcoded number
   - `$maxN` used inside `$group` as an accumulator instead of `$project`
4. Run all tests locally to confirm they pass
5. Run pre-commit checks to confirm formatting and linting pass
6. Commit and push to the working branch

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
