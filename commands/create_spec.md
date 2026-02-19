# Create Software Specification

You are tasked with creating detailed software specifications (SPEC.md) and comprehensive evaluation definitions (tests.yaml) through deep analysis and iterative refinement. You produce specifications that are implementation-ready, language-agnostic, and durable enough to survive complete reimplementation.

The core philosophy: code is a disposable cache of understanding. The durable assets are the evaluations — invariants, properties, contracts, and behavioral checks — that let you delete a codebase and regenerate it with confidence. This skill produces those durable assets.

## Initial Response

When this command is invoked:

1. **Check if parameters were provided**:
   - If a description, requirements doc, or topic was provided as a parameter, acknowledge and proceed
   - If referencing existing code to extract a spec from, read those files first

2. **If no parameters provided**, respond with:
```
I'll help you create a detailed software specification with comprehensive evaluations.

Please provide:
1. What the software/library should do (high-level purpose)
2. Any existing requirements, PRDs, or reference implementations
3. Target languages or platforms (optional - specs are language-agnostic by default)
4. Any constraints or design principles to follow
5. System scope: is this a single library, or does it have multiple components/services?
6. Will this run in production? If so, what metrics matter for correctness?

I'll analyze your requirements and create a complete SPEC.md with matching tests.yaml covering all three evaluation tiers: durable behavioral tests, property-based invariants, and live evaluation criteria.
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

3. **When extracting from existing code, hunt for implicit invariants**:

   Most systems have invariants embedded in code that "just works" without anyone knowing why. This extraction is archaeological work and the most valuable part of spec creation. Look for:
   - Assertions and guard clauses that enforce properties
   - Validation logic that constrains inputs/outputs
   - Relationships between data that are maintained by convention
   - Ordering guarantees, uniqueness constraints, conservation laws
   - Properties that hold across ALL code paths (not just happy paths)

4. **Synthesize and clarify**:
   ```
   Based on my analysis, I understand we need to create a spec for:
   [Purpose summary]

   Key capabilities:
   - [Capability 1]
   - [Capability 2]

   System scope:
   - [ ] Single library/module
   - [ ] Multi-component system (needs interface contracts)
   - [ ] Production service (needs live evaluation criteria)

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

### Step 3: Invariant & Property Discovery

This step identifies the most durable evaluation artifacts — properties that must hold regardless of implementation language, architecture, or internal design.

1. **Identify system invariants** — properties that must ALWAYS be true:

   Think through these categories:
   - **Conservation**: What quantities are preserved? (e.g., "total balance across all accounts is constant")
   - **Monotonicity**: What only grows or only shrinks? (e.g., "event sequence numbers never decrease")
   - **Boundedness**: What has hard limits? (e.g., "balances never go negative", "queue depth never exceeds N")
   - **Consistency**: What relationships always hold between data? (e.g., "parent.children.count == children referencing parent")
   - **Idempotency**: What operations are safe to repeat? (e.g., "applying the same update twice has the same effect as once")
   - **Ordering**: What sequence guarantees exist? (e.g., "events maintain causal ordering")

2. **Identify behavioral properties** — universal truths about functions:

   For each function, ask:
   - **Round-trip**: Does `inverse(f(x)) == x`? (serialize/deserialize, encrypt/decrypt, encode/decode)
   - **Idempotency**: Does `f(f(x)) == f(x)`? (normalization, formatting, deduplication)
   - **Commutativity**: Does `f(a, b) == f(b, a)`? (merge operations, set operations)
   - **Associativity**: Does `f(f(a, b), c) == f(a, f(b, c))`? (aggregations, folds)
   - **Monotonicity**: Does `a <= b` imply `f(a) <= f(b)`? (scoring, ranking)
   - **Distributivity**: Does `f(a + b) == f(a) + f(b)`? (linear transformations)
   - **Identity**: Is there an `e` where `f(x, e) == x`? (zero elements, empty collections)

3. **Present discovered invariants and properties** for validation:
   ```
   I've identified these system invariants:

   - INV-001: [Property] — [Why this must always hold]
   - INV-002: [Property] — [Why this must always hold]

   And these behavioral properties:

   - PROP-001: [Function]: [Property, e.g., "normalize(normalize(x)) == normalize(x)"]
   - PROP-002: [Functions]: [Property, e.g., "deserialize(serialize(x)) == x"]

   These are the most durable part of the spec — they survive any reimplementation.
   Are these correct? Any missing?
   ```

