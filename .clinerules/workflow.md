## Brief overview
Workflow rules for managing tasks, versioning, and git operations in the Global Portfolio Engine project. These rules ensure consistent task tracking and release management.

## Task Progress Tracking
- When in **PLAN MODE**, add new tasks to `.clinerules/memory-bank/progress.md` with status `[ ]` and priority
- When in **ACT MODE**, mark completed tasks as `[x]` in the progress tracker
- Keep the progress tracker updated after each significant milestone
- Include task summary, files modified, and status changes

## Semantic Versioning
- Follow SemVer (MAJOR.MINOR.PATCH) for all version bumps
- **PATCH** (x.y.Z): Bug fixes, minor UI corrections
- **MINOR** (x.Y.z): New features, new transaction types, new UI sections
- **MAJOR** (X.y.z): Breaking changes, data model changes requiring migration
- Update version in `index.html` title tag: `Global Portfolio Engine vX.Y.Z`
- Version format in title: `v53.0` → increment appropriately

## Git Commit & Push
- After implementing a task, commit with a descriptive message in the format:
  ```
  feat: description of feature added
  fix: description of bug fixed
  refactor: description of refactoring
  docs: description of documentation change
  ```
- Use conventional commit prefixes: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`
- After commit, push to GitHub immediately
- Commit message should reference the task implemented

## Example Workflow
1. PLAN MODE: Add task to progress.md → `[ ] Add cash balance tracking`
2. ACT MODE: Implement the feature
3. ACT MODE: Mark task complete in progress.md → `[x] Add cash balance tracking`
4. ACT MODE: Update version number (e.g., v53.0 → v53.1 for minor feature)
5. ACT MODE: `git add . && git commit -m "feat: add full cash management for portfolio tracker"`
6. ACT MODE: `git push origin main`