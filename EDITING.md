# Editing guide

Everything you normally change is plain text: Markdown, YAML, or BibTeX. You never need to touch HTML or CSS unless you want to restyle the site. The workflow is always the same:

1. Edit a file (any text editor works; VS Code is nice for YAML indentation).
2. Optional: preview with `docker compose up` in the repo folder → http://localhost:8080. Edits reload automatically.
3. Commit and push to `master`. The "Deploy site" GitHub Action rebuilds the site in a couple of minutes.

## Where things live

| I want to change…                             | Edit this                                                  |
| --------------------------------------------- | ---------------------------------------------------------- |
| Home page text (bio, interests, education)    | `_pages/about.md`                                          |
| Subtitle under your name on the home page     | `subtitle:` at the top of `_pages/about.md`                |
| Photo on the home page                        | replace `assets/img/prof_pic.jpg` (any size; portrait or square) |
| Text under the photo (location, email)        | `profile.more_info` at the top of `_pages/about.md`        |
| Icons at the bottom of the home page          | `_data/socials.yml`                                        |
| Publications                                  | `_bibliography/papers.bib`                                 |
| Which papers show on the home page            | `selected = {true}` on the entry in `papers.bib`           |
| News items (home page + News page)            | add a file to `_news/`                                     |
| Talks, slides, notes                          | bottom of `_pages/publications.md`                         |
| CV page sections (education, experience, …)   | `_data/cv.yml`                                             |
| CV PDF                                        | replace `assets/pdf/CV.pdf`                                |
| Slides / posters / notes files                | put the file in `assets/pdf/`, link it as `/assets/pdf/name.pdf` |
| Site name, description, favicon emoji         | top of `_config.yml`                                       |
| Navigation order                              | `nav_order:` in the front matter of each file in `_pages/` |
| Fonts, colors, spacing, rounded corners       | `_sass/_custom.scss` (see "Changing the design" below)     |

## Home page (`_pages/about.md`)

The block between the two `---` lines at the top is settings (photo, subtitle, whether to show news and selected papers). Everything below it is ordinary Markdown: `**bold**`, `[link text](https://…)`, `- bullet`, `#### small heading`. This is the first thing to update: the bio still says "starting from September 2024".

## Publications (`_bibliography/papers.bib`)

Add a BibTeX entry, exactly as you would for a paper. Standard fields (`title`, `author`, `journal` or `booktitle`, `year`, `volume`, `pages`, …) are rendered automatically. Extra fields add buttons under the entry:

| Field                  | What it does                                            |
| ---------------------- | ------------------------------------------------------- |
| `selected = {true}`    | also lists the paper on the home page                   |
| `abbr = {NeurIPS}`     | venue badge on the left (colors/links in `_data/venues.yml`) |
| `arxiv = {2305.12211}` | "arXiv" button                                          |
| `pdf = {file.pdf}`     | "PDF" button; the file goes in `assets/pdf/`            |
| `slides = {file.pdf}`  | "Slides" button                                         |
| `poster = {file.pdf}`  | "Poster" button                                         |
| `video = {https://…}`  | "Video" button                                          |
| `html = {https://…}`   | "HTML" button (publisher page)                          |
| `code = {https://…}`   | "Code" button                                           |
| `abstract = {…}`       | "Abs" button that unfolds the abstract                  |
| `bibtex_show = {true}` | "Bib" button that shows the BibTeX                      |

Write authors as `Last, First and Last, First` so the site can bold your own name. Entries are grouped by year, newest first. To make the paper list link coauthors' homepages, add them to `_data/coauthors.yml` (example inside).

## News (`_news/`)

One file per item, named `YYYY-MM-DD-anything.md`:

```markdown
---
layout: post
date: 2026-09-05 09:00:00-0700
inline: true
related_posts: false
---

One or two sentences, with [links](https://example.com) if you like.
```

The newest five appear on the home page (change `limit:` in `_pages/about.md`), all of them on the News page. For a longer post with its own page, set `inline: false` and add a `title:` line.

## Talks and slides (`_pages/publications.md`)

The list at the bottom of the file is plain Markdown. Copy a bullet, change the text and links:

```markdown
- **Title of the talk** — Where, When
  <span class="links">[Slides](/assets/pdf/my_slides.pdf) [Video](https://youtu.be/…)</span>
```

The `<span class="links">…</span>` line is optional; it becomes the row of small pill buttons.

