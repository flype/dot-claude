I want to continue work on issue #$ARGUMENT$ 

git fetch to get the last branches created
We always store the worktree in the project-directory/tree
check we have the tree created already.
create the worktree if not
CD to move claude working directory to the right worktree directory

Make sure you read the issue and comments in order to understand the full context using gh cli

Read all the commits, both code and messages, in all the work done already in the branch.

Prepare a plan on what's missing and what are you going to work next.
Show the plan Ask for any clarification that you need on the issue content before starting the implentation

Leave a comment in the PR indicating the plan of attack for the WIP.

After you finish working and all the build and test work locally, your work it's pushed
Check the result from the CI and loop and fix any issue that might arise until the build passes.

If the implemented allows suggest me a plan for doing the manual testing, if confirmed do it and left a trail in a comment. check the "Testing checklist" in the PR content if any. leave detailed evicence of the manual testing done, run outputs, scripts used or any relevant info so I can be confident it worked.

After all leave a comment invoking the claude final review in the CI to kick the PR final review for merge. Just create a comment with this content: "@claude"

wait for the review comment from claude when finish, propose me the changes you would make from their suggestion and ask me for confirmation before doing them

if the change will require another team to do changes on their side, backend or IOS, prepare a memo explaining what they need to do, so I can send them easily, leave it as a comment
