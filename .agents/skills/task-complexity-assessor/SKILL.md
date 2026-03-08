---
name: task-complexity-assessor
description: Assess task complexity to determine when to use a todo list. Analyzes user requests for new features or tasks, estimates complexity in steps, and suggests relevant skills from the skills directory that could help accomplish the task.
---

# Task Complexity Assessor Skill

**Answers the question: Is this task complex enough to warrant a todo list? Which skills might help?**

This skill evaluates task complexity and recommends the appropriate planning approach.

## When to Use

Use this skill when a user requests:

- A new feature implementation
- A bug fix requiring multiple steps
- A refactoring task
- Any multi-step task or project
- Help with a complex problem

**Key indicator**: User asks for something that will require more than one action to complete.

## How It Works

### Step 1: Analyze the Request

Evaluate the user's request by considering:

- **Number of distinct components** - How many files or systems are involved?
- **Prerequisites** - Are there dependencies or setup steps required?
- **Testing** - Will tests need to be written?
- **Documentation** - Are docs or README updates needed?
- **Configuration** - Are there config changes required?
- **Integration** - Does this connect with other systems?

### Step 2: Estimate Complexity

Categorize the task:

| Complexity | Indicators | Steps Estimate |
|------------|------------|----------------|
| Simple | Single file, one change, no tests | 1-2 steps |
| Moderate | Few files, some testing, basic config | 3-5 steps |
| Complex | Multiple files, full test suite, docs, config | 6-10 steps |
| Very Complex | Many files, system-wide changes, migrations | 10+ steps |

### Step 3: Determine Todo Requirement

- **≤3 steps**: Proceed directly without todo list
- **>3 steps**: Create a todo list to track progress

### Step 4: Scan for Relevant Skills

Examine the `.agents/skills/` directory for skills that could help:

1. **List all available skills** from the skills directory
2. **Match skills to the task** based on:
   - Skill purpose vs task type
   - Required tooling
   - Workflow patterns
3. **Recommend top 2-3 most relevant skills**

## Skills Directory Structure

The skills are located in `.agents/skills/` with this structure:

```
.agents/skills/
├── skill-name/
│   ├── SKILL.md          # Main skill definition
│   └── references/       # Optional reference docs
└── ...
```

## Available Skills Reference

| Skill | Purpose |
|-------|---------|
| feature-breakdown | Decompose feature specs into tasks |
| feature-planning | Sequence tasks and identify dependencies |
| feature-summary | Document feature capabilities |
| ai-agent-implementation | Execute tasks using AI agents |
| skill-creator | Create new skills |
| skill-validator | Validate skill definitions |
| git-commit-workflow | Commit changes following conventions |
| git-worktrees-usage | Work with isolated git worktrees |
| github-create-issue | Create GitHub issues |
| github-pull-request | Create and manage PRs |
| github-workflows | Create GitHub Actions workflows |
| mcp-builder | Build MCP servers |
| devcontainer-config | Configure dev containers |
| readme-updater | Update README files |
| semver-changelog | Manage changelogs |
| brainstorming-partner | Brainstorm ideas |

## Output Format

After assessment, provide:

```markdown
## Task Complexity Assessment

**Task**: [User's request summary]

**Complexity Rating**: [Simple|Moderate|Complex|Very Complex]
**Estimated Steps**: [N] steps

### Todo Recommendation
[Use a todo list / Proceed without todo list]

### Relevant Skills
Based on the task, consider these skills:
1. **[Skill Name]**: [Why it's relevant]
2. **[Skill Name]**: [Why it's relevant]
3. **[Skill Name]**: [Why it's relevant]

### Recommended Approach
[High-level strategy for tackling this task]
```

## Workflow Integration

When you assess a task:

1. **Load this skill** to analyze complexity
2. **Follow the assessment steps** above
3. **If todo recommended**:
   - Use `todowrite` to create the todo list
   - Break down the task into individual steps
   - Mark each step with priority (high/medium/low)
4. **If skills recommended**:
   - Suggest loading relevant skills to the user
   - Explain how each skill helps

## Examples

### Example 1: Simple Task (No Todo)

**User Request**: "Fix the typo in the README"

- Analysis: Single file, one change
- Complexity: Simple
- Steps: 1
- **Recommendation**: Proceed without todo list

### Example 2: Complex Task (Use Todo)

**User Request**: "Add user authentication to the app"

- Analysis: Multiple files, database, config, tests, docs
- Complexity: Complex
- Steps: 8+
- **Recommendation**: Create todo list
- **Relevant Skills**:
  - `feature-breakdown` - Decompose authentication into tasks
  - `github-workflows` - Add auth-related CI workflows
  - `skill-creator` - If creating auth-related skills

### Example 3: Moderate Task (Use Todo)

**User Request**: "Create a new GitHub workflow for deployments"

- Analysis: Single system, but requires workflow file, tests, docs
- Complexity: Moderate
- Steps: 4
- **Recommendation**: Create todo list
- **Relevant Skills**:
  - `github-workflows` - Create/modify workflows
  - `skill-validator` - If creating a deployment skill

## Key Principles

1. **Be conservative** - When in doubt, recommend a todo list
2. **Think ahead** - Consider not just immediate steps but testing, docs, config
3. **Match skills wisely** - Only recommend skills that genuinely apply
4. **Explain reasoning** - Help the user understand the complexity assessment
5. **Be actionable** - Provide concrete next steps

## Common Pitfalls to Avoid

❌ **Underestimating complexity** - "It looks simple" often leads to scope creep

❌ **Missing relevant skills** - Not scanning available skills thoroughly

❌ **Over-recommending todo** - Simple tasks don't need overhead

❌ **Vague recommendations** - "Consider using skills" is not helpful; be specific
