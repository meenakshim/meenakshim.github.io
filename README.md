# meenakshim.github.io

Personal website for Meenakshi Mani — a single-page Jekyll site covering her intro/bio, research, and contact info, published via GitHub Pages.

Built on the [`minimal-mistakes`](https://mmistakes.github.io/minimal-mistakes/) theme (loaded remotely, not vendored in this repo — see `_config.yml`'s `remote_theme`).

## Local development

Requires Ruby + Bundler.

```sh
bundle install
bundle exec jekyll serve
```

This serves the site at `http://localhost:4000` with live reload. Use `bundle exec jekyll build` to build a static copy into `_site/` (gitignored) without serving it.

If `bundle install` fails with a permission error writing to the system gem directory, install into a local, project-scoped path instead:

```sh
bundle config set --local path "vendor/bundle"
bundle install
```

(`vendor/` is not currently gitignored — clean it up, along with `_site/` and `.jekyll-cache/`, after local builds rather than committing it.)

## Structure

- `index.md` — all site content. It's a single scrolling page; each `## Heading` becomes its own section with an auto-generated anchor id (e.g. `## Research` → `#research`).
- `_data/navigation.yml` — top nav bar entries. Research/Contact link to in-page anchors (`/#research`, `/#contact`); CV links straight to the PDF.
- `_config.yml` — site metadata, theme selection, and the `author:` block (name, photo, bio, contact links) that populates the left sidebar shown on every page.
- `_includes/head/custom.html` — small custom CSS injected into `<head>` (currently: a larger author photo, smooth-scrolling for anchor links). This is the place to add theme tweaks without forking the remote theme.
- `assets/images/author.png`, `assets/pdf/resume.pdf` — profile photo and CV, referenced from `_config.yml` and the nav respectively.
- `404.html` — standalone not-found page.

There are no blog posts (`_posts/` is empty) and no separate content pages (`_pages/` is empty) — everything lives on the one page.

## Deployment

Static output is published via GitHub Pages directly from this repo; there's no separate build/deploy pipeline to run.
