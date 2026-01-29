# Test Desiderata Skill

A Claude Code skill for analyzing and improving test quality using Kent Beck's Test Desiderata framework.

## Overview

This skill helps Claude Code evaluate test code across 12 quality dimensions and provide actionable recommendations for improvement. It's designed to work with any testing framework and programming language.

## What is Test Desiderata?

Test Desiderata is a framework created by Kent Beck that identifies 12 properties that make tests more valuable:

1. **Isolated** - Tests don't affect each other
2. **Composable** - Test components can be combined and reused
3. **Deterministic** - Same input always produces same result
4. **Fast** - Tests run quickly
5. **Writable** - Tests are easy to write
6. **Readable** - Tests are easy to understand
7. **Behavioral** - Tests verify behavior, not implementation
8. **Structure-insensitive** - Tests survive refactoring
9. **Automated** - Tests run without human intervention
10. **Specific** - Failures clearly indicate the problem
11. **Predictive** - Passing tests mean production-ready code
12. **Inspiring** - Tests build confidence in the system

## When to Use This Skill

The skill activates automatically when you:
- Analyze test files
- Review test code
- Ask about test quality or best practices
- Request suggestions for test improvements

You can also explicitly invoke it with `/test-desiderata`

## Usage Examples

### Analyze Existing Tests

```
"Review the tests in tests/user_service_test.py and identify quality issues"
```

Claude will evaluate the tests against all 12 Test Desiderata properties and provide specific feedback.

### Check Specific Properties

```
"Are these tests well isolated?"
"How can I make these tests faster without sacrificing coverage?"
```

### Get Improvement Suggestions

```
"These tests are failing intermittently. What's wrong?"
"Suggest improvements for test readability"
```

### Understand Tradeoffs

```
"I want comprehensive tests but they're too slow. What should I do?"
```

Claude will explain which Test Desiderata properties conflict and suggest design patterns to resolve the tension.

## What You'll Get

When you use this skill, Claude provides:

1. **Specific Issues** - Exact code locations with problems
2. **Property Violations** - Which Test Desiderata principles are violated
3. **Impact Analysis** - Why the issue matters
4. **Concrete Fixes** - Actionable code examples
5. **Tradeoff Discussion** - When improvements conflict with other properties

Example output:
```
Issue: Test "test_user_creation" violates the Isolated property
Location: Line 45 - shares database connection across tests
Impact: Test results depend on execution order, causing intermittent failures
Fix: Use fresh database connection per test with proper cleanup
Tradeoff: Slightly slower but much more reliable
```

## Skill Contents

- **`SKILL.md`** - Main skill prompt with all 12 Test Desiderata properties
- **`reference.md`** - Detailed examples showing good vs. bad patterns for each property
- **`LICENSE.txt`** - Attribution to Kent Beck

## Philosophy

From Kent Beck:

> "Some properties support each other, some interfere with each other, and sometimes properties only SEEM to interfere - that's where good design makes the difference."

This skill helps you find those design opportunities where you can improve multiple properties simultaneously.

## Attribution

**All Test Desiderata concepts and principles are created by Kent Beck.**

Original resources:
- Website: [testdesiderata.com](https://testdesiderata.com/)
- Original essay: [Test Desiderata](https://medium.com/@kentbeck_7670/test-desiderata-94150638a4b3)
- Follow-up: [Programmer Test Principles](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934)
- Videos: Each property has a 5-minute explanatory video on YouTube

This skill applies Kent Beck's principles to help you write better tests.

## Installation

### Project Installation
```bash
# From your project root
mkdir -p .claude/skills
cp -r path/to/test-desiderata .claude/skills/
```

### Personal Installation
```bash
# Available across all projects
mkdir -p ~/.claude/skills
cp -r path/to/test-desiderata ~/.claude/skills/
```

## License

MIT License with attribution to Kent Beck as creator of the Test Desiderata framework.

See [LICENSE.txt](LICENSE.txt) for full details.
