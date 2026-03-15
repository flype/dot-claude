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
1. What the software/service should do (high-level purpose)
2. What problem does it solve? (the "why", not just the "what")
3. Any existing requirements, PRDs, or reference implementations
4. Target languages or platforms (optional - specs are language-agnostic by default)
5. Any constraints or design principles to follow
6. System scope:
   - Single library/module?
   - Multi-component system? (needs interface contracts, domain model)
   - Long-running service? (needs state machines, failure model, observability)
   - Production service? (needs live evaluation criteria, security, configuration)
7. What is explicitly OUT of scope? (non-goals)

I'll analyze your requirements and create a complete SPEC.md with matching tests.yaml covering all evaluation tiers: durable behavioral tests, property-based invariants, and live evaluation criteria.
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
   - State machines embedded in if/switch logic
   - Configuration defaults and validation scattered across files
   - Error handling patterns and recovery behaviors

4. **Synthesize and clarify**:
   ```
   Based on my analysis, I understand we need to create a spec for:
   [Purpose summary]

   Problem it solves:
   - [Problem 1]
   - [Problem 2]

   Key capabilities:
   - [Capability 1]
   - [Capability 2]

   Important boundaries (what this system is NOT):
   - [Boundary 1: e.g., "This is a scheduler, not a workflow engine"]
   - [Boundary 2: e.g., "Reads from tracker but does not write to it"]

   System scope:
   - [ ] Single library/module
   - [ ] Multi-component system (needs interface contracts, domain model)
   - [ ] Stateful service (needs state machines, failure model)
   - [ ] Production service (needs live evaluation, security, observability)

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

### Step 3: Domain Model Discovery *(for multi-component or stateful systems)*

Before designing functions, define the data the system operates on. The domain model is the shared vocabulary that makes the rest of the spec unambiguous.

1. **Identify all entities** the system tracks or processes:

   For each entity, define:
   - Field name, abstract type, and constraints
   - Which fields are required vs. optional
   - Nullability semantics
   - Normalization rules (e.g., "labels are normalized to lowercase", "identifiers are sanitized to `[A-Za-z0-9._-]`")

2. **Identify stable identifiers and keys**:
   - What uniquely identifies each entity?
   - What is used for human display vs. internal lookup?
   - How are identifiers derived or sanitized?

3. **Identify relationships between entities**:
   - One-to-many, many-to-many
   - Ownership and lifecycle dependencies
   - Referential integrity rules

4. **Present domain model** for validation:
   ```
   I've identified these domain entities:

   - [Entity 1]: [Purpose]. Fields: [key fields list]
   - [Entity 2]: [Purpose]. Fields: [key fields list]

   Key relationships:
   - [Entity 1] owns many [Entity 2]
   - [Entity 3] references [Entity 1] by [field]

   Normalization rules:
   - [Rule 1]
   - [Rule 2]

   Are these entities correct and complete?
   ```

### Step 4: State Machine Discovery *(for stateful systems)*

Systems with lifecycle states need explicit state machines. Behavior tables alone cannot capture state transitions, guards, and triggers.

1. **Identify all state dimensions**:

   A system may have multiple independent state machines. For example:
   - Issue orchestration state (Unclaimed → Claimed → Running → Released)
   - Run attempt lifecycle (Preparing → Launching → Streaming → Succeeded/Failed)
   - Connection state (Disconnected → Connecting → Connected → Reconnecting)

2. **For each state machine, define**:
   - All possible states (including terminal states)
   - All transition triggers (what causes a state change)
   - Guards/preconditions for each transition
   - Side effects of each transition
   - Recovery/reset paths

3. **Present state machines** for validation:
   ```
   I've identified these state machines:

   [State Machine 1: Name]
   States: [State A] → [State B] → [State C] → [Terminal]
   Triggers: [trigger list]

   Key transitions:
   - [State A] → [State B]: triggered by [X], requires [guard]
   - [State B] → [State C]: triggered by [Y]
   - [State B] → [Terminal]: triggered by [Z] (error path)

   Are these states and transitions correct?
   ```

### Step 5: Invariant & Property Discovery

This step identifies the most durable evaluation artifacts — properties that must hold regardless of implementation language, architecture, or internal design.

1. **Identify system invariants** — properties that must ALWAYS be true:

   Think through these categories:
   - **Conservation**: What quantities are preserved? (e.g., "total balance across all accounts is constant")
   - **Monotonicity**: What only grows or only shrinks? (e.g., "event sequence numbers never decrease")
   - **Boundedness**: What has hard limits? (e.g., "balances never go negative", "queue depth never exceeds N")
   - **Consistency**: What relationships always hold between data? (e.g., "parent.children.count == children referencing parent")
   - **Idempotency**: What operations are safe to repeat? (e.g., "applying the same update twice has the same effect as once")
   - **Ordering**: What sequence guarantees exist? (e.g., "events maintain causal ordering")
   - **Containment**: What must stay within boundaries? (e.g., "workspace path must be under workspace root")
   - **Uniqueness**: What must never be duplicated? (e.g., "no two running tasks for the same issue ID")

2. **Identify behavioral properties** — universal truths about functions:

   For each function, ask:
   - **Round-trip**: Does `inverse(f(x)) == x`? (serialize/deserialize, encrypt/decrypt, encode/decode)
   - **Idempotency**: Does `f(f(x)) == f(x)`? (normalization, formatting, deduplication)
   - **Commutativity**: Does `f(a, b) == f(b, a)`? (merge operations, set operations)
   - **Associativity**: Does `f(f(a, b), c) == f(a, f(b, c))`? (aggregations, folds)
   - **Monotonicity**: Does `a <= b` imply `f(a) <= f(b)`? (scoring, ranking)
   - **Distributivity**: Does `f(a + b) == f(a) + f(b)`? (linear transformations)
   - **Identity**: Is there an `e` where `f(x, e) == x`? (zero elements, empty collections)

3. **Identify safety invariants** — the most critical subset:

   Safety invariants prevent dangerous states. They are the spec's highest-priority artifacts:
   - Filesystem containment (paths stay within allowed roots)
   - Resource isolation (processes only access their own data)
   - Input sanitization (identifiers are cleaned before use)
   - Secret protection (tokens are never logged or exposed)

4. **Present discovered invariants and properties** for validation:
   ```
   I've identified these system invariants:

   - INV-001: [Property] — [Why this must always hold]
   - INV-002: [Property] — [Why this must always hold]

   Safety invariants (highest priority):
   - SAFE-001: [Safety property] — [What it prevents]

   And these behavioral properties:

   - PROP-001: [Function]: [Property, e.g., "normalize(normalize(x)) == normalize(x)"]
   - PROP-002: [Functions]: [Property, e.g., "deserialize(serialize(x)) == x"]

   These are the most durable part of the spec — they survive any reimplementation.
   Are these correct? Any missing?
   ```

### Step 6: Function/API Design

1. **Enumerate all functions/endpoints/components** the software needs

2. **For each function, define**:
   - Purpose (one sentence)
   - Arguments with abstract types
   - Return type
   - Behavior rules with precise thresholds
   - Edge cases and error conditions
   - Examples

3. **For complex behaviors, write reference algorithms in pseudocode**:

   Behavior tables work for simple functions. For orchestration logic, loops, multi-step procedures, or state machine transitions, pseudocode is clearer and less ambiguous:
   ```text
   function do_thing(input):
     validated = validate(input)
     if validated failed:
       return error("invalid input")

     result = process(validated)
     notify_observers(result)
     return result
   ```

   Guidelines for pseudocode:
   - Language-agnostic (no language-specific syntax)
   - Focus on control flow and decision logic
   - Include error paths explicitly
   - Reference domain model entities by name
   - Cross-reference invariants that must hold at each step

4. **For multi-component systems, define interface contracts**:

   A contract specifies what crosses boundaries between components. Contracts survive reimplementation of either side.

   For each component boundary:
   - **Request schema**: Precise shape of what is sent
   - **Response schema**: Precise shape of what is returned
   - **Protocol**: HTTP, WebSocket, event bus, function call, etc.
   - **Invariants**: What must always hold about this interaction
   - **Error contract**: What error shapes are possible
   - **Versioning**: How contract changes are managed

5. **Spawn parallel analysis tasks**:

   For each major function, use **codebase-pattern-finder** or **web-search-researcher** to:
   - Find similar implementations to learn from
   - Identify edge cases others have handled
   - Discover threshold values or conventions used

6. **Present function designs** for each function:
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

   **Properties** (from Step 5):
   - [Property this function must satisfy]

   **Edge cases**:
   - [edge case]: [handling]

   **Errors**:
   - [error condition]: [error type]

   Does this capture the expected behavior?
   ```

