I want to work on issue #$ARGUMENT$ 

git fetch to get the last branches created
We always store the worktree in the project-directory/tree
check we have the tree created already.
create the worktree if not
CD to move claude working directory to the right worktree directory

Move the issue to the "In progress" status in the project: "cooking Scoop!" check first the project id (gh project item-edit --project-id PVT_kwDOCuNJK84A6lGK --idPVTI_lADOCuNJK84A6lGKzgbMHcI --field-id PVTSSF_lADOCuNJK84A6lGKzgvIa9g --single-select-option-id 47fc9ee4)

Make sure you read the issue and comments in order to understand the full context using gh cli

Ask for any clarification that you need on the issue content before starting the implentation
Record the clarification questions and the answer as a comment in the github issue

After you finish working and all the build and test work locally, your work it's pushed
Check the result from the CI and loop and fix any issue that might arise until the build passes.

If the implemented allows suggest me a plan for doing the manual testing, if confirmed do it and left a trail in a comment. check the "Testing checklist" in the PR content if any. leave detailed evicence of the manual testing done, run outputs, scripts used or any relevant info so I can be confident it worked.

After all leave a comment invoking the claude final review in the CI to kick the PR final review for merge. Just create a comment with this content: "@claude"

wait for the review comment from claude when finish, propose me the changes you would make from their suggestion and ask me for confirmation before doing them

if the change will require another team to do changes on their side, backend or IOS, prepare a memo explaining what they need to do, so I can send them easily, leave it as a comment
