# Codex Skills Library

This repository is initialized to store and manage your Codex skills in one place.

## Repository Layout

```text
.
├── skills/
│   ├── README.md
│   ├── _template/
│   │   └── SKILL.md
│   └── pr-change-summary/
│       └── SKILL.md
└── .gitignore
```

## Conventions

- Each skill lives in `skills/<skill-name>/`.
- The skill definition file is always `SKILL.md`.
- Use lowercase kebab-case for skill folder names.

## Add a New Skill

1. Copy `skills/_template/` to `skills/<new-skill-name>/`.
2. Update `SKILL.md` with purpose, triggers, workflow, and examples.
3. Commit the change.

## Suggested Next Steps

1. Populate `skills/pr-change-summary/SKILL.md`.
2. Add more skills by copying from the template.