## CV page (`_data/cv.yml`)

A YAML file; the comment at the top describes the format. Each key under `sections:` becomes a card, in the order listed. `Education`, `Experience`, `Awards`, `Skills`, `Languages` and `Interests` have dedicated layouts; any other name (`Teaching`, `Activities`, or one you invent) is a simple list of `label:` / `details:` pairs. Experience entries are sorted by date automatically; omit `end_date` for something ongoing. Dates can be `2024` or `2024-09-01`; only the year is shown.

YAML cares about indentation, so copy an existing entry and edit it rather than typing from scratch. If the build fails after a CV edit, it is almost always a missing quote around a value containing a colon (`"Advisor: …"`).

The "Download CV" button in the table of contents and the PDF icon in the title both point to `cv_pdf:` in `_pages/cv.md`.

## Adding a page

Create `_pages/something.md`:

```yaml
---
layout: page
permalink: /something/
title: something
description: One line shown under the title.
nav: true
nav_order: 4
---
```

and write Markdown below. `nav: false` keeps a page off the navigation bar.

## Changing the design

Short answer: small changes are easy, big ones are not.

All the styling that makes this site different from stock al-folio lives in one file, `_sass/_custom.scss`, organized in commented sections (typography, colors, navigation bar, home page, publications, news, CV, table of contents). It is ordinary CSS with a few named variables at the top of some sections. Things you can change in seconds:

- **Accent color**: `--global-theme-color` (light) and the same line under `html[data-theme="dark"]`.
- **Fonts**: the `$site-font` list at the top. To use a Google Font instead of the system font, put its name first in that list and add it to the `google_fonts` URL in `_config.yml`.
- **Text size / line height**: the `body` block.
- **CV spacing**: `$cv-card-gap`, `$cv-card-padding`, `$cv-entry-padding` at the top of the CV section.
- **Rounded corners**: search for `border-radius`.
- **Table of contents**: `$toc-width` and `$toc-min-viewport`, and the `#cv-download` block for the download pill.
- **Pill buttons**: the `.links a.btn` (publications) and `.talks .links a` blocks.

Recipe: run the Docker preview, right-click the thing you want to change in the browser → "Inspect" to see its class name and current CSS, add a rule for that class at the end of `_custom.scss`, save, and the page reloads. Because `_custom.scss` is loaded last, your rule wins over the theme's.

Harder changes — moving elements around, changing what a page shows, new page types — mean overriding one of the theme's layout files (they live inside the al-folio Ruby gems, not in this repo). Two are already overridden here and can serve as examples: `assets/css/main.scss` (theme stylesheet plus `@use "custom";`) and `_layouts/cv.liquid` (theme CV layout plus `{{ content }}` so `_pages/cv.md` can add the download link and rename the first card to "Information"). For anything of that kind, ask for help; the al-folio docs are at https://github.com/alshedivat/al-folio/tree/main/docs.

## Features that are off but available

- **Back-to-top button**: `back_to_top: true` in `_config.yml`.
- **Search (Ctrl+K)**: `search_enabled: true` in `_config.yml`.
- **Blog**: add `_posts/` and a `_pages/blog.md` from the al-folio template; set `pagination.enabled: true`.
- **Projects**: add `_projects/` and a `_pages/projects.md` from the template.
- **Citation counts / badges** on publications: `enable_publication_badges` in `_config.yml` plus your Google Scholar id in `_data/socials.yml`.
- **Google Analytics**: `analytics.google` in `_config.yml`.

## Things to update soon

Content was carried over from the old site as-is:

- `_pages/about.md`: bio says "starting from September 2024"; add current lab, advisor and research direction.
- `_bibliography/papers.bib`: anything since 2023; status of the JMAA paper.
- `_pages/publications.md` (talks) and `_data/cv.yml`: Stanford-era talks, TA roles, experience.
- `assets/pdf/CV.pdf`: current CV.
- `_data/socials.yml`: `scholar_userid` (the 12-character `user=` value in your Google Scholar profile URL; the old site's `jhpaeng306` is not a valid id), and LinkedIn / ORCID if you like.

## Upgrading al-folio

The theme is a set of Ruby gems pinned in `Gemfile` / `Gemfile.lock`. Upgrading means bumping those versions (see al-folio release notes and `docs/INSTALL.md` → "Upgrading") and checking the two overridden files above still match the new theme. Content files are unaffected.
