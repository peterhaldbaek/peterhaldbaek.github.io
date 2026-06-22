# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The "Brisk Bee" personal blog — a [Jekyll](https://jekyllrb.com/) site using the
`minima` theme, served through GitHub Pages. Despite the repo name
(`peterhaldbaek.github.io`), the site is published at the custom domain in `CNAME`
(`www.briskbee.com`); `_config.yml` `url`/`email` reflect that domain, not GitHub.

## Commands

```sh
bundle install              # install gems (first run / after Gemfile changes)
bundle exec jekyll serve    # build + watch + serve at http://localhost:4000
bundle exec jekyll serve --drafts   # also render _drafts/
bundle exec jekyll build    # one-off build into _site/ (gitignored)
```

There is no test, lint, or CI step — pushing to `main` is what deploys (GitHub
Pages builds the site server-side). Note `_config.yml` is *not* hot-reloaded; restart
`serve` after editing it.

## Content conventions

- **Posts** live in `_posts/` named `YYYY-MM-DD-title.md` with front matter
  `layout: post` and a `title`/`date`. Author defaults to "Peter Haldbæk" via
  `_config.yml` `defaults`, so don't set it per-post. Permalinks are
  `/:year/:month/:title.html` (see `_config.yml`) — changing a filename or this
  pattern breaks existing URLs.
- **Pages** are top-level `.md` files (e.g. `about.md`, the `*-tv-oversigt.md`
  match-schedule pages) with `layout: page`. Add `show_in_menu: true` to surface a
  page in the site nav — `_includes/header.html` iterates `site.pages` and only
  renders ones with that flag set. Use `redirect_from:` (jekyll-redirect-from) to
  preserve old URLs when moving a page.
- **`_archive/`** and **`_drafts/`** hold posts that are intentionally *not*
  published. `_archive/` is a non-standard underscore dir, so Jekyll ignores it;
  `_drafts/` only renders with `--drafts`. To publish an archived post, move it into
  `_posts/` with a dated filename.

## Theme overrides

The site is built on the gem-based `minima` theme, but ships a custom dark-first
look called **Pollen** (neon nectar-pink + honey accents on a plum-ink background).
The override files here are what produce that look; many other layouts/styles still
live inside the gem. Files here override the gem by matching its path:
`_layouts/home.html`, `_layouts/post.html`, `_includes/head.html`,
`_includes/header.html`, `_includes/disqus_comments.html`, and `assets/main.scss`.
When something looks "missing," it's coming from the `minima` gem — add a same-named
file here to override it rather than editing the gem.

### Pollen theme details

- **`assets/main.scss` is self-contained** — it does *not* `@import "minima"`. All
  styling (base, header, hero, post feed, post page, code, footer, mobile nav) lives
  here, driven by CSS custom properties. Editing minima variables does nothing; edit
  the tokens at the top of this file.
- **Dark/light.** Theme is chosen by the `data-theme` attribute on `<html>`. Tokens
  are defined for `[data-theme="dark"]` (the default) and `[data-theme="light"]`. An
  inline script in `_includes/head.html` sets the attribute before first paint
  (reading `localStorage.theme`, defaulting to dark) to avoid a flash; the toggle
  button and its click handler live in `_includes/header.html` and persist the choice
  to `localStorage`.
- **Fonts** load from Google Fonts in `_includes/head.html`: `Spectral` (display /
  headings), `Work Sans` (body), `JetBrains Mono` (code, eyebrows, meta, tags). The
  display/body pairing is chosen for editorial heritage over the AI-default look.
  Code blocks are clean bordered panels — no colored accent stripe.
- **Tags.** Posts may set `tags: [a, b, c]` in front matter; they render as pills on
  the home feed (`_layouts/home.html`) and the post header (`_layouts/post.html`).
  Posts without tags simply show none — safe to adopt gradually.
- **Hero.** The home page shows a hero block (only on paginator page 1). Edit its
  copy in `_layouts/home.html`.

## Plugins in use

`jekyll-feed` (RSS), `jekyll-paginate` (10 posts/page at `/page/:num/`),
`jekyll-redirect-from`. Disqus comments are wired via the `disqus` shortname in
`_config.yml`.
