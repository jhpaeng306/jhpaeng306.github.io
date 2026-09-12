# jhpaeng306.github.io

Personal academic website of Jinhee Paeng, built with [al-folio](https://github.com/alshedivat/al-folio) (Jekyll).

**To edit the site, start with [EDITING.md](EDITING.md)** — it lists which file to touch for every part of the site.

## How it deploys

Pushing to the `master` branch triggers the GitHub Actions workflow in `.github/workflows/deploy.yml`.
It builds the site and publishes the result to the `gh-pages` branch, which GitHub Pages serves at
https://jhpaeng306.github.io. You never edit `gh-pages` by hand.

One-time setup (already done if the site is live): **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `gh-pages` / (root)**, and **Settings → Actions → General → Workflow permissions → Read and write permissions**.

## Previewing locally (optional)

**With Docker** (easiest — install [Docker Desktop](https://www.docker.com/products/docker-desktop/) once):

```bash
docker compose up
```

Open http://localhost:8080. Edits to Markdown/YAML/BibTeX files reload automatically; stop with Ctrl+C.
The first run downloads the al-folio image (~1 GB) and installs gems, so it takes a few minutes; later runs are fast.

**With Ruby** (Ruby ≥ 3.3, e.g. `brew install ruby` or rbenv; plus `brew install imagemagick`):

```bash
bundle install
bundle exec jekyll serve
```

Open http://localhost:4000. You do not need a local build to publish — just commit and push.