### Step 7: Configuration, Failure Model & Security *(for services/production systems)*

For systems beyond simple libraries, three additional concerns must be specified:

1. **Configuration specification**:
   - What settings exist and their defaults?
   - Source precedence (file → environment → defaults)
   - Dynamic reload behavior (what can change at runtime vs. requiring restart?)
   - Validation rules (what makes a config invalid?)
   - Environment variable indirection (e.g., `$VAR_NAME` syntax)

2. **Failure model**:
   - Categorize all failure classes (config errors, network errors, subprocess errors, etc.)
   - For each class, define recovery behavior (retry, skip, crash, degrade)
   - Define operator intervention points (how humans fix things)
   - Define restart/recovery semantics (what state survives a restart?)

3. **Security and safety**:
   - Trust boundary assumptions (what inputs are trusted?)
   - Secret handling (how are tokens stored, resolved, and protected from logging?)
   - Filesystem safety (path containment, sanitization)
   - Input validation at system boundaries

4. **Present for validation**:
   ```
   Configuration:
   - [N] configurable settings with defaults
   - Dynamic reload: [what changes at runtime]
   - Validation: [what blocks startup]

   Failure model:
   - [N] failure classes identified
   - Recovery strategy: [summary]
   - Restart semantics: [what survives restart]

   Security:
   - Trust boundary: [summary]
   - Secret handling: [approach]
   - Safety invariants: [references to SAFE-* from Step 5]

   Are these appropriate for the deployment context?
   ```

