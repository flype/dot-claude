# Claude Code Configuration

This repository contains Felipe's customized Claude Code setup, including custom commands, workflows, and project management tools.

## Setup

### Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- Git configured on your system
- GitHub CLI (`gh`) for issue management

### Installation

1. Clone this repository to your Claude Code configuration directory:
   ```bash
   git clone <repository-url> ~/.claude
   cd ~/.claude
   ```

2. Configure your model preference in `settings.json` if needed:
   ```json
   {
     "model": "sonnet"
   }
   ```
3. Update [YOUR_NAME] in the project to your name

4. Create agents
You have as example `frontend-developer.md`
Create agents for your specific tech stack, at the end of them add this code and update the naming of the documentation files "[doc_file_name]"

````
## Goal
Your goal is to propose a detailed implementation plan for our current codebase & project, including specifically which files to create/change, what changes/content are, and all the important notes (assume others only have outdated knowledge about how to do the implementation)
NEVER do the actual implementation, just propose implementation plan
Save the implementation plan in `.claude/doc/{feature_name}/[doc_file_name].md`


## Output format
Your final message HAS TO include the implementation plan file path you created so they know where to look up, no need to repeat the same content again in final message (though is okay to emphasis important notes that you think they should know in case they have outdated knowledge)

e.g. I've created a plan at `.claude/doc/{feature_name}/[doc_file_name].md`, please read that first before you proceed


## Rules
- NEVER do the actual implementation, or run build or dev, your goal is to just research and parent agent will handle the actual building & dev server running
- Before you do any work, MUST view files in `.claude/sessions/context_session_{feature_name}.md` file to get the full context
- After you finish the work, MUST create the `.claude/doc/{feature_name}/[doc_file_name].md` file to make sure others can get full context of your proposed implementation
```
## Update `claude.md`
Update the Subagents Workflow section with the agents created (Search [YOUR AGENTS])


## Structure

```
.
├── commands/                    # Custom Claude Code commands
│   ├── explore-plan.md
│   ├── create-new-gh-issue.md
│   ├── start-working-on-issue.md
│   └── ...
├── projects/                    # Project-specific configurations
├── todos/                       # Todo tracking files
├── shell-snapshots/             # Shell session snapshots
├── statsig/                     # Statistics and metrics
├── settings.json                # Main configuration
├── CLAUDE.md                    # Personal instructions for Claude
└── .gitignore                   # Git ignore rules
```

## Commands

These commands follow a main development workflow sequence. Use them in order for the complete development cycle:

### Main Workflow (Sequential)


#### 1. `/explore-plan <feature-description>`
**Purpose**: Define and create context session file with all the context neede for the feature
- Analyzes the feature request against existing codebase
- Asks clarifying questions about requirements
- Gather al the neded context for the feature usign agents to leverage space in context
- Creates comprehensive plan with problem statement, user value, and definition of done
- Includes detailed manual testing checklist


#### 2. `/create-new-gh-issue <context_file>`
**Purpose**: Create well-structured GitHub issues from context session plan
- Analyze the context file 
- Creates comprehensive issue with problem statement, user value, and definition of done
- Includes detailed manual testing checklist

#### 3. `/start-working-on-issue-new <issue-number>`
**Purpose**: Start working on a GitHub issue with comprehensive analysis and planning
- Fetches and analyzes the complete issue including all comments
- Evaluates multiple potential solutions with pros/cons
- Moves issue to "In progress" status in project management
- Creates structured implementation plan
- Start in a worktree the implementation of the feature

[In loop 4-5]
#### 4. `/update-feedback <issue-number>`
**Purpose**: Resume work on an existing GitHub issue
- Analyzes all previous work and commits
- Uses `qa-criteria-validator` agent to open a browser and complete the manual tests
- Update the Issue and the PR with feedback

#### 5. `/implement-feedback` (if build fails)
**Purpose**: Continuously monitor and fix CI build failures
- Fetches and analyzes the complete issue including all comments
- Implement the feedback from `/update-feedback.md` command


## Configuration

### Personal Instructions

Edit `CLAUDE.md` to customize how Claude Code behaves for your workflow:

- Code style preferences
- Testing requirements
- Git workflow rules
- Communication preferences

### Model Settings

Configure your preferred Claude model in `settings.json`:

```json
{
  "model": "sonnet"
}
```

## Usage

Once installed, Claude Code will automatically use these configurations. Custom commands can be invoked using the slash command syntax in Claude Code:

```
/start-working-on-issue-new 123
/explore-plan-code-test
/pixel-perfect-design header
```

## Workflow Integration

### Main Development Flow
Follow this sequence for structured development:

1. `/explore-plan` → Create and plan the feature and gather context
1. `/create-new-gh-issue` → Create gh issue
2. `/start-working-on-issue-new` → Begin implementation
3. `/update-feedback` → Test and provide feedback
4. `/implement-feedbac` → Implement the feedback

## Customization

To add your own commands:

1. Create a new markdown file in `commands/`
2. Follow the existing command template patterns
3. Commands are automatically available in Claude Code

To modify workflows:

1. Edit the relevant command files in `commands/`
2. Update `CLAUDE.md` for behavioral changes
3. Modify `settings.json` for system-level configuration


## Contributing

This is a personal configuration repository. If you find useful patterns, feel free to adapt them for your own Claude Code setup.
