# Adding a project or a paper

The site uses the Blowfish theme (vendored in `themes/blowfish`, Hugo ≥ 0.164; CI pins 0.167.0).

## New project

    hugo new projects/my-project/index.md

1. Fill in the front matter (the archetype documents every field) and set `draft: false`.
2. Drop an image called `feature.jpg` in `content/projects/my-project/`. It is used for the card and the page header.
3. Keep `{{</* project-meta */>}}` at the top of the body: it renders period, affiliation, role, status, links and the citation from the front matter.
4. `weight` orders the cards (lower first).

## New paper

Add an entry at the top of `data/publications.yaml` (title, authors, venue, pages, year, type, doi). It appears on `/publications/` and in the CV automatically.

## Preview

    hugo server        # http://localhost:1313
    hugo server -D     # also show drafts
