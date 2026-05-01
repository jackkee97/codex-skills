# pr-change-summary

## Purpose
Generate a clear PR change summary from the current working branch against `main` or `master`.

## When to Use
- User asks to summarize PR changes.
- User wants release/review notes for the current branch.

## Inputs
- Current git branch.
- Optional context: ticket link, acceptance criteria, test notes.

## Workflow
1. Detect base branch:
   - Prefer `main` if it exists.
   - Fallback to `master` if `main` does not exist.
2. Determine comparison range:
   - Use merge-base to avoid including unrelated base-branch commits.
   - Range should be `$(git merge-base HEAD <base>)..HEAD`.
3. Gather change data:
   - Changed files: `git diff --name-status <range>`.
   - Commit list: `git log --oneline <range>`.
   - Diff details: `git diff --stat <range>` and targeted hunks when needed.
4. Group changes by concern:
   - Feature/behavior changes.
   - Bug fixes.
   - Refactors/internal cleanups.
   - Tests/docs/config changes.
5. Produce a concise PR summary using the output format below.

## Output
- Use this exact structure:

```md
## Summary
<2-4 bullets on what changed and why>

## Changes by Area
- <area 1>: <what changed>
- <area 2>: <what changed>

## Files of Interest
- `<path>`: <brief reason this file changed>

## Testing
- <tests run>
- <manual verification>

## Risks / Follow-ups
- <known risk, migration, or rollout note>
```

- Keep it factual and grounded in the diff only.

## Guardrails
- Never invent behavior not present in commits/diff.
- Explicitly call out missing tests or unclear intent.
- If both `main` and `master` are missing, stop and ask user which base branch to compare against.
