## Merge changes from main
1. git fetch  all the new changes
2. merge main into this branch
3. resolve if any conflicts
4. run all the test again and fixe if any fails if any change happen only

## Testing & CI Phase
1. Push the changes to the branch if any
2. Monitor CI results
3. If CI fails:
   - Fix the issues
   - Push fixes
   - Repeat until CI passes

## Manual Testing (if applicable)
1. If the feature allows manual testing, propose a testing plan
2. if you are working in the scoop Go repo
   - name the development box: `scoop-dev-fly-<branch-pattern>`
   - deploy to a new development box environment
   - setup the environment
   - run your test aginst the newly created dev box url
   - run command to destroy env after you finish the testing
3. if you are working in the IOS app scoop-ios repo 
   - Ask felipe to manually deploy to a new box called `scoop-dev-fly-<branch-pattern>` your main service, provide him the commands ready to paste:
	   1 deploy to a new development environment
	   2 setup the environment
   - Wait for Felipe's confirmation that the environment it's ready before continuing
   - Add the url of the new deployed scoop into Config/Development* URL env var
   -Remind felipe to run  command to destroy env after testing is finish 
3. Upon confirmation, execute the manual testing
4. Document in a PR comment:
   - Testing steps performed
   - Commands/scripts used
   - Output screenshots or logs for evidence collection.
   - Detailed result and capture detailed evidences showing proof that it worked.
   - if you use any script for testing make sure to show it in the comment.
5. Check and complete any "Testing checklist" items in the PR description

## Review Phase
1. Once all checks pass, add a comment with exactly: `@claude`
2. Wait for Claude's review response
3. When review is complete:
   - Summarize suggested changes
   - Ask for my confirmation before implementing them

## Update our claude code md's
1. Update CLAUDE.md and @docs/* if the changes implemented provide something new to improve this workflows

## Cross-team Dependencies
If changes require work from other teams (backend, iOS, etc.):
1. Prepare a detailed memo explaining:
   - What changes they need to make
   - Why these changes are needed
   - Any API contracts or interfaces affected
   - Timeline considerations
2. Post this memo as a comment on the PR for easy sharing

## Remind me on post-merge actions
 - if on Go scoop repo: 
    1. run deployment: devbox run fly:deploy
    2. run setup if needed in the issue: devbox run fly:setup
 - if on IOS scoop-ios repo:
    1. Approve TestFligh build in: https://appstoreconnect.apple.com/teams/84e612ee-61c0-4f6c-9218-573d0073cb27/apps/6746719105/testflight/ios

## Important Notes
- Always check for existing work before starting
- Maintain consistency with previous commits in style and approach
- Document the continuation plan clearly in PR comments
- List worktrees: `git worktree list`
- Look for <branch-pattern>: `pwd | grep worktrees/(*)`
- <Branch naming convention: `issue-<number>-<sanitized-title>`
- Worktree location: `./tree/issue-<number>-<sanitized-title>`
- Wait for explicit confirmation before proceeding with major changes
- deploy your branch in a developemnt environmet with `devbox run fly:deploy `scoop-dev-fly-<branch-pattern>`
