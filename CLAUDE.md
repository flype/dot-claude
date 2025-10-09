# Working with [YOUR_NAME]

## Code Writing

- YOU MUST ALWAYS address me as "[YOUR_NAME]" in all communications.
- We STRONGLY prefer simple, clean, maintainable solutions over clever or complex ones. Readability and maintainability are PRIMARY CONCERNS, even at the cost of conciseness or performance.
- YOU MUST make the SMALLEST reasonable changes to achieve the desired outcome.
- YOU MUST MATCH the style and formatting of surrounding code, even if it differs from standard style guides. Consistency within a file trumps external standards.
- YOU MUST NEVER make code changes unrelated to your current task. If you notice something that should be fixed but is unrelated, document it rather than fixing it immediately.
- YOU MUST NEVER remove code comments unless you can PROVE they are actively false. Comments are important documentation and must be preserved.
- All code files MUST start with a brief 2-line comment explaining what the file does. Each line MUST start with "ABOUTME: " to make them easily greppable.
- YOU MUST NEVER refer to temporal context in comments (like "recently refactored"). Comments should be evergreen and describe the code as it is.
- YOU MUST NEVER implement mock modes for testing or any purpose. We always use real data and real APIs.
- YOU MUST NEVER throw away implementations to rewrite them without EXPLICIT permission. If you're considering this, YOU MUST STOP and ask first.
- YOU MUST NEVER use temporal naming conventions like 'improved', 'new', or 'enhanced'. All naming should be evergreen.
- YOU MUST NOT change whitespace unrelated to code you're modifying.

## Version Control

- For non-trivial edits, all changes MUST be tracked in git.
- If the project isn't in a git repo, YOU MUST STOP and ask permission to initialize one.
- If there are uncommitted changes or untracked files when starting work, YOU MUST STOP and ask how to handle them. Suggest committing existing work first.
- When starting work without a clear branch for the current task, YOU MUST create a WIP branch.
- YOU MUST commit frequently throughout the development process.

## Getting Help

- YOU MUST ALWAYS ask for clarification rather than making assumptions.
- If you're having trouble, YOU MUST STOP and ask for help, especially for tasks where human input would be valuable.

## Testing

- Tests MUST comprehensively cover ALL implemented functionality. 
- YOU MUST NEVER ignore system or test output - logs and messages often contain CRITICAL information.
- Test output MUST BE PRISTINE TO PASS.
- If logs are expected to contain errors, these MUST be captured and tested.
- NO EXCEPTIONS POLICY: ALL projects MUST have unit tests, integration tests, AND end-to-end tests. The only way to skip any test type is if [YOUR_NAME] EXPLICITLY states: "I AUTHORIZE YOU TO SKIP WRITING TESTS THIS TIME."

## Compliance Check

Before submitting any work, verify that you have followed ALL guidelines above. If you find yourself considering an exception to ANY rule, YOU MUST STOP and get explicit permission from [YOUR_NAME] first.



## Rules
- After a plan mode phase you should create a `.claude/sessions/context_session_{feature_name}.md` with the definition of the plan
- Before you do any work, MUST view files in `.claude/sessions/context_session_{feature_name}.md` file and `.claude/doc/{feature_name}/*` files to get the full context (feature_name being the id of the session we are operate, if file doesnt exist, then create one)
- `.claude/sessions/context_session_{feature_name}.md` should contain most of context of what we did, overall plan, and sub agents will continusly add context to the file
- After you finish the work, MUST update the `.claude/sessions/context_session_{feature_name}.md` file to make sure others can get full context of what you did
- After you finish the each phase, MUST update the `.claude/sessions/context_session_{feature_name}.md` file to make sure others can get full context of what you did

## Sub-Agent Workflow
This project uses specialized sub-agents for different concerns. Always consult the appropriate agent:

- **qa-criteria-validator**: Final UI/UX validation and feedback
- **frontend-developer**: Client-side business logic
- [YOUR AGENTS]

Sub agents will do research about the implementation and report feedback, but you will do the actual implementation;
When passing task to sub agent, make sure you pass the context file, e.g. `.claude/sessions/context_session_{feature_name}.md`.
After each sub agent finish the work, make sure you read the related documentation they created to get full context of the plan before you start executing
