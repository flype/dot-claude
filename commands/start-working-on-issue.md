# GitHub Issue Workflow for Issue #$ARGUMENT$

## Setup Phase
1. Fetch latest branches: `git fetch origin`
2. Get issue details to create branch name:
   - Fetch issue title: `gh issue view $ARGUMENT$ --json title -q .title`
   - Generate branch name format: `issue-$ARGUMENT$-<sanitized-title>`
   - Sanitize title: lowercase, replace spaces with hyphens, remove special characters
3. Create worktree with new branch:
   - `git worktree add ./tree/issue-$ARGUMENT$-<sanitized-title> -b issue-$ARGUMENT$-<sanitized-title>`
4. Change working directory to the new worktree: `cd ./tree/issue-$ARGUMENT$-<sanitized-title>`

## Project Management
1. Move issue #$ARGUMENT$ to "In progress" status in project "cooking Scoop!"
   - First, verify the project ID matches: PVT_kwDOCuNJK84A6lGK
   - Use command: `gh project item-edit --project-id PVT_kwDOCuNJK84A6lGK --id <ITEM_ID> --field-id PVTSSF_lADOCuNJK84A6lGKzgvIa9g --single-select-option-id 47fc9ee4`

## Analysis Phase
1. Read the full issue content and ALL comments using: `gh issue view $ARGUMENT$ --comments`
2. Analyze the requirements and context thoroughly
3. If any clarifications are needed:
   - List all questions clearly
   - Ask me for answers
   - Post both questions and answers as a comment on the github issue $ARGUMENT$

## Implementation Phase
1. Execute the plan step by step, remember to build test before the implementation and run the test suite constanly to get quick feedback.
2. Ensure consistency with existing code in the branch
3. Run local builds and tests suite before git commit & push
4. create the PR
5. Remind me to start the '/post-implementation-workflow when you feel we are ready to advance

## Important Notes
- Always use `gh` CLI for GitHub operations
- Keep detailed records of all actions as PR/issue comments
- Branch naming convention: `issue-<number>-<sanitized-title>`
- Worktree location: `./tree/issue-<number>-<sanitized-title>`
- Wait for explicit confirmation before proceeding with major changes
- deploy your branch in a developemnt environmet with `devbox run fly:deploy `scoop-dev-fly-<branch-pattern>`
