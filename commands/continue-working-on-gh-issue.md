# Continue Work on GitHub Issue #$ARGUMENT$

## Setup Phase
1. Fetch latest branches: `git fetch origin`

## Context Analysis Phase
1. Read the full issue content and ALL comments: `gh issue view $ARGUMENT$ --comments`
2. Find associated PR: `gh pr list --search "issue:$ARGUMENT$" --json number,url`
3. Review existing work:
   - Read all commits: `git log --oneline --decorate`
   - Review commit messages and changes: `git log -p`
   - Check PR description and comments: `gh pr view --comments`
4. Analyze current state:
   - What has been implemented
   - What tests have been added
   - Any feedback from previous reviews

## Planning Phase
1. Create a detailed plan covering:
   - Summary of work already completed
   - List of remaining tasks
   - Any refactoring needed based on previous commits
   - Testing requirements
2. Present the plan to me for review
3. Ask for any clarifications needed
4. Post the approved plan as a comment on the PR with header: "## Continuing Work - Plan"

## Implementation Phase
1. Execute the plan step by step, remember to build test before the implementation and run the build constanly to get quick feedback.
2. Ensure consistency with existing code in the branch
3. Run local builds and tests before git commit & push
4. create the PR
5. Remind me to start the '/post-implementation-workflow when you feel we are ready to advance

## Important Notes
- Always check for existing work before starting
- Maintain consistency with previous commits in style and approach
- Document the continuation plan clearly in PR comments
- Branch naming convention: `issue-<number>-<sanitized-title>`
- Worktree location: `./tree/issue-<number>-<sanitized-title>`
- Wait for explicit confirmation before proceeding with major changes