### Step 8: Test Case Design

Tests are classified along two orthogonal dimensions:

**Durability** — how long tests remain valid:
- **Durable**: Survive reimplementation (the real codebase)
- **Ephemeral**: Useful during development, disposable when implementation changes

**Validation profile** — when/where tests run:
- **Core Conformance**: Deterministic tests required for all implementations
- **Extension Conformance**: Required only for optional features an implementation ships
- **Real Integration**: Environment-dependent checks needing credentials/network access

1. **Durable evaluations** (survive reimplementation — the real codebase):

   These are specified at boundaries that outlive any particular implementation. They are the most valuable tests.

   - **Invariant checks**: Verify system-wide properties hold (from Step 5 INV-*)
   - **Safety checks**: Verify safety invariants hold (from Step 5 SAFE-*)
   - **Property-based tests**: Verify behavioral properties across generated inputs (from Step 5 PROP-*)
   - **Contract conformance**: Verify interface schemas and interaction guarantees
   - **End-to-end behavioral checks**: Given input X, the system produces output in class Y (internal path irrelevant)
   - **Boundary tests**: Test at exact threshold values (n-1, n, n+1)
   - **State machine tests**: Verify valid transitions and reject invalid ones

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
   - **State transition tests**: Valid and invalid state transitions (for stateful systems)

   **Ephemeral categories**:
   - **Example tests**: Specific input/output pairs for documentation
   - **Progression tests**: Representative values from each range
   - **Edge cases**: Empty inputs, zero values, max values, etc.

4. **Assign validation profiles**:

   A test can be durable AND only runnable in the Real Integration profile. Assign profiles:
   - **Core Conformance**: Can run without external dependencies (most tests)
   - **Extension Conformance**: Tests for optional features, skipped if feature not implemented
   - **Real Integration**: Needs live credentials, network, or external services

5. **Spawn analysis tasks** to find edge cases:

   Use **web-search-researcher** to find:
   - Common bugs in similar implementations
   - Edge cases others have missed
   - Input variations users actually try

6. **Generate test structure** following this pattern:
   ```yaml
   function_name:
     # DURABLE — Invariant checks
     - name: "invariant - [what property holds]"
       durability: durable
       profile: core
       input: { ... }
       assert: "[invariant expression]"

     # DURABLE — Boundary tests (most important for correctness)
     - name: "boundary - at threshold X"
       durability: durable
       profile: core
       input: { ... }
       output: "expected"

     # DURABLE — Error contract
     - name: "error - invalid input"
       durability: durable
       profile: core
       input: { ... }
       error: true

     # DURABLE — Real integration
     - name: "integration - live API call"
       durability: durable
       profile: real_integration
       input: { ... }
       output: "expected"

     # EPHEMERAL — Example progression
     - name: "example - middle of range Y"
       durability: ephemeral
       profile: core
       input: { ... }
       output: "expected"

     # EPHEMERAL — Edge cases
     - name: "edge - zero value"
       durability: ephemeral
       profile: core
       input: { ... }
       output: "expected"
   ```

### Step 9: Write SPEC.md

1. **Use numbered sections** for navigability and cross-referencing. For specs longer than ~200 lines, numbered sections are essential.

2. **Use this template structure**:

````markdown
# [System Name] Specification

Status: Draft v0.1.0 (language-agnostic)

Purpose: [One sentence: what this system does and why it exists]

## 1. Problem Statement

[What problem does this system solve? Why does it need to exist? What operational problems does it address?]

- [Problem 1 it solves]
- [Problem 2 it solves]
- [Problem 3 it solves]

Important boundary:

- [What this system IS: e.g., "X is a scheduler/runner and tracker reader."]
- [What this system is NOT: e.g., "X does not manage user authentication."]
- [Responsibility limit: e.g., "Ticket writes are performed by the agent, not the orchestrator."]

## 2. Goals and Non-Goals

### 2.1 Goals

