# Working in this repository

This is the Mintlify source of https://docs.exmergo.com. Read `README.md` first.

- Never edit a product folder (`dex/`). It is generated in that product's own
  repository and replaced on its next release; change the source there.
- Site-wide settings live in `docs.json`. A product's menu and redirects live
  in its own folder and are read with `$ref`.
- Pages are MDX with `title` and `description` frontmatter. Link by site route
  (`/dex/connectors/bigquery`), not by file.
- Prose is plain and em-dash free.
- Before proposing a change, run `npx --yes mint@4.2.994 validate` and
  `npx --yes mint@4.2.994 broken-links --check-anchors --check-redirects`.
