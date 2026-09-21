# hldr-content

Content and configuration of [hvpaiva.dev](https://hvpaiva.dev). The
server in [hldr](https://github.com/hvpaiva/hldr) pulls this repository
and serves one commit of it at a time. Nothing else stores the site: its
database is rebuilt from here.

## Layout

```
site.yaml            Site: title, default theme, banner, descriptions, blog
profile.md           Profile: identity and links above, the about page below
projects/<name>.md   Project: one per file, named by the file
themes/<name>.yaml   Theme: one color scheme per file, named by the file
```

Every file declares its `kind` and must sit where that kind lives. Names
match `[a-z0-9][a-z0-9-]*`. Markdown files carry their fields in a YAML
frontmatter between `---` lines. Any other file, such as this README, is
not content and is ignored.

## Fields

`hldr explain` documents each kind and field from the schema the server
publishes. Fields marked `-required-` must be in the file; the others may
be left out.

```
hldr explain project
hldr explain project.spec.links
hldr explain theme.spec.colors
```

## Changing content

`hldr edit`, `hldr apply` and the other writing commands commit here and
have the site sync that commit. A plain `git push` works too: the server
polls every five minutes, and `hldr sync` fetches at once.

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