- [Goal 1: concrete, testable outcome]
- [Goal 2]
- [Goal 3]

### 2.2 Non-Goals

- [Non-goal 1: what this system explicitly does not do]
- [Non-goal 2]
- [Non-goal 3]

## 3. System Overview

### 3.1 Main Components

1. `[Component 1]`
   - [Responsibility]
   - [Key behavior]

2. `[Component 2]`
   - [Responsibility]
   - [Key behavior]

### 3.2 Abstraction Layers *(include for layered systems)*

The system is organized in these layers:

1. `[Layer 1]` ([scope])
   - [What lives here]

2. `[Layer 2]` ([scope])
   - [What lives here]

### 3.3 External Dependencies

- [Dependency 1: what it provides]
- [Dependency 2: what it provides]

## 4. Design Principles

1. **[Principle name].** [Detailed explanation]

2. **[Principle name].** [Detailed explanation]

3. **[Principle name].** [Detailed explanation]

---

## 5. Core Domain Model

### 5.1 Entities

#### 5.1.1 [Entity Name]

[One sentence: what this entity represents]

Fields:

- `field_name` (type)
  - [Description and constraints]
- `field_name` (type or null)
  - [Description, nullability semantics]

#### 5.1.2 [Entity Name]

[Repeat for each entity]

### 5.2 Stable Identifiers and Normalization Rules

- `[Identifier 1]`
  - [How it's used, where it comes from]
- `[Identifier 2]`
  - [Derivation rule, e.g., "Replace any character not in `[A-Za-z0-9._-]` with `_`"]
- `[Normalized Value]`
  - [Normalization rule, e.g., "Compare after `lowercase`"]

---

## 6. System Invariants

Properties that must hold across ALL implementations, regardless of language, architecture, or internal design. These are the specification's most durable artifacts. An implementation that violates any invariant is incorrect by definition.

### 6.1 Safety Invariants *(highest priority)*

These prevent dangerous states. Violations are security or correctness bugs:

- **SAFE-001**: [Safety property]. *Rationale: [what it prevents]*
- **SAFE-002**: [Safety property]. *Rationale: [what it prevents]*

### 6.2 System Invariants

- **INV-001**: [Property that must always be true]. *Rationale: [why]*
- **INV-002**: [Property that must always be true]. *Rationale: [why]*
- **INV-003**: [Property that must always be true]. *Rationale: [why]*

### 6.3 Verification

Each invariant should be verifiable by:
1. Running after any state-mutating operation
2. Running as a continuous production check (see Section [N]: Live Evaluation)
3. Including in property-based test suites with generated inputs

---

## 7. Behavioral Properties

Universal truths about function behavior that hold for ALL valid inputs. These are specified as properties for use with generative testing frameworks (Hypothesis, fast-check, PropEr, etc.).

- **PROP-001**: `[function]`: [property in plain English]. *Formal: `[expression, e.g., deserialize(serialize(x)) == x for all valid x`]*
- **PROP-002**: `[function]`: [property in plain English]. *Formal: `[expression]`*
- **PROP-003**: `[functions]`: [property in plain English]. *Formal: `[expression]`*

---

## 8. State Machines *(include for stateful systems)*

### 8.1 [State Machine Name] States

This is the system's internal state, not external/user-visible state.

1. `[State A]`
   - [What this state means]

2. `[State B]`
   - [What this state means]

3. `[State C]` (terminal)
   - [What this state means]

### 8.2 Transition Triggers

- `[Trigger 1]`
  - [What happens, what state changes result]

- `[Trigger 2]`
  - [What happens, what state changes result]

### 8.3 Idempotency and Recovery Rules

- [Rule about state mutation authority]
- [Rule about duplicate prevention]
- [Rule about restart recovery]

---

## 9. Interface Contracts *(include for multi-component systems)*

Contracts specify what crosses boundaries between components. A contract survives reimplementation of either side. Each contract is versioned independently.

### 9.1 [Component A] → [Component B]

**Protocol**: [HTTP/WebSocket/event bus/function call/stdio]

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

## 10. Configuration Specification *(include for configurable systems)*

### 10.1 Source Precedence

Configuration values are resolved in this order (first wins):

1. [Highest precedence source, e.g., "CLI arguments"]
2. [Next source, e.g., "Configuration file values"]
3. [Next source, e.g., "Environment variable indirection via `$VAR_NAME`"]
4. [Lowest: "Built-in defaults"]

### 10.2 Configuration Fields

- `[section.field]`: type, default `[value]`
  - [Description and constraints]
  - [Dynamic reload behavior: "Changes apply at runtime" or "Requires restart"]

### 10.3 Dynamic Reload Semantics *(include for long-running services)*

