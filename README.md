# umutonuryasar.com

Personal academic website of **Umut Onur Yaşar** — Applied AI Research Engineer.

Built with [Jekyll](https://jekyllrb.com/) on the [Academic Pages](https://github.com/academicpages/academicpages.github.io) template, hosted on GitHub Pages.

## Local development

```bash
bundle install
bundle exec jekyll serve -l -H localhost
```

Or with Docker:

```bash
docker compose up
```

## Structure

- `_pages/` — static pages (About, CV, Portfolio, Publications)
- `_posts/` — blog posts
- `_portfolio/` — project pages
- `_publications/` — publication entries
- `_data/cv.yml` — structured CV data rendered at `/cv/`
