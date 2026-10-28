# Contributing

This repo is a curated list. A contribution is usually a new resource in one
category, described by a YAML entry. Category pages and the browse site are
generated from that data, so you never edit them by hand.

## Prerequisites

Python 3. The generator and the validation script use only the standard
library, so there is nothing to install.

## Add or update a resource

1. Add an entry to `data/entries.yml`. The schema reference is
   `docs/data-schema.md`. A minimal entry looks like this:

   ```yaml
   - name: Example Tool
     category: tools
     summary: One or two sentences that explain what it is and why it matters.
     url: https://example.com/tool
     tags: [tools]
   ```

   For categories with a `table` layout, an entry also needs a `section` that
   matches a section name under that category.

2. Run `python3 scripts/generate.py` from the repo root. The script rewrites
   the category pages in `docs/categories/` and builds the browse site into
   `site/` (gitignored).
3. Commit `data/entries.yml` and the regenerated category pages together.

## Editing rules

- Keep `summary` to one or two sentences on a single line. The YAML parser
  supports only single-line values; it rejects `|` and `>` block scalars.
- Quote values that contain `: ` or start with a special character.
- Pick one category. Do not list the same resource under two categories.
- Keep `tags` as lowercase slugs. They power the browse site's tag filter.
- The generator validates the file and reports problems by entry number.

## Longer detail pages

If a resource needs setup notes, comparisons, screenshots, or other content
that does not fit a summary, create a page under `docs/entries/` from the
template in `docs/templates/entry-template.md`, then link to that page from
the entry's `url`.

## New categories

Adding a category slug adds a generated page directory and a row in the
`readme.md` category table. Propose it in an issue first.

## What CI checks

One workflow guards every PR that touches `data/`, `scripts/`, or
`docs/categories/`. It runs `python3 scripts/generate.py --check` and fails
when the committed pages differ from the generated output. This catches hand
edits and stale pages.

A second workflow deploys the browse site to GitHub Pages on every push to
`main`. Contributors do not deploy anything.

## Pull requests

Keep one resource, or one small fix, per pull request. Branch names like
`add-example-tool` or `fix-category-typo` work well. CI must show the
`Validate generated pages` check as green before review continues.
