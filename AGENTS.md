# AGENTS.md — nooneswarup.github.io

This is a personal blog built with Jekyll and the Chirpy theme, deployed via GitHub Pages.

## Common Commands

- **Preview locally:** `bundle exec jekyll serve` (http://127.0.0.1:4000, live reload)
- **New post:** `bundle exec jekyll post "Title"`
- **New draft:** `bundle exec jekyll draft "Title"`
- **Build + test:** `bash tools/test.sh`
- **Dev container:** VS Code → "Reopen in Container" for isolated Ruby/Jekyll env

## Content Locations

| What | Where |
|---|---|
| Blog posts | `_posts/YYYY-MM-DD-slug.md` |
| Drafts | `_drafts/` |
| Static pages | `_tabs/` |
| Images | `assets/img/` |
| Site config | `_config.yml` |
| Author data | `_data/authors.yml` |

## Post Frontmatter

Every post must include:

```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM -0500
author: nooneswarup
categories: [Category]
tags: [tag1, tag2]
---
```

## Guidelines for Changes

- **Content edits only** — edit posts, pages, and data files freely
- **Do not modify** theme files, build config, CI, or dev container unless explicitly asked
- **No comments in code** unless asked
- **No emojis** in content unless asked
- **Spelling/grammar** fixes are always welcome
- After editing content, verify with `bundle exec jekyll serve` (the user runs this)
- Deploy happens automatically on push to `main` via GitHub Actions (`.github/workflows/pages-deploy.yml`)