- [How config changes are detected, e.g., "File watch on config file"]
- [What applies immediately vs. what needs restart]
- [Invalid reload behavior: "Keep last known good config, emit operator-visible error"]

### 10.4 Validation Rules

Startup validation (blocks startup if failed):
- [Validation check 1]
- [Validation check 2]

Per-operation validation (skips operation if failed):
- [Validation check 1]

### 10.5 Config Quick Reference *(intentionally redundant for implementor convenience)*

This section consolidates all config fields for quick implementation reference:

- `[field]`: type, default `[value]`, [dynamic/static]
- `[field]`: type, default `[value]`, [dynamic/static]

---

## 11. Output Structure

Generate the minimal files needed to use and test the system. Do not create package distribution scaffolding.

**Do generate:**
- [Required output 1]
- [Required output 2]

**Do not generate:**
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

---

## 12. Type Conventions

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

## 13. Error Handling

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

## 14. [Domain-specific section, e.g., "Rounding and Boundaries"]

### [Subsection for specific rules]

[Precise rules with examples]

```
[threshold definitions or formulas]
```

---

## 15. Functions

### 15.1 function_name(arg1, arg2?) → return_type

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

## 16. Reference Algorithms *(include for complex behaviors)*

Language-agnostic pseudocode for behaviors that cannot be fully captured in behavior tables.

### 16.1 [Algorithm Name]

```text
function [name](args):
  [step 1]
  if [condition]:
    [step 2a]
  else:
    [step 2b]

  [step 3]
  return [result]
```

### 16.2 [Algorithm Name]

```text
[pseudocode]
```

---

## 17. Failure Model and Recovery *(include for services)*

### 17.1 Failure Classes

1. `[Failure Class 1, e.g., "Configuration Failures"]`
   - [Specific failure 1]
   - [Specific failure 2]

2. `[Failure Class 2, e.g., "Network Failures"]`
   - [Specific failure 1]
   - [Specific failure 2]

3. `[Failure Class 3, e.g., "Subprocess Failures"]`
   - [Specific failure 1]
   - [Specific failure 2]

### 17.2 Recovery Behavior

- [Failure class 1] failures:
  - [Recovery action, e.g., "Skip dispatch, keep service alive, continue other operations"]

- [Failure class 2] failures:
  - [Recovery action, e.g., "Retry with exponential backoff"]

- [Failure class 3] failures:
  - [Recovery action, e.g., "Log and skip, try again next cycle"]

### 17.3 Restart Recovery

After restart:
- [What state is NOT restored, e.g., "No retry timers restored from prior process"]
- [How the system recovers, e.g., "Fresh polling of active items, re-dispatch eligible work"]
- [Cleanup behavior, e.g., "Startup sweep removes stale artifacts"]

### 17.4 Operator Intervention Points

Operators control behavior by:
- [Intervention 1, e.g., "Editing config file (auto-detected, no restart needed)"]
- [Intervention 2, e.g., "Changing external state (triggers reconciliation on next cycle)"]
- [Intervention 3, e.g., "Restarting the service (for process recovery or deployment)"]

---

## 18. Security and Safety *(include for production systems)*

### 18.1 Trust Boundary

[State the trust assumptions clearly:
- What inputs are trusted vs. untrusted?
- What execution environment is assumed?
- What approval/sandbox posture does the system require?]

### 18.2 Filesystem Safety

Mandatory:
- [Safety requirement, e.g., "Output paths must remain under configured root"]
- [Safety requirement, e.g., "Directory names must use sanitized identifiers"]

### 18.3 Secret Handling

- [How secrets are resolved, e.g., "Support `$VAR` indirection in config"]
- [Protection rule, e.g., "Do not log API tokens or secret env values"]
- [Validation rule, e.g., "Validate presence of secrets without printing them"]

### 18.4 Hardening Guidance

[Recommendations for deployment-specific hardening beyond baseline requirements]

---

## 19. Observability *(include for services)*

### 19.1 Logging Conventions

Required context fields for logs:
- `[field_1]` — [what it identifies]
- `[field_2]` — [what it identifies]

Message formatting:
- [Convention, e.g., "Use stable `key=value` phrasing"]
- [Convention, e.g., "Include action outcome: completed, failed, retrying"]

### 19.2 Logging Outputs

- [Where logs go, e.g., "Operators must see failures without attaching a debugger"]
- [Failure behavior, e.g., "Log sink failures do not crash the service"]

### 19.3 Monitoring Interface *(optional but recommended)*

If the implementation exposes a monitoring interface, it should return:
- [Data point 1, e.g., "running sessions list"]
- [Data point 2, e.g., "aggregate metrics"]
- [Data point 3, e.g., "health/error indicators"]

---

## 20. Evaluation Tiers

This specification defines three tiers of evaluation, each with a different lifetime and purpose.

