# foxgame-web

Compiled output for every FoxGame title. **Public**; the source repositories are not.

Deployed by Cloudflare Pages to `foxgamedao.pages.dev`. Nothing here is written
by hand — it is produced by `fg release` from a title repository:

```bash
cd ../game-<slug>
pnpm build
pnpm exec fg release --dist dist --web ../foxgame-web
```

## Layout

```
index.html      catalogue, regenerated on every release
catalog.json    read at runtime, so a new title can appear in an old title's
                cross-promotion slot without rebuilding it
g/<slug>/       one title, including its build.json
```

## One origin, on purpose

Every title is served from this one origin under `/g/<slug>/`. Carry tokens live
in `localStorage`, which is partitioned per origin — titles on separate
subdomains could not see each other's, and every carry would have to be retyped.

Changing the origin therefore strands every wallet. Tokens are plain text and
each title has an export/import screen, so the migration costs a copy and a
paste rather than the data, but it is still not free.

## What must never land here

- **Sourcemaps.** They reconstruct private source from a public bundle.
  `.gitignore` blocks them and `fg release` withholds them.
- **Anything built from a modified tree.** `fg release` refuses: `build.json`
  would record a commit that does not contain the shipped code.
- **Platform bundles.** CrazyGames, Poki and itch builds are uploaded to those
  platforms, not published here.
