# hldr-content

Content and configuration of [hvpaiva.dev](https://hvpaiva.dev). The
server in [hldr](https://github.com/hvpaiva/hldr) pulls this repository
and serves one commit of it at a time. Nothing else stores the site: its
database is rebuilt from here.

## Layout

```
site.yaml                 Site: title, default theme, free values
profile.yaml              Profile: identity and links
nav.yaml                  Nav: the file tree and the statusline
kinds/<name>.yaml         PageKind: a page type, its names and fields
collections/<name>.yaml   Collection: the home of one page type
pages/<name>.yaml         Page: a page outside any collection
pages/<name>.md           its markdown, when the manifest names it
<collection>/<name>.yaml  a page of that collection's type, such as a Project
<collection>/<name>.md    its markdown
themes/<name>.yaml        Theme: one color scheme per file
```

Every manifest declares `kind`, `metadata` and `spec`, and must sit where
that kind lives; `metadata.name` repeats the file name. Singletons (Site,
Profile, Nav) have no `metadata`. Names match `[a-z0-9][a-z0-9-]*`. Any
other file, such as this README, is not content and is ignored, except a
markdown file beside page manifests, which one of them must name.

A page's `metadata` describes it, and is what its frontmatter shows:
`title`, `description` and the fields of its type. Its `spec` says how it
is built, the same for every type:

```yaml
kind: Project
metadata:
  name: hldr
  title: hldr
  tagline: Site and CLI for hvpaiva.dev.
  status: active
spec:
  frontmatter: open       # hidden (default), open, or folded
  content:
    file: hldr.md         # or inline: the markdown itself
```

`index` is the page at `/`, and `not-found` the body of every 404.

## Page types

A type is a PageKind in `kinds/` and a Collection in `collections/`, the
way a Kubernetes CRD is. The PageKind names the type and declares its
fields (`string`, `int`, `bool`, `enum`, `list`, `url`, `links`); the
Collection gives it a home: a listing at `/<name>`, and its pages in
`<name>/`, served at `/<name>/<page>`. A collection with `enabled: false`
answers 404 and shows `off` in the tree. A new type needs no release of
hldr: the site, `hldr get`, `hldr explain` and `hldr validate` learn it
from here.

## Templates

Markdown, titles, descriptions and nav labels take a closed set of
directives:

```
{{ profile.name }}  {{ site.title }}  {{ site.values.about }}  {{ page.tagline }}
{{ link /about about.md }}                              a link without brackets
{{ banner site.values.banner }}                         ASCII art, alone on its line
{{ collection projects where=highlight limit=3 }}       listing rows, alone on its line
{{ links }}  {{ links source }}                         the profile's links
{{ clone page.links.repo }}                             git clone, for a safe URL
```

A value used by more than one page lives in `site.values`; a page reads
its own metadata and never another page's. Nothing expands inside code,
and `\{{` is a literal `{{`.

## Fields

`hldr explain` documents each kind and field from the schema the server
publishes, page types included. Fields marked `-required-` must be in the
file; the others may be left out.

```
hldr explain project
hldr explain project.metadata.links
hldr explain page.spec.frontmatter
hldr explain theme.spec.colors
```

## Changing content

`hldr edit`, `hldr apply` and the other writing commands commit here and
have the site sync that commit; a page's markdown travels with its
manifest, and `hldr edit page about --content` opens it. A plain
`git push` works too: the server polls every five minutes, and
`hldr sync` fetches at once.

## Checking

```
hldr validate -f .
```

It runs the parser and the checks across files the server indexes with,
offline. CI runs it on every push and pull request with the latest hldr
release, which is what production runs.

## Preview

From a checkout of hldr next to this one:

```
HLDR_CONTENT_DIR=../hldr-content HLDR_CONTENT_POLL=2s cargo run -p hldr-server
```

The site is then at <http://127.0.0.1:8080>, and every save shows within
two seconds. A file that does not validate leaves the last good state
served, with the error on `/health`.
