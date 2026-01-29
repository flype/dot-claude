# Create Software Specification

You are tasked with creating detailed software specifications (SPEC.md) and comprehensive test definitions (tests.yaml) through deep analysis and iterative refinement. You produce specifications that are implementation-ready and language-agnostic.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a description, requirements doc, or topic was provided as a parameter, acknowledge and proceed
   - If referencing existing code to extract a spec from, read those files first

2. **If no parameters provided**, respond with:
```
I'll help you create a detailed software specification with comprehensive tests.

Please provide:
1. What the software/library should do (high-level purpose)
2. Any existing requirements, PRDs, or reference implementations
3. Target languages or platforms (optional - specs are language-agnostic by default)
4. Any constraints or design principles to follow

I'll analyze your requirements and create a complete SPEC.md with matching tests.yaml.
```

Then wait for the user's input.

## Process Steps

### Step 1: Requirements Analysis

1. **Read all mentioned files completely**:
   - Requirements documents, PRDs, tickets
   - Existing implementations to extract patterns from
   - Reference specs or similar projects
   - **IMPORTANT**: Use Read tool WITHOUT limit/offset to read entire files

2. **Spawn parallel research tasks** to understand the domain:

   Use specialized agents for comprehensive research:

   - **codebase-pattern-finder**: Find similar implementations or libraries in the codebase
   - **web-search-researcher**: Research existing solutions, standards, and best practices
   - **codebase-analyzer**: If extracting from existing code, analyze the implementation

3. **Synthesize and clarify**:
   ```
   Based on my analysis, I understand we need to create a spec for:
   [Purpose summary]

   Key capabilities:
   - [Capability 1]
   - [Capability 2]

   Questions before I proceed:
   - [Clarification on scope or behavior]
   - [Design decision that affects the spec]
   ```

### Step 2: Design Principles Discovery

1. **Identify core constraints** that should govern the implementation:

   Spawn a **codebase-analyzer** or **web-search-researcher** to find:
   - Common design patterns for this type of software
   - Industry standards or conventions
   - Error handling patterns
   - Performance considerations

2. **Present proposed principles** for validation:
   ```
   I propose these design principles for the spec:

   1. [Principle] - [Rationale]
   2. [Principle] - [Rationale]
   3. [Principle] - [Rationale]

   Do these align with your expectations? Any to add or modify?
   ```

### Step 3: Function/API Design

1. **Enumerate all functions/endpoints/components** the software needs

2. **For each function, define**:
   - Purpose (one sentence)
   - Arguments with abstract types
   - Return type
   - Behavior rules with precise thresholds
   - Edge cases and error conditions
   - Examples

3. **Spawn parallel analysis tasks**:

   For each major function, use **codebase-pattern-finder** or **web-search-researcher** to:
   - Find similar implementations to learn from
   - Identify edge cases others have handled
   - Discover threshold values or conventions used

4. **Present function designs** for each function:
   ```
   ### Function: [name]

   **Purpose**: [one sentence]

   **Arguments**:
   - `arg1` (type): description
   - `arg2` (type, optional): description

   **Behavior**:
   | Condition | Output |
   |-----------|--------|
   | [condition 1] | [output 1] |
   | [condition 2] | [output 2] |

   **Edge cases**:
   - [edge case]: [handling]

   **Errors**:
   - [error condition]: [error type]

   Does this capture the expected behavior?
   ```

### Step 4: Test Case Design

1. **For each function, design comprehensive test cases**:

   **Categories to cover**:
   - **Boundary tests**: Test at exact threshold values (n-1, n, n+1)
   - **Progression tests**: Cover each range/category
   - **Error tests**: All documented error conditions
   - **Input variation tests**: Different valid input formats
   - **Edge cases**: Empty inputs, zero values, max values, etc.

2. **Spawn analysis tasks** to find edge cases:

   Use **web-search-researcher** to find:
   - Common bugs in similar implementations
   - Edge cases others have missed
   - Input variations users actually try

3. **Generate test structure** following this pattern:
   ```yaml
   function_name:
     # Boundary tests (most important for correctness)
     - name: "boundary - at threshold X"
       input: { ... }
       output: "expected"

     # Progression through ranges
     - name: "normal - middle of range Y"
       input: { ... }
       output: "expected"

     # Edge cases
     - name: "edge - zero value"
       input: { ... }
       output: "expected"

     # Error cases
     - name: "error - invalid input"
       input: { ... }
       error: true
   ```

### Step 5: Write SPEC.md

1. **Use this template structure**:

````markdown
# [Library Name] Specification v0.1.0

## Overview

[2-3 sentence description of what the library does and its core value proposition]

All functions are [key constraint, e.g., "pure—no side effects"]. The [key input type] is always passed explicitly.

## Design Principles

1. **[Principle name].** [Detailed explanation]

2. **[Principle name].** [Detailed explanation]

3. **[Principle name].** [Detailed explanation]

---

## Output Structure

Generate the minimal files needed to use and test the library. Do not create package distribution scaffolding.

**Do generate:**
- [Required output 1]
- [Required output 2]

**Do not generate:**
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

---

## Type Conventions

Since this spec targets multiple languages, types are described abstractly:

| Spec type | Meaning | Examples |
|-----------|---------|----------|
| `type1` | [Description] | [Examples] |
| `type2` | [Description] | [Examples] |

### [Type] normalization

When a function receives a `[type]`:
1. [Rule 1]
2. [Rule 2]

---

## Error Handling

Errors should be reported idiomatically for the target language:

