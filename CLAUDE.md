# CLAUDE.md

This file provides guidance for AI assistants (Claude Code) working in this repository.

## What this repo is

This is a GitHub Learning Lab course repository: a [Jekyll](https://jekyllrb.com/)-powered, [reveal.js](https://github.com/hakimel/reveal.js/)-based slideshow that runs as a GitHub Pages site. Each "slide" is a Markdown file; Jekyll renders them into a single scrollable/navigable HTML presentation. There is no application code, build pipeline beyond Jekyll, or test suite in the traditional sense — this is a content/config repo.

## Repository structure

```
_posts/             One Markdown file per slide (Jekyll "posts" used as slides)
_layouts/           HTML page layouts (presentation, slide, print)
_includes/          Reusable HTML/Liquid fragments (head, script, slide)
script/             Shell scripts for setup, local server, CI build, staging
node_modules/        reveal.js bundled here (vendored slideshow engine, NOT app deps)
_config.yml         Jekyll + reveal.js configuration (single source of truth for site behavior)
index.html           Entry point; loops over site.posts to assemble the deck
Gemfile / Gemfile.lock   Ruby deps (github-pages gem, html-proofer)
package-lock.json     Pins reveal.js version (no real package.json/npm scripts)
```

### Slides (`_posts/`)

- Each slide is a Markdown file with Jekyll front matter, named with a date prefix per Jekyll's post convention, e.g. `_posts/0000-01-01-intro.md`.
- Front matter fields used by the layouts/includes:
  - `layout: slide` — required to render through `_layouts/slide.html` → `_includes/slide.html`.
  - `title` — rendered as an `<h1>` on the slide (omit/empty to suppress).
  - `slide-id` — optional, sets the section's `id`.
  - `classes` — optional list, added as CSS classes on the slide's `<section>`.
  - `data` — optional list of `[key, value]` pairs rendered as `data-*` attributes (used by reveal.js for things like backgrounds/transitions).
- Slide order is controlled by `index.html`, which iterates `site.posts reversed` — since Jekyll posts sort newest-first by date, `reversed` puts oldest dates first. **To control slide order, set each post's date in the filename/front matter**, not the order files were created.
- Example minimal slide:
  ```markdown
  ---
  layout: slide
  title: "My Slide Title"
  ---

  Slide body in Markdown.
  ```

### Layouts (`_layouts/`)

- `presentation.html` — the main reveal.js deck wrapper; used by `index.html`.
- `slide.html` — standalone single-slide page (used for previewing one slide outside the deck).
- `print.html` — minimal layout for print/PDF export.

### Includes (`_includes/`)

- `head.html` — `<head>` contents: page title, reveal.js CSS (reset/reveal/theme `moon.css`/highlight `monokai.css`), and the print/PDF stylesheet swap script.
- `script.html` — loads `reveal.js` and calls `Reveal.initialize(...)` with the markdown/highlight/notes plugins.
- `slide.html` — renders a single post as a `<section class="step ...">` per the front matter described above.

## Configuration (`_config.yml`)

This is the most important file to read before changing site behavior. Key sections:

- Jekyll basics: `timezone`, `markdown: kramdown`, `permalink`, `highlighter: rouge`.
- `baseurl` is commented out — leave it commented out unless deploying under a subpath (the `cibuild` script builds with `--baseurl "."` instead).
- `title` / `author` / `description` — site/course metadata shown in the page `<title>`.
- `reveal:` — maps directly to `Reveal.initialize()` options consumed indirectly (current `script.html` hardcodes a minimal initialize call; this block largely documents intended options such as `controls`, `progress`, `transition`, `width`/`height`, etc. — check `_includes/script.html` if you need these to actually take effect).
- `exclude:` — files Jekyll should not process/copy into `_site` (Gemfile, vendor dirs, extraneous reveal.js files).

## Development workflow

Setup, local serving, and CI all go through the `script/` directory — use these rather than ad hoc commands:

```sh
script/setup    # installs Ruby (rbenv) + bundler gems, runs `git submodule update --init`
script/server   # bundle exec jekyll serve — local dev server with live rebuild
script/cibuild  # bundle exec jekyll build --baseurl "." ; then html-proofer checks _site/index.html
script/stage    # internal-only: builds with a staging baseurl and force-pushes _site to a staging GHE remote
```

- `script/setup` references a `.gitmodules`-based submodule, but `.gitmodules` is currently empty (reveal.js was migrated to a vendored `node_modules` checkout — see git history: "removed submodule", "use node module for reveal.js"). The `git submodule update --init` step is effectively a no-op now; don't reintroduce a reveal.js submodule.
- There is no `package.json`/npm install step — `node_modules/reveal.js` is committed directly to the repo as a vendored dependency, not installed via npm. Don't add a build step that assumes `npm install` will fetch it.
- `script/stage` pushes to an internal GitHub Enterprise staging remote (`ghe.io`) — this is GitHub-internal tooling, not relevant/usable for typical contributors or CI.

### Validating changes

- Run `script/cibuild` to build the Jekyll site and run `html-proofer` against the generated `_site/index.html` (checks for broken links/images; `--empty-alt-ignore` allows images without alt text).
- There are no unit/integration tests beyond this — verifying a slide change means building the site and/or running `script/server` and checking the rendered deck in a browser.

## Conventions

- **Indentation**: see `.editorconfig`.
  - Default: tabs.
  - JSON/JS/CSS/SCSS/YAML/HTML: 2-space indent.
  - Markdown: 4-space indent, trailing whitespace preserved (used for Markdown line breaks), final newline required.
- Keep `_site/`, `.sass-cache/`, `.jekyll-metadata`, and `.bundle` out of commits (already gitignored) — these are all build artifacts.
- This repo is consumed by GitHub Learning Lab as course material; the README and `_posts/0000-01-01-intro.md` content is learner-facing copy, not developer documentation — be careful not to confuse the two when editing.

## Making changes

- **Adding a slide**: add a new file to `_posts/` with an appropriate date-ordered filename and `layout: slide` front matter (see example above).
- **Changing the deck/site config**: edit `_config.yml`; remember `reveal:` block changes may need corresponding wiring in `_includes/script.html` to take effect.
- **Changing global slide chrome** (theme, scripts, meta): edit `_includes/head.html` / `_includes/script.html`, not the layouts directly.
- **Upgrading reveal.js**: it's vendored under `node_modules/reveal.js` and pinned via `package-lock.json`; update both together rather than relying on `npm install`.
