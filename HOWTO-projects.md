# Adding a project to the site

Every project is one markdown file in `content/projects/`. The card grid on
`/projects/` and the detail page are generated from its front matter.

## 1. Create the file

```bash
hugo new projects/my-new-project.md
```

This copies `archetypes/projects.md`, which has every field with a comment.
The file name becomes the URL: `/projects/my-new-project/`.

## 2. Fill in the front matter

| Field            | Used where            | Notes                                                        |
|------------------|-----------------------|--------------------------------------------------------------|
| `title`          | card, page, sidebar   |                                                              |
| `summary`        | card                  | 1–2 sentences. If empty Hugo uses the first ~70 words.       |
| `period`         | card, page            | Free text, e.g. `"Jun 2026 – present"`. Falls back to year.  |
| `affiliation`    | card, page            | e.g. `"Honda Research Institute Europe"`                     |
| `role`           | page                  | e.g. `"PhD research"`                                        |
| `status`         | page                  | e.g. `"Ongoing"`, `"Published"`, `"Completed"`               |
| `tags`           | filters, card, page   | Also creates `/tags/<tag>/` pages. Reuse existing tag names. |
| `featuredImage`  | card, page            | Path under `static/`, e.g. `/images/projects/foo.png`        |
| `links`          | card, page            | List of `{name, url}`; e.g. Paper, Code, Demo                |
| `publication`    | page (bottom box)     | Citation text, markdown allowed                              |
| `weight`         | ordering              | Lower = higher on the list. `0` = sort by date.              |
| `draft`          | build                 | Set to `false` to publish.                                   |

## 3. Write the body

Anything below the front matter is the detail page. The archetype suggests
`## Overview`, `## Approach`, `## Results`, but any markdown works: images
(`![caption](/images/projects/foo.png)`), tables, code blocks, KaTeX maths.

## 4. Preview and publish

```bash
hugo server -D          # -D also shows drafts; open http://localhost:1313/projects/
git add content/projects/my-new-project.md static/images/projects/
git commit -m "project: my new project"
git push                # GitHub Actions rebuilds https://danialzendehdel.com
```

## Where the pieces live

- `layouts/projects/projects.html` – card grid + tag filter buttons
- `layouts/projects/single.html`   – detail page
- `assets/css/custom.css`          – styling for cards, filters, CV page
- `archetypes/projects.md`         – template used by `hugo new`
