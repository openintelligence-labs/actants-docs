# actants — documentation

Source for the actants documentation site.

**Published at:** <https://openintelligence-labs.github.io/actants-docs/>
**Product repo:** <https://github.com/openintelligence-labs/actants>

This repository contains documentation only. Bugs and feature requests for
actants itself belong in the
[product repo](https://github.com/openintelligence-labs/actants/issues).

## Local preview

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/mkdocs serve
```

Then open <http://127.0.0.1:8000>.

To check the site builds cleanly:

```bash
.venv/bin/mkdocs build --strict
```

`--strict` turns broken internal links into build failures, which is what CI
runs. A clean build means every cross-reference resolves.

## Layout

| Path | Purpose |
|---|---|
| `mkdocs.yml` | Site config, theme, and navigation |
| `docs/` | Page content (Markdown) |
| `docs/stylesheets/theme.css` | The site's entire visual layer |
| `.github/workflows/deploy.yml` | Builds and publishes to GitHub Pages |

This site is self-contained. It shares no configuration, theme, or build step
with any other project — change anything here without affecting anything else.

To adjust the look, edit the accent at the top of `docs/stylesheets/theme.css`;
everything else in the stylesheet derives from it.

## API reference

The API reference is generated from the actants source with mkdocstrings. CI
checks the product repo out to `src-checkout/` before building, so the reference
reflects the real code rather than a copy that drifts.

To build it locally, create the same checkout:

```bash
ln -s ../actants src-checkout
```

That path is gitignored.

## Contributing

Every page has an edit link in its top-right corner that opens a pull request
against this repository. Corrections are welcome — particularly anywhere the
documentation has drifted from the code.

MIT licensed.
