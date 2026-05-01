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
   - Detect project shape:
     - Multi-component repo (for example backend + frontend + admin + migrations).
     - Single-component repo (for example frontend only).
5. Produce a concise PR summary using the output format below.

## Output
- Return a single copy-paste-ready markdown block only.
- Do not include any text before or after the markdown block.
- Use exactly these 4 sections in this order:

```md
## Summary
<1 short paragraph on what changed and why>

## Changes
<Choose one style based on repo shape>

<Style A: multi-component repo>
### Backend (`<path>`)
- <change>
- <change>

### Frontend (`<path>`)
- <change>
- <change>

### Admin (`<path>`)
- <change>
- <change>

### Migration (`<path or id>`)
- <change>

<Style B: single-component repo>
### Feature: <feature name> (`<path>`)
- <change>
- <change>

### Page: <page/screen name> (`<path>`)
- <change>
- <change>

### Service: <service/module name> (`<path>`)
- <change>
- <change>

## Tests
- <test updates made>
- Verified with: `<command>` (<result>)

## Notes
- <important rollout/migration/risk note>
```

- Keep it factual and grounded in the diff only.
- Keep section headers exactly as shown for consistent GitHub PR formatting.
- In `Changes`, use only one style:
  - Style A for multi-component repos.
  - Style B for single-component repos.
- Include only subsections that exist in the actual changes.
- If a required section has no content, keep it and write `- None.`.

## Guardrails
- Never invent behavior not present in commits/diff.
- Explicitly call out missing tests or unclear intent.
- If both `main` and `master` are missing, stop and ask user which base branch to compare against.
- Do not wrap the final markdown in triple backticks unless user explicitly asks for a fenced block.
- Forbidden headings/sections: `Scope`, `Commits`, `Changed files`, `Files changed`, `Range`, `Base`, `Behavior Matrix`, `Validation`, `Risks / Follow-ups`, `What changed`.
- Final self-check before responding: ensure output starts with `## Summary` and includes only `Summary`, `Changes`, `Tests`, and `Notes`.
- Final self-check for `Changes`: never mix Style A and Style B in the same output.
