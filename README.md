# arc-conventional-commits

A Claude Code skill that injects [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) with [semantic-release](https://github.com/semantic-release/semantic-release) into any repository for automated semantic versioning.

## What it does

- **Detects** existing versioning tools (standard-version, changesets, lerna, release-please) and offers to replace them
- **Installs** semantic-release with the full plugin chain (commit-analyzer, release-notes-generator, changelog, npm, github, git)
- **Configures** the Conventional Commits preset so `fix:` = PATCH, `feat:` = MINOR, `BREAKING CHANGE` = MAJOR
- **Optionally** sets up commitlint + husky for commit message enforcement and GitHub Actions for CI
- **Protects main** with three layers: git pre-commit hook, Claude Code PreToolUse hook, and GitHub branch protection rules

## Install

```bash
npx skills add andysolomon/arc-conventional-commits-skill
```

To install globally (available across all projects):

```bash
npx skills add andysolomon/arc-conventional-commits-skill -g
```

To target a specific agent:

```bash
npx skills add andysolomon/arc-conventional-commits-skill -a claude-code
```

## Usage

Once installed, the skill triggers when you ask Claude Code things like:

- "Set up conventional commits in this repo"
- "Add semantic versioning"
- "Configure release automation"
- "Replace our current versioning setup with semantic-release"
- "What commit message should I use for a breaking change?"

## Commit Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

| Prefix | Version Bump | Example |
|--------|-------------|---------|
| `fix:` | PATCH (0.0.x) | `fix: prevent race condition in auth flow` |
| `feat:` | MINOR (0.x.0) | `feat(api): add pagination to /users endpoint` |
| `feat!:` | MAJOR (x.0.0) | `feat!: drop support for Node 14` |

Valid types: `feat`, `fix`, `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`

## Branch Protection

The skill sets up three layers to prevent direct commits to main:

| Layer | What it does | Where it runs |
|-------|-------------|---------------|
| **Husky pre-commit hook** | Blocks `git commit` on main | Local terminal |
| **Claude Code PreToolUse hook** | Prompts for approval when Claude tries to commit on main | Claude Code sessions |
| **GitHub branch protection** | Requires PRs with reviews to merge into main | GitHub server-side |

This ensures all changes go through feature branches and pull requests.