### Step 4: Function/API Design

1. **Enumerate all functions/endpoints/components** the software needs

2. **For each function, define**:
   - Purpose (one sentence)
   - Arguments with abstract types
   - Return type
   - Behavior rules with precise thresholds
   - Edge cases and error conditions
   - Examples

3. **For multi-component systems, define interface contracts**:

   A contract specifies what crosses boundaries between components. Contracts survive reimplementation of either side.

   For each component boundary:
   - **Request schema**: Precise shape of what is sent
   - **Response schema**: Precise shape of what is returned
   - **Protocol**: HTTP, WebSocket, event bus, function call, etc.
   - **Invariants**: What must always hold about this interaction
   - **Error contract**: What error shapes are possible
   - **Versioning**: How contract changes are managed

4. **Spawn parallel analysis tasks**:

   For each major function, use **codebase-pattern-finder** or **web-search-researcher** to:
   - Find similar implementations to learn from
   - Identify edge cases others have handled
   - Discover threshold values or conventions used

5. **Present function designs** for each function:
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

   **Properties** (from Step 3):
   - [Property this function must satisfy]

   **Edge cases**:
   - [edge case]: [handling]

   **Errors**:
   - [error condition]: [error type]

   Does this capture the expected behavior?
   ```

### Step 5: Test Case Design

Tests are classified by **durability** — how long they remain valid as the implementation changes.

1. **Durable evaluations** (survive reimplementation — the real codebase):

   These are specified at boundaries that outlive any particular implementation. They are the most valuable tests.

   - **Invariant checks**: Verify system-wide properties hold (from Step 3 INV-*)
   - **Property-based tests**: Verify behavioral properties across generated inputs (from Step 3 PROP-*)
   - **Contract conformance**: Verify interface schemas and interaction guarantees
   - **End-to-end behavioral checks**: Given input X, the system produces output in class Y (internal path irrelevant)
   - **Boundary tests**: Test at exact threshold values (n-1, n, n+1)

2. **Ephemeral tests** (useful during development, disposable when implementation changes):

   These are coupled to a specific implementation. Write them freely; delete them without guilt when regenerating.

   - **Progression tests**: Cover each range/category with specific examples
   - **Input variation tests**: Different valid input formats
   - **Implementation-specific edge cases**: Platform-specific or language-specific quirks

   Mark these clearly so they can be discarded during reimplementation.

3. **For each function, design comprehensive test cases** covering both tiers:

   **Durable categories**:
   - **Invariant tests**: System-wide properties verified after this function runs
   - **Property tests**: Universal truths about this function's behavior (for all valid inputs)
   - **Boundary tests**: At exact threshold values (n-1, n, n+1)
   - **Error tests**: All documented error conditions
   - **Contract tests**: Interface schema conformance (for multi-component)

   **Ephemeral categories**:
   - **Example tests**: Specific input/output pairs for documentation
   - **Progression tests**: Representative values from each range
   - **Edge cases**: Empty inputs, zero values, max values, etc.

4. **Spawn analysis tasks** to find edge cases:

   Use **web-search-researcher** to find:
   - Common bugs in similar implementations
   - Edge cases others have missed
   - Input variations users actually try

5. **Generate test structure** following this pattern:
   ```yaml
   function_name:
     # DURABLE — Invariant checks
     - name: "invariant - [what property holds]"
       durability: durable
       input: { ... }
       assert: "[invariant expression]"

     # DURABLE — Boundary tests (most important for correctness)
     - name: "boundary - at threshold X"
       durability: durable
       input: { ... }
       output: "expected"

     # DURABLE — Error contract
     - name: "error - invalid input"
       durability: durable
       input: { ... }
       error: true

     # EPHEMERAL — Example progression
     - name: "example - middle of range Y"
       durability: ephemeral
       input: { ... }
       output: "expected"

     # EPHEMERAL — Edge cases
     - name: "edge - zero value"
       durability: ephemeral
       input: { ... }
       output: "expected"
   ```

### Step 6: Write SPEC.md

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

## System Invariants

Properties that must hold across ALL implementations, regardless of language, architecture, or internal design. These are the specification's most durable artifacts. An implementation that violates any invariant is incorrect by definition.

- **INV-001**: [Property that must always be true]. *Rationale: [why]*
- **INV-002**: [Property that must always be true]. *Rationale: [why]*
- **INV-003**: [Property that must always be true]. *Rationale: [why]*

### Verification

Each invariant should be verifiable by:
1. Running after any state-mutating operation
2. Running as a continuous production check
3. Including in property-based test suites with generated inputs

---

## Behavioral Properties

Universal truths about function behavior that hold for ALL valid inputs. These are specified as properties for use with generative testing frameworks (Hypothesis, fast-check, PropEr, etc.).

- **PROP-001**: `[function]`: [property in plain English]. *Formal: `[expression, e.g., deserialize(serialize(x)) == x for all valid x`]*
- **PROP-002**: `[function]`: [property in plain English]. *Formal: `[expression]`*
- **PROP-003**: `[functions]`: [property in plain English]. *Formal: `[expression]`*

---

## Interface Contracts *(include for multi-component systems)*

Contracts specify what crosses boundaries between components. A contract survives reimplementation of either side. Each contract is versioned independently.

### [Component A] → [Component B]

**Protocol**: [HTTP/WebSocket/event bus/function call]

**Request schema**:
```
{
  "field1": type (required) — description,
  "field2": type (optional) — description
}
```

**Response schema**:
```
{
  "field1": type — description,
  "field2": type — description
}
```

**Error schema**:
```
{
  "error": string — error code,
  "message": string — human-readable description
}
```

**Contract invariants**:
- [What must always hold about this interaction]
- [Ordering, consistency, or delivery guarantees]

**Versioning**: [How breaking changes are managed]

---

[Repeat for each component boundary]

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

**Properties:**
- [PROP-XXX]: [Brief restatement of the property this function satisfies]

**Rationale:** [Why these thresholds/behaviors were chosen]

**Edge cases:**
- [Edge case] → [Behavior]

---

[Repeat for each function]

---

## Evaluation Tiers

This specification defines three tiers of evaluation, each with a different lifetime and purpose.

### Tier 1: Durable Evaluations (survive reimplementation)

These are the real codebase. They encode what the system must do, independent of how any implementation does it. If you delete the implementation and regenerate it, these evaluations tell you whether the new code is correct.

- **Invariant checks**: Verify INV-* properties hold after operations
- **Property-based tests**: Verify PROP-* properties with generated inputs
- **Contract conformance**: Verify interface schemas (for multi-component)
- **End-to-end behavioral checks**: Given input, verify output class
- **Boundary tests**: Exact threshold verification (n-1, n, n+1)

### Tier 2: Ephemeral Tests (disposable with implementation)

Useful during development, coupled to a specific implementation. Delete without guilt when regenerating.

- **Example-based tests**: Specific input/output pairs
- **Progression tests**: Representative values from each range
- **Platform-specific edge cases**: Language or runtime quirks

### Tier 3: Live Evaluations (continuous in production)

These run continuously against production reality. Intent and reality can diverge even when all explicit tests pass.

- **Operational metrics**: Latency, error rates, throughput within acceptable ranges
- **Business invariants**: Domain-specific properties monitored continuously
- **Drift detection**: Baseline behavior compared against current behavior
- **Anomaly alerts**: Deviations that signal correctness problems

---

## Testing

### Test data format

Tests are defined in `tests.yaml` as language-agnostic evaluations organized by durability tier.

Structure:
```yaml
# Durable — input/output behavioral check
function_name:
  - name: "human-readable test name"
    durability: durable
    input: { ... }        # Function arguments
    output: "expected"    # Expected return value
    error: true           # Present only if function should error

# Durable — property-based (for generative testing frameworks)
properties:
  - name: "human-readable property name"
    functions: [func1, func2]
    for_all: { x: "constraint on generated input" }
    property: "expression that must hold"

# Durable — invariant checks
invariants:
  - name: "human-readable invariant name"
    after: [func1, func2]  # Check after these functions execute
    property: "expression that must hold"
```

### Using tests.yaml

Implementations MUST pass all tests.yaml test cases. The workflow:

1. **Parse tests.yaml** in your target language
2. **Implement durable evaluations first** — these define correctness
3. **For property-based tests**, use a generative testing framework:
   - Python: Hypothesis
   - TypeScript/JavaScript: fast-check
   - Rust: proptest
   - Erlang/Elixir: PropEr
   - Haskell: QuickCheck
4. **For invariant checks**, run the invariant assertion after each listed function
5. **Run tests** and iterate until all pass

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

## Live Evaluation Criteria *(include for production systems)*

These are not test-time assertions but production monitoring requirements. They encode correctness properties that must hold continuously in the real world.

### Operational Metrics

| Metric | Acceptable range | Alert threshold |
|--------|-----------------|-----------------|
| [e.g., p99 latency] | [e.g., < 200ms] | [e.g., > 500ms] |
| [e.g., error rate] | [e.g., < 0.1%] | [e.g., > 1%] |

### Business Invariants (monitored continuously)

Properties from the System Invariants section that should be verified continuously in production, not just at test time:

- [INV-XXX]: [How to monitor this in production]
- [INV-XXX]: [How to monitor this in production]

### Drift Detection

After any reimplementation or regeneration, compare:

- [Baseline metric 1]: [What normal looks like]
- [Baseline metric 2]: [What normal looks like]
- [Behavioral fingerprint]: [Key output distributions or patterns]

Flag any deviation beyond [acceptable threshold] for human review.

### Cost Metrics *(include for AI-assisted systems)*

| Metric | Baseline | Alert threshold |
|--------|----------|-----------------|
| [e.g., inference cost per request] | [value] | [threshold] |
| [e.g., token usage per operation] | [value] | [threshold] |

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

## Regeneration Confidence Checklist

Before considering the specification complete, apply this litmus test: if you deleted the entire implementation, could you regenerate it with confidence using only this spec and tests.yaml?

- [ ] All system invariants are explicit (not buried in implementation code)
- [ ] All behavioral properties are formally stated
- [ ] All interface contracts have precise schemas
- [ ] All functions have unambiguous behavior tables
- [ ] All boundary conditions have exact threshold values
- [ ] All error conditions are documented
- [ ] Property-based tests cover function composition behaviors
- [ ] Live evaluation criteria would catch drift after regeneration
- [ ] No critical behavior exists only as implicit knowledge

If any item fails, the spec has gaps that would make regeneration risky.

## Implementation Checklist

Before considering the implementation complete:

- [ ] All functions implemented
- [ ] All tests.yaml durable evaluations pass
- [ ] All property-based tests pass with generative framework
- [ ] All invariant checks pass
- [ ] All contract conformance tests pass (if multi-component)
- [ ] [Specific requirement 1]
- [ ] [Specific requirement 2]
- [ ] Errors are raised/returned idiomatically
- [ ] Code is idiomatic for target language
- [ ] usage.md generated
- [ ] Live evaluation monitoring configured (if production system)

---

## Version History

- **v0.1.0** - Initial specification
````

### Step 7: Write tests.yaml

1. **Use this template structure**:

```yaml
version: "0.1.0"

# Reference values used in tests:
# [Document key reference values, e.g., timestamps, constants]

# =============================================================================
# SYSTEM INVARIANTS — Properties that must hold after any state mutation
# These are the most durable evaluations. Check them after every operation.
# =============================================================================
invariants:
  - name: "invariant - [description of what must always be true]"
    after: [function1, function2]  # Verify after these functions run
    property: "[expression that must hold, e.g., 'total_balance == initial_total']"

  - name: "invariant - [description]"
    after: ["*"]  # Verify after ANY function
    property: "[expression]"

# =============================================================================
# BEHAVIORAL PROPERTIES — Universal truths for generative testing
# Use with Hypothesis, fast-check, PropEr, proptest, QuickCheck, etc.
# =============================================================================
properties:
  - name: "property - round-trip [description]"
    functions: [serialize, deserialize]
    for_all: { x: "any valid [type]" }
    property: "deserialize(serialize(x)) == x"

  - name: "property - idempotent [description]"
    functions: [normalize]
    for_all: { x: "any valid [type]" }
    property: "normalize(normalize(x)) == normalize(x)"

  - name: "property - [description]"
    functions: [func1, func2]
    for_all: { a: "constraint", b: "constraint" }
    property: "[expression that must hold for all valid a, b]"

# =============================================================================
# INTERFACE CONTRACTS — Schema conformance (for multi-component systems)
# =============================================================================
contracts:
  - name: "contract - [Component A] → [Component B] request shape"
    endpoint: "[identifier]"
    direction: request
    schema:
      required_fields:
        field1: { type: "string", constraints: "non-empty" }
        field2: { type: "integer", constraints: ">= 0" }
      optional_fields:
        field3: { type: "string", default: "value" }

  - name: "contract - [Component A] → [Component B] response shape"
    endpoint: "[identifier]"
    direction: response
    schema:
      required_fields:
        field1: { type: "string", constraints: "non-empty" }

# =============================================================================
# FUNCTION EVALUATIONS — Per-function behavioral checks
# =============================================================================
function_name:
  # DURABLE — Boundary tests at exact threshold values
  - name: "boundary - just below threshold X"
    durability: durable
    input: { field: value }
    output: "expected"

  - name: "boundary - exactly at threshold X"
    durability: durable
    input: { field: value }
    output: "expected"

  - name: "boundary - just above threshold X"
    durability: durable
    input: { field: value }
    output: "expected"

  # DURABLE — Error contract
  - name: "error - [description]"
    durability: durable
    input: { field: value }
    error: true

  # EPHEMERAL — Normal progression through ranges
  - name: "example - [description]"
    durability: ephemeral
    input: { field: value }
    output: "expected"

  # EPHEMERAL — Edge cases
  - name: "edge - zero/empty/min/max"
    durability: ephemeral
    input: { field: value }
    output: "expected"

  # EPHEMERAL — Special cases
  - name: "special - [description]"
    durability: ephemeral
    input: { field: value }
    output: "expected"


next_function:
  # ... same structure
```

2. **Test case naming conventions**:
   - `invariant - [what property holds]`
   - `property - [what universal truth]`
   - `contract - [what interface shape]`
   - `boundary - [what threshold]`
   - `error - [what error condition]`
   - `example - [what range/case]`
   - `edge - [what edge case]`
   - `special - [what special behavior]`

3. **Ensure comprehensive coverage by tier**:

   **Durable (MUST have)**:
   - Every system invariant has a check
   - Every behavioral property has a generative test definition
   - Every interface contract has schema conformance tests
   - Every threshold has boundary tests (value-1, value, value+1)
   - Every error condition has a test

   **Ephemeral (SHOULD have)**:
   - Every output category has at least one example
   - Common input variations are tested
   - Platform-specific edge cases documented

### Step 8: Review and Iterate

1. **Apply the Regeneration Confidence Test**:

   Before presenting the draft, mentally simulate: "If I deleted every line of implementation code and only had SPEC.md and tests.yaml, could I regenerate the system and know it works?"

   If the answer is no, identify what's missing:
   - Implicit invariants not yet captured?
   - Behavioral properties only described by example, not by universal rule?
   - Interface contracts missing precise schemas?
   - Live evaluation criteria that would catch silent drift?

2. **Present the draft files**:
   ```
   I've created:
   - SPEC.md: [path]
   - tests.yaml: [path]

   Evaluation coverage:
   - [N] system invariants
   - [N] behavioral properties (for generative testing)
   - [N] interface contracts (if multi-component)
   - [N] durable behavioral tests
   - [N] ephemeral example tests
   - [N] live evaluation criteria (if production system)

   Key decisions made:
   - [Decision 1 with rationale]
   - [Decision 2 with rationale]

   Regeneration confidence: [High/Medium/Low]
   - [What gives confidence]
   - [What gaps remain, if any]

   Please review:
   - Are the invariants correct and complete?
   - Are the behavioral properties actually universal?
   - Are the behavior thresholds correct?
   - Any missing edge cases?
   - Should any functions be added/removed?
   ```

3. **Iterate based on feedback**:
   - Update both files together (keep them in sync)
   - Re-validate test coverage after spec changes
   - Add new tests for any new behaviors
   - Re-run the regeneration confidence test after each iteration

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

5. **Evaluation durability matters**:
   - Prioritize durable evaluations over ephemeral ones
   - Every function should have at least one property-based test when possible
   - System invariants are the most valuable artifact — invest time discovering them
   - Mark test durability explicitly so teams know what to keep vs. discard

6. **The regeneration test is the quality bar**:
   - A spec is complete when you could delete the codebase and regenerate with confidence
   - If the spec feels incomplete, it probably is — keep digging for implicit invariants
   - When extracting from existing code, the hardest (and most valuable) work is making implicit knowledge explicit

7. **Track progress**:
   - Use TodoWrite to track specification tasks
   - Mark sections complete as they're finalized

## Example Invocations

```
/create_spec A library for validating email addresses
/create_spec Based on the requirements in prd.md, create a spec for the payment processing module
/create_spec Extract a spec from the existing implementation in src/utils/date.ts
/create_spec A multi-service system for real-time chat with message persistence
```

## Output Files

- `SPEC.md` - Full specification with invariants, properties, contracts, and live evaluation criteria
- `tests.yaml` - Comprehensive evaluations across all three durability tiers
