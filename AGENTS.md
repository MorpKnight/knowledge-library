# Knowledge Library

This repository contains the English-language Quartz garden at <https://garden.giovanc.com/>. Read `README.md` and the current configuration before changing writing or deployment conventions.

## Document learning

Use the `document-learning` skill for requests to capture lessons, study notes, troubleshooting findings, project reflections, or experiment results. The canonical entrypoint is [skills/document-learning/SKILL.md](skills/document-learning/SKILL.md). Agents without native skill discovery can read that file and its relevant references directly.

The repository's `.agents/skills/document-learning` and `.claude/skills/document-learning` links point to that canonical directory. User-level skill links on the author's machine make the same skill available from other workspaces. Keep the instructions and templates in one place.

## Content conventions

- Notes live in `content/`; choose an existing topic and connect relevant project pages.
- Write new content, metadata, and captions in English. Preserve existing slugs unless a URL migration is part of the task.
- Ground results in inspected artifacts. Distinguish observations, interpretations, plans, and missing evidence; retain failed experiments and limitations.
- New captures default to `publish: false` and `draft: true`. Published notes use `publish: true` and `draft: false`. Check current task authorization before publication or external Git actions.
- This repository is public. Draft flags do not hide files or attachments in Git. Verify `private/` is ignored and files are untracked before using it for local private captures.
- Check links, frontmatter, and diffs. For published content changes, run `npm run build`; report any verification that could not be completed.
