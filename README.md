# utkarshbahuguna.me

Built output. **Do not edit these files by hand** — every one of them is
generated, and the next publish overwrites the lot.

The source lives in [`u7k4rs6/Newspaper`](https://github.com/u7k4rs6/Newspaper),
which stays private. Only the compiled site is published here.

## Why the built output and not the source

The site is Next.js with `output: 'export'`, so it has a build step and cannot
be served from source. Building here instead would mean either making the
source public or storing a token with access to a private repo; publishing the
compiled output keeps the source private and needs no credentials.

## Republishing after a content change

From a clone of `Newspaper`, with this repo checked out alongside it:

```bash
# in Newspaper/
NEXT_PUBLIC_BASE_PATH='' \
NEXT_PUBLIC_SITE_URL='https://utkarshbahuguna.me' \
npm run build

# into this repo, keeping CNAME
rsync -a --delete --exclude '.git' --exclude 'CNAME' Newspaper/out/ u7k4rs6.github.io/
touch u7k4rs6.github.io/.nojekyll
```

Then commit and push. Pages serves this repo from `main` at the repository
root.

## Three things that must stay true

**`CNAME` must survive every publish.** It contains `utkarshbahuguna.me` and it
is what claims the domain. Deleting it un-claims the domain and the Pages custom
domain setup has to be done again. The `rsync` above excludes it deliberately.

**`.nojekyll` must exist.** Pages serves this repo through its legacy Jekyll
pipeline, and Jekyll ignores every directory whose name begins with an
underscore. Without this file the whole of `_next/` is dropped and the site
deploys with no CSS and no JavaScript.

**`NEXT_PUBLIC_BASE_PATH` must be empty.** The source was previously served
from `u7k4rs6.github.io/Newspaper`, where every absolute path needed a
`/Newspaper` prefix. Served from a domain root it needs none. Build with that
variable set and every asset resolves one directory too deep.
