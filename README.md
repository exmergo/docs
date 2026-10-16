# Exmergo documentation

The source of https://docs.exmergo.com. Mintlify deploys this repository's
`main` branch.

## Layout

| Path | Owner | What it is |
| --- | --- | --- |
| `docs.json` | this repository | Site settings: name, colours, navbar, footer, search, analytics, and the product switcher. |
| `index.mdx` | this repository | The Exmergo home page. |
| `skill.md` | this repository | Tells an agent that finds the site where each product's real skills come from. |
| `dex/` | [exmergo/dex](https://github.com/exmergo/dex) | The Dex section, served at `/dex`. Generated; never edit it here. |

Each product owns one folder and publishes it from its own repository: the
release workflow there builds the section and commits it here, replacing that
folder and nothing else. A product's menu and redirects ship inside its folder
(`dex/navigation.json`, `dex/redirects.json`), and `docs.json` reads them with
`$ref`, so a product release never edits this repository's own files.

To change a Dex page, change its source in exmergo/dex: `docs/`, `references/`,
`CHANGELOG.md` or the CLI help text. The next Dex release publishes it.

## Adding a product

1. The product's repository builds its pages into a folder named after it
   (`viz/`), with `navigation.json` holding its product entry and
   `redirects.json` its redirects, all inside that folder.
2. Its release workflow commits that folder here, the way exmergo/dex does.
3. Add `{ "$ref": "./viz/navigation.json" }` to `navigation.products` in
   `docs.json`. Redirects take one `$ref`; with a second product, merge the
   lists in a small step here rather than giving one product the whole key.

## Checks

Every pull request and every push to `main` runs Mintlify's `validate`,
`broken-links --check-anchors --check-redirects` and `a11y` over the whole site
(`.github/workflows/validate.yml`). A pull request that edits a product's folder
fails unless it carries the `product-sync` label, because those folders are
written by their product's release, not by hand.

## Previewing

```
npx --yes mint@4.2.994 dev
```

Then open http://localhost:3000. To preview unreleased Dex changes, build the
site from an exmergo/dex checkout instead:

```
uv run --project packages/dex-core python scripts/build_docs_site.py --site ../docs
cd build/docs-site && npx --yes mint@4.2.994 dev
```
