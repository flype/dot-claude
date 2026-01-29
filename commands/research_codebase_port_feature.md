# Research Codebase for Porting

You are tasked with conducting comprehensive research across the codebase to document how a specific functionality is implemented, so it can be ported to another codebase. The output should be self-contained with actual code examples and thorough explanations.

## Initial Setup:

When this command is invoked, respond with:
```
I'm ready to research the codebase for porting documentation. Please describe the functionality you want to port, and I'll create comprehensive documentation with code examples and implementation details that can be used to recreate this functionality elsewhere.
```

Then wait for the user's query about what functionality to document.

## Steps to follow after receiving the query:

1. **Read any directly mentioned files first:**
   - If the user mentions specific files, read them FULLY first
   - **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - **CRITICAL**: Read these files yourself in the main context before spawning any sub-tasks
   - This ensures you have full context before decomposing the research

2. **Analyze and decompose the functionality:**
   - Break down the functionality into its core components
   - Identify the key abstractions, interfaces, and data structures
   - Map out dependencies between components
   - Create a research plan using TodoWrite to track all subtasks
   - Think about what someone would need to know to reimplement this from scratch

3. **Spawn parallel sub-agent tasks for comprehensive research:**
   - Create multiple Task agents to research different aspects concurrently
   - Use specialized agents:

   **For codebase research:**
   - Use the **codebase-locator** agent to find WHERE files and components live
   - Use the **codebase-analyzer** agent to understand HOW specific code works
   - Use the **codebase-pattern-finder** agent to find usage examples and patterns

   **For web research (if needed for external context):**
   - Use the **web-search-researcher** agent for external documentation about libraries/frameworks used

   **CRITICAL INSTRUCTION FOR ALL SUB-AGENTS:**
   Tell each sub-agent to return:
   - ACTUAL CODE SNIPPETS (not just file references)
   - Complete function/class implementations when relevant
   - Configuration examples
   - Type definitions and interfaces
   - Example usage patterns with real code

4. **Wait for all sub-agents to complete and synthesize findings:**
   - IMPORTANT: Wait for ALL sub-agent tasks to complete before proceeding
   - Compile all sub-agent results
   - Organize findings by logical component/layer
   - Ensure you have actual code for all key pieces

5. **Generate the porting documentation:**
   - Structure the document to be completely self-contained
   - Someone reading this should be able to reimplement the functionality WITHOUT access to the original codebase
   - Use the following structure:

   ```markdown
   # Porting Guide: [Functionality Name]

   **Date**: [Current date]
   **Source Repository**: [Repository name]
   **Purpose**: Self-contained documentation for porting [functionality] to another codebase

   ## Overview

   [High-level description of what this functionality does, why it exists, and its role in the system]

   ## Architecture

   [Describe the overall architecture with a clear explanation of how components interact]

   ### Component Diagram (Text-based)
   ```
   [ASCII or text-based diagram showing component relationships]
   ```

   ## Core Concepts

   ### [Concept 1]
   [Explain the concept thoroughly - what it is, why it's needed, how it's used]

   ### [Concept 2]
   ...

   ## Data Structures

   ### [Structure Name]
   ```[language]
   // Complete type/interface/class definition
   [actual code]
   ```
   **Purpose**: [What this structure represents and how it's used]

   ## Implementation Details

   ### [Component/Module 1]

   #### Purpose
   [What this component does and why]

   #### Dependencies
   - [List external libraries/frameworks needed]
   - [List internal dependencies on other components]

   #### Core Implementation
   ```[language]
   // Complete implementation code
   [actual code - full functions, not snippets]
   ```

   #### Key Implementation Notes
   - [Important decisions made and why]
   - [Edge cases handled]
   - [Performance considerations]

   ### [Component/Module 2]
   ...

   ## Configuration

   ### Required Configuration
   ```[language/format]
   [actual configuration examples]
   ```

   ### Environment Variables
   | Variable | Purpose | Example Value |
   |----------|---------|---------------|
   | VAR_NAME | Description | example_value |

   ## Usage Examples

   ### Basic Usage
   ```[language]
   // Complete working example
   [actual code]
   ```

   ### Advanced Usage
   ```[language]
   // More complex example showing full capabilities
   [actual code]
   ```

   ## Integration Points

   ### How to Integrate
   [Step-by-step guide for integrating this into a new codebase]

   ### API Surface
   [Document the public API - what functions/methods/endpoints are exposed]

   ```[language]
   // Public interface definition
   [actual code]
   ```

   ## Dependencies

   ### External Libraries
   | Library | Version | Purpose |
   |---------|---------|---------|
   | lib-name | ^x.y.z | What it's used for |

   ### Why These Dependencies
   [Explain why each major dependency was chosen and if alternatives exist]

   ## Error Handling

   ### Error Types
   ```[language]
   // Error definitions
   [actual code]
   ```

   ### Error Handling Patterns
   [Describe how errors are handled throughout the system]

   ## Testing Approach

   ### Unit Test Examples
   ```[language]
   // Example test code
   [actual code]
   ```

   ### Integration Test Patterns
   [Describe how to test the integrated functionality]

   ## Gotchas and Pitfalls

   - [Common mistakes to avoid]
   - [Non-obvious behaviors]
   - [Platform-specific considerations]

   ## Adaptation Notes

   [Guidance on how to adapt this implementation for different contexts]
   - What can be simplified for smaller scale
   - What needs to change for different frameworks
   - What's optional vs essential
   ```

6. **Review for completeness:**
   - Verify all code examples are complete and runnable
   - Ensure no references to specific file paths or line numbers
   - Check that someone unfamiliar with the original codebase could implement this
   - Confirm all dependencies and configurations are documented

7. **Present findings:**
   - Save the document to an appropriate location
   - Present a summary to the user
   - Ask if any areas need more detail or clarification

## Important notes:

- **NO FILE REFERENCES**: Do not include file paths, line numbers, or GitHub links - these won't exist in the target codebase
- **COMPLETE CODE**: Always include complete, working code - not truncated snippets with "..."
- **SELF-CONTAINED**: The document must stand alone - assume the reader has no access to the original codebase
- **EXPLAIN THE WHY**: Don't just show code - explain design decisions, tradeoffs, and reasoning
- **INCLUDE EXAMPLES**: Show how to use every major component with real code examples
- **DOCUMENT DEPENDENCIES**: List all external dependencies with versions and purposes
- **CAPTURE EDGE CASES**: Document known edge cases, error handling, and gotchas
- Always use parallel Task agents to maximize efficiency
- Each sub-agent should return actual code, not just descriptions
- Focus on making the documentation actionable for implementation
- Include both the "happy path" and error handling
- Document configuration and environment requirements thoroughly
