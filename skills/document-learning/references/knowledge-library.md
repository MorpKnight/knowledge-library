# Giovan's Knowledge Library

Read this profile when no other destination was specified, or when writing for Giovan's garden. These are local project conventions, not requirements for other knowledge bases.

## Locate and inspect

The default checkout is `~/Programming/documents/knowledge-library`, with Markdown under `content/`. The public site is <https://garden.giovanc.com/> and the repository is <https://github.com/MorpKnight/knowledge-library>.

An explicit path or workspace configuration takes precedence. When the active workspace is this repository, use its actual root rather than assuming the home-directory path. In another workspace, keep source and destination repositories distinct: inspect destination instructions and Git status before editing it. If sandbox permissions prevent the write, prepare a local draft and report the pending transfer; do not claim it was added to the garden.

Read `AGENTS.md`, `README.md`, `quartz.config.yaml`, the relevant topic/project pages, and similar notes before editing. Recheck conventions against the checkout instead of treating this profile as live configuration.

## Format and placement

Write garden notes, metadata, and captions in English. Preserve original source titles and quoted material where appropriate; conversation replies may use the user's language.

Use this frontmatter for a new draft, replacing the example date with the actual writing date:

```yaml
---
title: A specific learning or experiment title
description: A concise explanation of the question and takeaway.
date: YYYY-MM-DD
tags: []
publish: false
draft: true
---
```

Quartz is configured to render notes only with `publish: true` and without `draft: true`. Set `publish: true` and `draft: false` when the current task authorizes publication. These flags do not protect files in the public Git repository, including attachments. The repository-root `private/` directory is intended for local captures; verify it is ignored and that the intended file is not already tracked before placing private material there.

Choose the primary category from these existing topic maps:

| Directory                  | Use for                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------- |
| `ai-machine-learning/`     | Models, training, inference, evaluation, and ML methods                            |
| `medical-ai-research/`     | Medical imaging, research design, data provenance, calibration, and interpretation |
| `software-development/`    | Applications, architecture, programming, testing, and development tools            |
| `product-design/`          | Problem framing, user research, prototyping, interface and product decisions       |
| `infrastructure-homelab/`  | Servers, networking, self-hosting, cloud environments, and deployment              |
| `hardware-embedded/`       | Electronics, PCBs, devices, firmware, and embedded systems                         |
| `career-academic-journey/` | Education, interviews, research communication, and career reflection               |

Category maps are `content/<category>/index.md`. Project maps live in `content/projects/`; inspect what exists before choosing one. A reusable concept belongs under its primary topic, with a link from the relevant project page. A project retrospective may live with its project. Add a new category or reorganize navigation only when the material calls for it and the task supports that scope.

Use root-relative wikilink targets without the `content/` prefix or `.md`, for example `[[infrastructure-homelab/index|Infrastructure & Homelab]]`. Only link to pages that actually exist. Add a discoverability link to the appropriate map when publishing a note; keep unpublished drafts out of public maps.

## Verification and delivery

- Run `git diff --check` in the destination and inspect the exact changed files, including any new files.
- For changes to published content, run `npm run build` from the library root when dependencies are available. This verifies rendering, not the research claims. A draft is intentionally excluded from rendered output, so check its frontmatter and links directly.
- Verify sensitive local capture files with `git check-ignore` and `git ls-files`; an ignore rule does not untrack files already committed.
- A push to `main` triggers GitHub Pages. Use only current task/session authorization for commit, push, and deployment. Avoid `quartz sync` for a note-only capture because it can commit, pull, and push together.
- If deployment is requested, inspect the relevant workflow result and deployed page before reporting it live. Do not alter DNS, Cloudflare rules, or Pages settings for an ordinary documentation task.
