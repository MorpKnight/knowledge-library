# Knowledge Library

A personal digital garden for learning in public, built with [Quartz 5](https://quartz.jzhao.xyz/). Notes are plain Markdown, connected by links, search, and a graph, and published as a static website.

[Explore the site](https://garden.giovanc.com/) · [GitHub repository](https://github.com/MorpKnight/knowledge-library) · [Quartz documentation](https://quartz.jzhao.xyz/)

## What is inside

The library brings together notes and project maps across AI and machine learning, medical research, software development, product and design, infrastructure, hardware, and career development. Topic and project pages are starting points; they do not claim that every proposed note or result is complete.

## Get started

Requirements: Node.js 22 or later, npm 10.9.2 or later, and Git. The setup has been verified with Node.js 24.

```sh
npm ci
npx quartz plugin install
npm run dev
```

Open <http://localhost:8080>. The development server rebuilds when content changes; stop it with `Ctrl+C`.

> [!NOTE]
> This project uses npm-based Quartz plugins and currently has no `quartz.lock.json`. The plugin install command may report that the file is missing; npm dependencies are installed from `package-lock.json`.

To build the static site:

```sh
npm run build
```

The generated site is written to `public/` and is not tracked by Git.

## Write and publish

- Write Markdown in `content/`; `content/index.md` is the home page.
- Open `content/` as an Obsidian vault, or edit the same files with a text editor or an AI agent.
- Copy `content/templates/catatan.md` when starting a note. The template directory is excluded from builds.
- Use Obsidian-style links such as `[[projects/c67|C67]]` to connect notes.
- A note is published only when its frontmatter has `publish: true` and does not have `draft: true`.

```yaml
---
title: A note title
description: A short summary.
publish: true
draft: false
---
```

> [!IMPORTANT]
> Markdown publication filters do not guarantee that attachments are excluded. Files committed to this public repository are publicly accessible even if Quartz does not render them. Review notes, attachments, and Git changes before publishing.

## Project structure

```text
content/                Notes, topic maps, and project pages
content/templates/      Unpublished writing templates
quartz.config.yaml      Site, plugin, and layout configuration
quartz.ts               Advanced programmatic overrides
docs/quartz-research.md  Initial Quartz research
.github/workflows/      GitHub Actions workflows
public/                 Generated output (ignored by Git)
```

## Deployment

Pushes to `main` trigger `.github/workflows/pages.yml`, which installs dependencies, builds the site, and deploys `public/` to GitHub Pages. The workflow can also be run manually from the Actions tab. See [deployment runs](https://github.com/MorpKnight/knowledge-library/actions/workflows/pages.yml).

GitHub Pages uses `garden.giovanc.com` as its custom domain, with DNS managed by Cloudflare. Quartz's `baseUrl` is set to that hostname. See [GitHub's custom domain guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) when changing domains.

## Maintenance

- `origin` points to this repository; `upstream` points to Quartz. The starting upstream revision was `97a2d05` on `v5`.
- Keep the working tree clean and review the [Quartz upgrade guide](https://quartz.jzhao.xyz/getting-started/upgrading) before updating. `quartz sync` can commit, pull, and push; review changes before using it.
- Upstream workflows remain in `.github/workflows/` and are restricted to the Quartz repository; `pages.yml` deploys this site.
- The optional Quartz theme is disabled and the unavailable Excalidraw plugin reference was removed during setup. The site uses Quartz's standard appearance.
- The npm audit performed during initial setup reported two moderate findings involving `minimatch` and `serve-handler`; the proposed automatic fix involved a major-version change. Re-run `npm audit` for the current status.
