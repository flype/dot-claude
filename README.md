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

## Structure

```
.
├── commands/                    # Custom Claude Code commands
│   ├── research_codebase_codex.md
│   ├── explore-plan-code-test.md
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

#### 1. `/create-new-gh-issue <feature-description>`
**Purpose**: Create well-structured GitHub issues from feature ideas
- Analyzes the feature request against existing codebase
- Asks clarifying questions about requirements
- Creates comprehensive issue with problem statement, user value, and definition of done
- Includes detailed manual testing checklist

#### 2. `/start-working-on-issue-new <issue-number>`
**Purpose**: Start working on a GitHub issue with comprehensive analysis and planning
- Fetches and analyzes the complete issue including all comments
- Evaluates multiple potential solutions with pros/cons
- Moves issue to "In progress" status in project management
- Creates structured implementation plan
- Sets up proper git workflow and branch management

#### 3. `/continue-working-on-gh-issue <issue-number>` (if needed)
**Purpose**: Resume work on an existing GitHub issue
- Analyzes all previous work and commits
- Reviews existing PR and feedback
- Creates continuation plan based on current state
- Maintains consistency with previous implementation approach

#### 4. `/cycle-ci-until-green` (if build fails)
**Purpose**: Continuously monitor and fix CI build failures
- Monitors GitHub workflow status after pushes
- Automatically fixes build failures
- Keeps pushing until all checks pass
- Uses GitHub CLI to check build status

#### 5. `/post-implementation-workflow`
**Purpose**: Complete post-development tasks and testing
- Merges latest changes from main branch
- Monitors CI and fixes failures
- Executes manual testing with development environment setup
- Handles code review process
- Updates documentation and cross-team dependencies
- Manages deployment and post-merge actions

### Utility Commands (Ad Hoc)

#### `/explore-plan-code-test` (Vibe Coding)
**Purpose**: Mini full-cycle prompt for quick development without formal issue tracking
- **Explore**: Uses parallel agents to find relevant files and examples
- **Plan**: Creates detailed implementation plan with research backing
- **Code**: Implements following existing codebase patterns
- **Test**: Runs comprehensive testing including browser testing if needed
- Generates PR description with implementation choices and justification

#### `/start-working-on-issue <issue-number>` (Legacy)
**Purpose**: Legacy version for starting work on GitHub issues
- Simpler workflow compared to the "new" version
- Focuses on immediate implementation without extensive solution analysis
- Good for straightforward issues that don't need deep planning

#### `/next-on-plan-from-prd <section>`
**Purpose**: Planning mode for brainstorming next features to implement
- Reviews API specifications and product requirements
- Builds inventory of implemented vs missing features
- Helps prioritize development work
- Creates problem statements for new issues

#### `/pixel-perfect-design <section>`
**Purpose**: Implement Figma designs with pixel-perfect accuracy
- Compares current app screenshots with Figma designs
- Creates detailed improvement plan for visual discrepancies
- Iteratively improves design implementation
- Uses Playwright for automated screenshot comparison

#### `/research_codebase_codex`
**Purpose**: Comprehensive codebase research and documentation
- Conducts evidence-based research across the entire codebase
- Creates detailed research documents with file:line references
- Uses multiple specialized research agents for different aspects
- Generates GitHub permalinks for easy navigation
- Maintains research documentation in structured format

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

1. `/create-new-gh-issue` → Create and plan the feature
2. `/start-working-on-issue-new` → Begin implementation
3. `/continue-working-on-gh-issue` → Resume if needed
4. `/cycle-ci-until-green` → Fix build issues if they occur
5. `/post-implementation-workflow` → Complete and deploy

### Quick Development (Vibe Coding)
For rapid prototyping or small changes:
- `/explore-plan-code-test` → Complete mini-cycle without formal issue tracking

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