### 20.1 Tier 1: Durable Evaluations (survive reimplementation)

These are the real codebase. They encode what the system must do, independent of how any implementation does it. If you delete the implementation and regenerate it, these evaluations tell you whether the new code is correct.

- **Safety checks**: Verify SAFE-* properties hold (highest priority)
- **Invariant checks**: Verify INV-* properties hold after operations
- **Property-based tests**: Verify PROP-* properties with generated inputs
- **State machine tests**: Verify valid transitions and reject invalid ones
- **Contract conformance**: Verify interface schemas (for multi-component)
- **End-to-end behavioral checks**: Given input, verify output class
- **Boundary tests**: Exact threshold verification (n-1, n, n+1)

### 20.2 Tier 2: Ephemeral Tests (disposable with implementation)

Useful during development, coupled to a specific implementation. Delete without guilt when regenerating.

- **Example-based tests**: Specific input/output pairs
- **Progression tests**: Representative values from each range
- **Platform-specific edge cases**: Language or runtime quirks

### 20.3 Tier 3: Live Evaluations (continuous in production)

These run continuously against production reality. Intent and reality can diverge even when all explicit tests pass.

- **Operational metrics**: Latency, error rates, throughput within acceptable ranges
- **Business invariants**: Domain-specific properties monitored continuously
- **Drift detection**: Baseline behavior compared against current behavior
- **Anomaly alerts**: Deviations that signal correctness problems

---

## 21. Validation Profiles

Tests are also classified by when/where they can run:

### 21.1 Core Conformance

Deterministic tests required for all conforming implementations. No external dependencies needed.

- [Section reference]: [What is tested]
- [Section reference]: [What is tested]

### 21.2 Extension Conformance

Required only for optional features that an implementation chooses to ship.

- If [feature] is implemented: [what must be tested]
- If [feature] is implemented: [what must be tested]

### 21.3 Real Integration Profile (Recommended)

Environment-dependent checks recommended before production use. May be skipped in CI when credentials are unavailable.

- [Integration test 1 with required credentials/services]
- [Integration test 2]
- A skipped real-integration test should be reported as skipped, not silently passed.

---

## 22. Testing

### 22.1 Test data format

Tests are defined in `tests.yaml` as language-agnostic evaluations organized by durability tier and validation profile.

Structure:
```yaml
# Durable — input/output behavioral check
function_name:
  - name: "human-readable test name"
    durability: durable
    profile: core            # core | extension | real_integration
    input: { ... }           # Function arguments
    output: "expected"       # Expected return value
    error: true              # Present only if function should error

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

# Durable — state machine transitions
state_transitions:
  - name: "valid transition - [description]"
    machine: "[state machine name]"
    from: "[state]"
    trigger: "[trigger]"
    to: "[expected state]"

  - name: "invalid transition - [description]"
    machine: "[state machine name]"
    from: "[state]"
    trigger: "[trigger]"
    error: true
```

### 22.2 Using tests.yaml

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
5. **For state machine tests**, verify transition outcomes match expected states
6. **Run tests** and iterate until all pass

### 22.3 Input field mapping

**[function1]:**
```yaml
input: { field1: <type>, field2: <type> }
```

**[function2]:**
```yaml
input: "<string>"  # Direct input, not an object
```

### 22.4 Error test handling

For entries with `error: true`, assert the function raises/returns an error.

---

## 23. Live Evaluation Criteria *(include for production systems)*

These are not test-time assertions but production monitoring requirements. They encode correctness properties that must hold continuously in the real world.

### 23.1 Operational Metrics

| Metric | Acceptable range | Alert threshold |
|--------|-----------------|-----------------|
| [e.g., p99 latency] | [e.g., < 200ms] | [e.g., > 500ms] |
| [e.g., error rate] | [e.g., < 0.1%] | [e.g., > 1%] |

### 23.2 Business Invariants (monitored continuously)

Properties from the System Invariants section (Section 6) that should be verified continuously in production:

- [INV-XXX]: [How to monitor this in production]
- [SAFE-XXX]: [How to monitor this in production]

### 23.3 Drift Detection

After any reimplementation or regeneration, compare:

- [Baseline metric 1]: [What normal looks like]
- [Baseline metric 2]: [What normal looks like]
- [Behavioral fingerprint]: [Key output distributions or patterns]

Flag any deviation beyond [acceptable threshold] for human review.

### 23.4 Cost Metrics *(include for AI-assisted systems)*

| Metric | Baseline | Alert threshold |
|--------|----------|-----------------|
| [e.g., inference cost per request] | [value] | [threshold] |
| [e.g., token usage per operation] | [value] | [threshold] |

---

## 24. Generated Documentation

Implementations MUST include a `usage.md` file documenting how to use the system.

