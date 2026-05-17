# Contributing

感谢你改进 Research Workflow Skill。Please keep contributions practical, evidence-aware, and useful for real research workflows.

## What to contribute

- Better phase checklists under `references/`.
- Stronger mentor or reviewer prompts.
- Realistic examples in `examples/`.
- Documentation improvements in `README.md` or `USAGE.md`.
- Bug reports about unclear routing, missing references, or invalid skill metadata.

## Contribution rules

- Keep the core skill domain-agnostic.
- Put subfield-specific guidance in examples unless it is broadly applicable.
- Do not add private data, unpublished reviewer comments, API keys, credentials, or copyrighted paper text.
- Preserve Simplified Chinese as the primary language for reference files.
- Keep `README.md` and `USAGE.md` bilingual when changing user-facing instructions.
- Ensure every path mentioned in `SKILL.md` exists.

## Local checklist

Before opening a pull request:

```powershell
python -c "import pathlib; p=pathlib.Path('SKILL.md').read_text(encoding='utf-8'); fm=p.split('---')[1]; print(fm)"
git status --short
```

Also read the changed Markdown files once in rendered preview or a Markdown viewer.

## Pull request style

- Explain the research workflow problem you are solving.
- List changed files.
- Mention whether you changed routing behavior in `SKILL.md`.
- Include at least one example prompt when changing mode or phase behavior.

## Main-only maintenance workflow

This repository is maintained directly on `main`. Do not create `feature/*` or `codex-*` branches for routine maintenance. Maintenance logs, package snapshots, temporary drafts, and local `plans/`, `memory/`, `sessions/`, or `transcripts/` directories should stay out of version control.
