# GitHub Issue Workflow for Issue #$ARGUMENT$

## Setup Phase
1. Fetch latest branches: `git fetch origin`
2. Get issue details 
   - Fetch issue title: `gh issue view $ARGUMENT$ --json title -q .title`

## Project Management
1. Move issue #$ARGUMENT$ to "In progress" status in project "cooking Scoop!"
   - First, verify the project ID matches: PVT_kwDOCuNJK84A6lGK
   - Use command: `gh project item-edit --project-id PVT_kwDOCuNJK84A6lGK --id <ITEM_ID> --field-id PVTSSF_lADOCuNJK84A6lGKzgvIa9g --single-select-option-id 47fc9ee4`

## Analysis Phase
1. Read the full issue content and ALL comments using: `gh issue view $ARGUMENT$ --comments`
2. Analyze the requirements and context thoroughly
3. Evaluate all the potential solutions, and leave a comment using this template:
	<potential-solutions>
	### Potential Solutions
	#### Option 1: [Name]
	- Description: How it works
	- Pros: Benefits
	- Cons: Drawbacks  
	- Files to modify: [list specific files]

	#### Option 2: [Name]
	[Same format as Option 1]

	#### Recommended Approach
	Which option and why

	### Technical Notes
	- Affected files: [list from code analysis]
	- Dependencies: [any external/team dependencies]
	</potential-solutions>
  (Ask me for my input and confirmation on the recomened approach)
4. Read the full issue content and ALL comments using: `gh issue view $ARGUMENT$ --comments`
5. If any clarifications are needed:
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
- Wait for explicit confirmation before proceeding with major changes
- deploy your branch in a developemnt environmet with `devbox run fly:deploy `scoop-dev-fly-$ARGUMENT$``
