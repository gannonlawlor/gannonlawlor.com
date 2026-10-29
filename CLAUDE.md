# CLAUDE.md

## Project

Personal blog at gannonlawlor.com, built with Hugo and the PaperMod theme.

## Stack

- **Hugo** (v0.158+, extended) — static site generator
- **PaperMod** — theme (in `themes/PaperMod/`, do not edit directly)
- **Cloudflare** — hosting and analytics

## Structure

- `content/posts/` — blog posts (Markdown with YAML front matter)
- `layouts/_partials/` — template overrides (extends PaperMod)
- `static/` — static assets (images, favicon, etc.)
- `hugo.toml` — site configuration

## Commands

- `hugo server -D` — local dev server with drafts
- `hugo` — production build (outputs to `public/`)

## Conventions

- Posts use YAML front matter (`title`, `date`, `draft`, `tags`, etc.)
- Override theme templates by placing files in `layouts/`, not by editing `themes/PaperMod/`
- Cloudflare analytics token is set via `HUGO_PARAMS_CLOUDFLAREANALYTICSTOKEN` env var
- Branch `main` is the primary branch; `release` is for deploys