| Language | Error style |
|----------|-------------|
| Python | Raise `ValueError` with descriptive message |
| TypeScript | Throw `Error` or return `null` (document which) |
| Rust | Return `Result<T, Error>` |
| Go | Return `(value, error)` tuple |

**Error conditions by function:**

| Function | Error when |
|----------|------------|
| `func1` | [Condition] |
| `func2` | [Condition] |

---

## [Domain-specific section, e.g., "Rounding and Boundaries"]

### [Subsection for specific rules]

[Precise rules with examples]

```
[threshold definitions or formulas]
```

---

## Functions

### function_name(arg1, arg2?) → return_type

[One sentence description]

**Arguments:**
- `arg1`: [Type] description
- `arg2`: Optional. [Type] description. Defaults to [default].

**Behavior:**

| Condition | Output |
|-----------|--------|
| [condition] | [output] |

**Rationale:** [Why these thresholds/behaviors were chosen]

**Edge cases:**
- [Edge case] → [Behavior]

---

[Repeat for each function]

---

## Testing

### Test data format

Tests are defined in `tests.yaml` as language-agnostic input/output pairs.

Structure:
```yaml
function_name:
  - name: "human-readable test name"
    input: { ... }        # Function arguments
    output: "expected"    # Expected return value
    error: true           # Present only if function should error
```

### Using tests.yaml

Implementations MUST pass all tests.yaml test cases. The workflow:

1. **Parse tests.yaml** in your target language
2. **Generate or write test cases** that call functions and assert results
3. **Run tests** and iterate until all pass

### Input field mapping

**[function1]:**
```yaml
input: { field1: <type>, field2: <type> }
```

**[function2]:**
```yaml
input: "<string>"  # Direct input, not an object
```

### Error test handling

For entries with `error: true`, assert the function raises/returns an error.

---

## Generated Documentation

Implementations MUST include a `usage.md` file documenting how to use the library.

### usage.md requirements

Include:
1. **Installation** — How to add the library
2. **Quick start** — Minimal code example
3. **Function reference** — Signature, params, examples for each function
4. **Error handling** — How errors are reported
5. **Type conversions** — What types are accepted

Keep it under 150 lines.

---

## Implementation Checklist

Before considering the implementation complete:

- [ ] All functions implemented
- [ ] All tests.yaml tests pass
- [ ] [Specific requirement 1]
- [ ] [Specific requirement 2]
- [ ] Errors are raised/returned idiomatically
- [ ] Code is idiomatic for target language
- [ ] usage.md generated

---

## Version History

- **v0.1.0** - Initial specification
````

### Step 6: Write tests.yaml

1. **Use this template structure**:

```yaml
version: "0.1.0"

# Reference values used in tests:
# [Document key reference values, e.g., timestamps, constants]

function_name:
  # Boundary tests - at exact threshold values
  - name: "boundary - just below threshold X"
    input: { field: value }
    output: "expected"

  - name: "boundary - exactly at threshold X"
    input: { field: value }
    output: "expected"

  - name: "boundary - just above threshold X"
    input: { field: value }
    output: "expected"

  # Normal progression through ranges
  - name: "normal - [description]"
    input: { field: value }
    output: "expected"

  # Edge cases
  - name: "edge - zero/empty/min/max"
    input: { field: value }
    output: "expected"

  # Future/inverse/special cases
  - name: "special - [description]"
    input: { field: value }
    output: "expected"

  # Error cases
  - name: "error - [description]"
    input: { field: value }
    error: true


next_function:
  # ... same structure
```

2. **Test case naming conventions**:
   - `boundary - [what threshold]`
   - `normal - [what range/case]`
   - `edge - [what edge case]`
   - `special - [what special behavior]`
   - `error - [what error condition]`

3. **Ensure comprehensive coverage**:
   - Every threshold in the spec has boundary tests (value-1, value, value+1)
   - Every output category has at least one test
   - Every error condition has a test
   - Common input variations are tested

### Step 7: Review and Iterate

1. **Present the draft files**:
   ```
   I've created:
   - SPEC.md: [path]
   - tests.yaml: [path]

   Key decisions made:
   - [Decision 1 with rationale]
   - [Decision 2 with rationale]

   Please review:
   - Are the behavior thresholds correct?
   - Any missing edge cases?
   - Should any functions be added/removed?
   ```

2. **Iterate based on feedback**:
   - Update both files together (keep them in sync)
   - Re-validate test coverage after spec changes
   - Add new tests for any new behaviors

## Important Guidelines

1. **Precision is critical**:
   - Every threshold must have exact numeric values
   - Every behavior must be unambiguous
   - Tests must cover exact boundary conditions

2. **Language-agnostic design**:
   - Use abstract types that map to any language
   - Document language-specific error handling
   - Avoid features tied to one language

3. **Test-spec synchronization**:
   - Every spec behavior must have corresponding tests
   - Every test must trace to a spec requirement
   - Update both when either changes

4. **Be comprehensive but minimal**:
   - Include all necessary behaviors
   - Exclude implementation details
   - Don't over-specify (leave room for idiomatic implementations)

5. **Track progress**:
   - Use TodoWrite to track specification tasks
   - Mark sections complete as they're finalized

## Example Invocations

```
/create_spec A library for validating email addresses
/create_spec Based on the requirements in prd.md, create a spec for the payment processing module
/create_spec Extract a spec from the existing implementation in src/utils/date.ts
```

## Output Files

- `SPEC.md` - Full specification document
- `tests.yaml` - Comprehensive test cases
