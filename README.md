# adrianschmidt.github.io

Serves the frozen legacy build of the Puzzle app at
<https://adrianschmidt.github.io/puzzle/>. The app itself moved to
<https://puzzlewise.github.io/> ([puzzlewise/puzzlewise.github.io](https://github.com/puzzlewise/puzzlewise.github.io)).
The old URL has to stay up: localStorage is per origin, so players' saves and
installed apps live here, and this build offers to move them to the new site.

`puzzle/` is a static build, committed as-is. There is no CI and nothing to
update; other project sites under `adrianschmidt.github.io/<repo>/` are
unaffected by this repo.

## The new site's address is load-bearing

The move handoff opens `https://puzzlewise.github.io/?receive-move` and only
accepts replies from exactly that origin. If the new site ever moves to a
custom domain, GitHub Pages redirects there, the origin no longer matches, and
every move from this build times out with "Couldn't reach the new site". Share
links keep redirecting fine, so the breakage is easy to miss. Rebuild this site
with the new origin in `VITE_MOVED_TO` if that ever happens, and keep the new
site's trust of `https://adrianschmidt.github.io` (`VITE_MOVED_FROM`).

## Rebuilding

From tag `legacy-frozen` of puzzlewise/puzzlewise.github.io, on the Node
version in its `.nvmrc`:

```sh
npm ci --ignore-scripts
VITE_BASE_PATH=/puzzle/ \
VITE_MOVED_TO=https://puzzlewise.github.io/ \
VITE_APP_VERSION=legacy \
VITE_UMAMI_WEBSITE_ID=<that repo's UMAMI_WEBSITE_ID secret> \
VITE_UMAMI_SCRIPT_URL=<its VITE_UMAMI_SCRIPT_URL variable> \
VITE_IMAGE_PROXY_URL=<its VITE_IMAGE_PROXY_URL variable> \
npm run build -- --outDir <this repo>/puzzle --emptyOutDir
```