### 24.1 usage.md requirements

Include:
1. **Installation** — How to add the library/deploy the service
2. **Quick start** — Minimal code example or startup command
3. **Function/API reference** — Signature, params, examples for each function
4. **Configuration** — All settings with defaults (reference Section 10)
5. **Error handling** — How errors are reported
6. **Type conversions** — What types are accepted

Keep it under 150 lines.

---

## 25. Regeneration Confidence Checklist

Before considering the specification complete, apply this litmus test: if you deleted the entire implementation, could you regenerate it with confidence using only this spec and tests.yaml?

- [ ] Problem statement and non-goals are explicit
- [ ] All domain entities have precise field definitions
- [ ] All system invariants are explicit (not buried in implementation code)
- [ ] All safety invariants are formally stated
- [ ] All behavioral properties are formally stated
- [ ] All state machines have explicit states, transitions, and triggers
- [ ] All interface contracts have precise schemas
- [ ] All functions have unambiguous behavior tables or pseudocode
- [ ] All boundary conditions have exact threshold values
- [ ] All error conditions are documented
- [ ] All configuration fields have defaults and validation rules
- [ ] Failure model covers all failure classes with recovery behaviors
- [ ] Property-based tests cover function composition behaviors
- [ ] State machine tests cover valid and invalid transitions
- [ ] Live evaluation criteria would catch drift after regeneration
- [ ] No critical behavior exists only as implicit knowledge

If any item fails, the spec has gaps that would make regeneration risky.

## 26. Implementation Checklist (Definition of Done)

### 26.1 Core Conformance (required)

- [ ] All domain model entities implemented
- [ ] All functions implemented
- [ ] All state machines implemented with correct transitions
- [ ] All tests.yaml durable evaluations pass
- [ ] All property-based tests pass with generative framework
- [ ] All invariant checks pass
- [ ] All safety invariant checks pass
- [ ] All contract conformance tests pass (if multi-component)
- [ ] All configuration fields with defaults and validation
- [ ] Errors are raised/returned idiomatically
- [ ] Code is idiomatic for target language
- [ ] Structured logging with required context fields (if service)
- [ ] usage.md generated

### 26.2 Extension Conformance (if applicable)

- [ ] Optional feature tests pass for shipped extensions
- [ ] Extension documentation included

### 26.3 Operational Readiness (if production system)

- [ ] Live evaluation monitoring configured
- [ ] Failure recovery behaviors verified
- [ ] Dynamic config reload verified
- [ ] Security requirements met
- [ ] Real integration profile tests pass with valid credentials

---

## 27. Appendices *(include for optional extensions)*

### Appendix A. [Extension Name] (Optional)

