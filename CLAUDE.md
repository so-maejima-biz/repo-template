# CLAUDE.md - AI Development Guidelines

## Core Principles
- **No secrets in code**: API keys, passwords, tokens must use environment variables
- **No destructive git operations**: Avoid `--force`, `reset --hard` without explicit approval
- **Minimal changes**: Only modify what's necessary. Avoid over-engineering
- **Confirm before commit**: Always review staged changes before committing

## Python Projects (.venv)
Before starting Python work, verify you're in the virtual environment:
```powershell
where python              # Should show .venv path
python -c "import sys; print(sys.prefix)"  # Should show .venv directory
```
If not activated, run: `.venv\Scripts\activate` (Windows)

> For detailed Python setup and troubleshooting, see `.claude/skills/python-dev/`

## Quick Commands
```bash
# Git
git status                 # Check before any commit
git diff --cached          # Review staged changes

# Python (after activating .venv)
pip install -r requirements.txt
python -m pytest           # Run tests
```

## Before PR Checklist
- [ ] No hardcoded secrets or credentials
- [ ] `.gitignore` covers sensitive files
- [ ] Tests pass (if applicable)
- [ ] Code follows project conventions

## Project Skills
Detailed procedures are in `.claude/skills/`:
- `git-hygiene/` - Git operations, .gitignore, secret prevention
- `python-dev/` - Virtual environment, testing, formatting
- `security-basics/` - Credential handling, minimal permissions

## Personal Notes
Use `CLAUDE.local.md` for machine-specific settings (git-ignored).
