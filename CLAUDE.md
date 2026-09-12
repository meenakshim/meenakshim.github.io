# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jekyll site (`meenakshim.github.io`, deployed via GitHub Pages) built on the `mmistakes/minimal-mistakes@4.24.0` remote theme. The theme's layouts/includes/sass are *not* vendored in this repo — they're fetched at build time by the `jekyll-remote-theme` plugin. To customize theme output without forking it, drop overrides in `_includes/head/custom.html` (a supported minimal-mistakes hook, rendered into `<head>`) rather than trying to create local `_layouts/*.html` overrides for theme layouts.

The site is a single scrolling page (`index.md`, `layout: single`, `author_profile: true`) with `## Heading` sections; Kramdown auto-generates heading `id`s from the text (e.g. `## Research` → `id="research"`), and `_data/navigation.yml` links to those as in-page anchors (`/#research`) rather than separate pages. There are no blog posts (`_posts/` is empty) and no other content pages (`_pages/` is empty) — all page content lives in `index.md`.

## Commands

Requires Ruby + Bundler. On this machine the repo lives on a WSL filesystem but is edited from Windows; Ruby/Jekyll are only installed inside the actual WSL Ubuntu distro, not in Windows Git Bash. Run toolchain commands via:

```
wsl.exe -d Ubuntu -- bash -lc 'cd ~/git/meenakshim.github.io && <command>'
```

- Install gems: `bundle install`
  - The system gem dir isn't user-writable here; if `bundle install` fails with a `Bundler::PermissionError`, run `bundle config set --local path "vendor/bundle"` first, then `bundle install` again.
  - If `bundle`/`jekyll` aren't found even though `gem list bundler` shows it installed, the gem bin dir is missing from `PATH` — add it: `export PATH="$PATH:$(ruby -e 'puts Gem.user_dir')/bin"`.
- Build: `bundle exec jekyll build` (add `--destination <dir>` to build to a scratch dir instead of the default `_site/`)
- Serve locally with live reload: `bundle exec jekyll serve`

There is no test suite, linter, or CI config in this repo.

Build artifacts (`_site/`, `.jekyll-cache/`, `vendor/`) are gitignored — clean them up after local builds (`rm -rf _site .jekyll-cache vendor .bundle`) rather than leaving them around.

## Architecture / key files

- `_config.yml` — theme (`remote_theme`), nav include path (`_pages`), the `defaults` block (sets `layout: single`, `author_profile: true` for anything under `_pages`), and the `author:` block. `author.image`/`avatar` + `author.bio` + `author.links` (label/icon/url triples, icons are Font Awesome classes) drive the left sidebar rendered whenever a page has `author_profile: true` — this is how the photo/bio/contact-links sidebar is populated, not something built by hand in a layout.
- `_data/navigation.yml` — the `main:` list drives the masthead nav. Entries are plain `{title, url}` pairs; anchors like `/#research` work fine since they're just links.
- `index.md` — the entire site content. Sections are `## Heading` markdown; the heading text is what determines the anchor id used in `_data/navigation.yml`, so renaming a section heading breaks its nav link unless you update both.
- `_includes/head/custom.html` — custom `<style>`/`<head>` injection point (rendered into every page's `<head>`, right after the theme's own CSS/JS). This is where the enlarged-avatar and smooth-scroll CSS overrides live, and is the place to add further theme tweaks without touching remote theme files.
- `404.html` — standalone, `layout: default`, styled inline.
- `assets/images/author.png`, `assets/pdf/resume.pdf` — referenced directly by `_config.yml`'s `author.image`/`avatar` and by the "CV" nav entry, respectively.
