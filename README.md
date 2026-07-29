# TRAIN Protocol Documentation

Source for [docs.train.tech](https://docs.train.tech), the TRAIN Protocol documentation site, built with [Mintlify](https://mintlify.com). Navigation, theme, and site-wide settings live in `docs.json`; content pages are `.mdx` files under `protocol/`, `protocol-spec/`, `changelog/`, and `research/`.

### Development

Install the [Mintlify CLI](https://www.npmjs.com/package/mintlify) to preview documentation changes locally:

```
npm i -g mintlify
```

Run the following command at the root of this repo (where `docs.json` is):

```
mintlify dev
```

### Publishing Changes

Changes are deployed to production automatically after pushing to the default branch, via the Mintlify Github App.

#### Troubleshooting

- Mintlify dev isn't running - Run `mintlify install` to re-install dependencies.
- Page loads as a 404 - Make sure you are running in a folder with `docs.json`.