[Description of optional extension that doesn't fit in the core spec]

#### A.1 [Subsection]

[Extension details]

#### A.2 [Subsection]

[Extension details]

---

## 28. Version History

- **v0.1.0** - Initial specification
````

### Step 10: Write tests.yaml

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
# SAFETY INVARIANTS — Highest priority, prevent dangerous states
# =============================================================================
safety:
  - name: "safety - [description of what must never happen]"
    after: ["*"]
    property: "[expression, e.g., 'workspace_path starts_with workspace_root']"

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
# STATE MACHINE TRANSITIONS — Valid and invalid state changes
# =============================================================================
state_transitions:
  - name: "transition - [from] to [to] on [trigger]"
    machine: "[state machine name]"
    from: "[state]"
    trigger: "[trigger event]"
    to: "[expected resulting state]"
    profile: core

  - name: "invalid transition - [from] on [trigger]"
    machine: "[state machine name]"
    from: "[state]"
    trigger: "[invalid trigger for this state]"
    error: true
    profile: core

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
    profile: core
    input: { field: value }
    output: "expected"

  - name: "boundary - exactly at threshold X"
    durability: durable
    profile: core
    input: { field: value }
    output: "expected"

  - name: "boundary - just above threshold X"
    durability: durable
    profile: core
    input: { field: value }
    output: "expected"

  # DURABLE — Error contract
  - name: "error - [description]"
    durability: durable
    profile: core
    input: { field: value }
    error: true

  # DURABLE — Real integration (needs credentials/network)
  - name: "integration - [description]"
    durability: durable
    profile: real_integration
    input: { field: value }
    output: "expected"

  # EPHEMERAL — Normal progression through ranges
  - name: "example - [description]"
    durability: ephemeral
    profile: core
    input: { field: value }
    output: "expected"

  # EPHEMERAL — Edge cases
  - name: "edge - zero/empty/min/max"
    durability: ephemeral
    profile: core
    input: { field: value }
    output: "expected"

  # EPHEMERAL — Special cases
  - name: "special - [description]"
    durability: ephemeral
    profile: core
    input: { field: value }
    output: "expected"


next_function:
  # ... same structure
```

2. **Test case naming conventions**:
   - `safety - [what dangerous state is prevented]`
   - `invariant - [what property holds]`
   - `property - [what universal truth]`
   - `transition - [from] to [to] on [trigger]`
   - `contract - [what interface shape]`
   - `boundary - [what threshold]`
   - `error - [what error condition]`
   - `integration - [what external interaction]`
   - `example - [what range/case]`
   - `edge - [what edge case]`
   - `special - [what special behavior]`

3. **Ensure comprehensive coverage by tier and profile**:

   **Durable (MUST have)**:
   - Every safety invariant has a check
   - Every system invariant has a check
   - Every behavioral property has a generative test definition
   - Every state machine transition (valid and invalid) has a test
   - Every interface contract has schema conformance tests
   - Every threshold has boundary tests (value-1, value, value+1)
   - Every error condition has a test

   **Ephemeral (SHOULD have)**:
   - Every output category has at least one example
   - Common input variations are tested
   - Platform-specific edge cases documented

   **Profile coverage**:
   - Core conformance tests run without external dependencies
   - Extension conformance tests are skipped if feature not shipped
   - Real integration tests are skippable but never silently passed

### Step 11: Review and Iterate

1. **Apply the Regeneration Confidence Test**:

   Before presenting the draft, mentally simulate: "If I deleted every line of implementation code and only had SPEC.md and tests.yaml, could I regenerate the system and know it works?"

   If the answer is no, identify what's missing:
   - Implicit invariants not yet captured?
   - Domain entities missing fields or normalization rules?
   - State machine transitions that are ambiguous?
   - Complex behaviors only described in prose, not pseudocode?
   - Behavioral properties only described by example, not by universal rule?
   - Interface contracts missing precise schemas?
   - Configuration fields without defaults or validation?
   - Failure classes without recovery behaviors?
   - Live evaluation criteria that would catch silent drift?

2. **Present the draft files**:
   ```
   I've created:
   - SPEC.md: [path]
   - tests.yaml: [path]

   Evaluation coverage:
   - [N] safety invariants
   - [N] system invariants
   - [N] behavioral properties (for generative testing)
   - [N] state machine transitions tested
   - [N] interface contracts (if multi-component)
   - [N] durable behavioral tests
   - [N] ephemeral example tests
   - [N] live evaluation criteria (if production system)

   Validation profiles:
   - Core conformance: [N] tests
   - Extension conformance: [N] tests
   - Real integration: [N] tests

   Key decisions made:
   - [Decision 1 with rationale]
   - [Decision 2 with rationale]

   Regeneration confidence: [High/Medium/Low]
   - [What gives confidence]
   - [What gaps remain, if any]

   Please review:
   - Are the invariants correct and complete?
   - Are the domain entities fully defined?
   - Are the state machines accurate?
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

## Adaptive Spec Depth

Not every spec needs every section. Use this guide to decide what to include:

| System Type | Required Sections | Optional Sections |
|-------------|-------------------|-------------------|
| **Simple library** | 1-7, 12-15, 20, 22, 24-26 | Skip 8-11, 16-19, 21, 23, 27 |
| **Multi-component library** | 1-7, 9, 12-15, 20, 22, 24-26 | 8, 10-11, 16-19, 21, 23, 27 |
| **Stateful service** | 1-8, 10, 13, 15-22, 24-26 | 9, 11, 14, 23, 27 |
| **Production service** | All sections | None — include everything |

When in doubt, include the section. It's better to have an empty section that signals "we considered this" than to silently omit a concern.

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

7. **Explicit boundaries prevent scope creep**:
   - State what the system does NOT do (non-goals)
   - State responsibility limits at component boundaries
   - Use "Important boundary" callouts for critical scope distinctions

8. **Redundancy serves implementors**:
   - Include "quick reference" summary sections that consolidate scattered details
   - A coding agent implementing from the spec benefits from intentional redundancy
   - Cross-reference sections by number so readers can navigate

9. **Track progress**:
   - Use TodoWrite to track specification tasks
   - Mark sections complete as they're finalized

## Example Invocations

```
/create_spec A library for validating email addresses
/create_spec Based on the requirements in prd.md, create a spec for the payment processing module
/create_spec Extract a spec from the existing implementation in src/utils/date.ts
/create_spec A multi-service system for real-time chat with message persistence
/create_spec A long-running orchestration service that polls Linear and dispatches coding agents
```

## Output Files

- `SPEC.md` - Full specification with problem statement, domain model, invariants, properties, state machines, contracts, configuration, failure model, security, observability, and live evaluation criteria
- `tests.yaml` - Comprehensive evaluations across all durability tiers and validation profiles
