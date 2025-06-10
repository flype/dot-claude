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
1. Implement the solution based on the issue requirements
2. Ensure all changes follow the project's coding standards
3. Run local builds and tests before pushing

## Testing & CI Phase
1. Push the changes to the branch
2. Monitor CI results
3. If CI fails:
   - Fix the issues
   - Push fixes
   - Repeat until CI passes

## Manual Testing (if applicable)
1. If the feature allows manual testing, propose a testing plan
2. if you are working in the scoop go service
   - name the dev box: `dev-box-fly-issue-$ARGUMENT$`
   - deploy to a new dev box environment
   - setup the environment
   - run your test aginst the newly created dev box url
   - run command to destroy env after you finish the testing
3. Upon confirmation, execute the manual testing
4. Document in a PR comment:
   - Testing steps performed
   - Commands/scripts used
   - Output screenshots or logs
   - Results and observations
5. Check and complete any "Testing checklist" items in the PR description

## Review Phase
1. Once all checks pass, add a comment with exactly: `@claude`
2. Wait for Claude's review response
3. When review is complete:
   - Summarize suggested changes
   - Ask for my confirmation before implementing them

## Cross-team Dependencies
If changes require work from other teams (backend, iOS, etc.):
1. Prepare a detailed memo explaining:
   - What changes they need to make
   - Why these changes are needed
   - Any API contracts or interfaces affected
   - Timeline considerations
2. Post this memo as a comment on the PR for easy sharing

## Important Notes
- Always use `gh` CLI for GitHub operations
- Keep detailed records of all actions as PR/issue comments
- Branch naming convention: `issue-<number>-<sanitized-title>`
- Worktree location: `./tree/issue-<number>-<sanitized-title>`
- Wait for explicit confirmation before proceeding with major changes
- for deploying a new dev box in fly.io, use the commands availables in devbox run fly:*
