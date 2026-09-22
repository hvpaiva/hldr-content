## The problem

A personal site whose deploy is a manual ritual contradicts a résumé
about delivery. The site had to be the argument, not a page describing
one.

## Constraints

One maintainer. No admin panel. No JavaScript required to read a page.
A content change must not be a deploy.

## What I built

A Rust workspace of three crates: the domain, the server and the CLI.

Content is not in the server. It lives in a second repository as YAML
manifests shaped like Kubernetes objects, `kind`, `metadata` and `spec`,
with the markdown of a page beside its manifest. The server resolves a
ref to one commit, downloads that commit and materializes it into SQLite.
Page types are content too: a blog is a manifest that declares its
fields, not a release of the server. Delete the database and nothing is
lost, because the next sync rebuilds it. A revision that fails to index
changes nothing, and the previous one stays served.

Administration is a CLI in the kubectl shape. `hldr get projects -o wide`,
`hldr describe project hldr`, `hldr explain project.metadata.status`,
`hldr apply -f`, `hldr diff`, `hldr patch`, `hldr edit`. Resource types
and their columns come from the server's discovery endpoint, so a page
type invented in content works in the CLI without a new binary. Writes
commit to the content repository the server reports it syncs from, so the
CLI cannot write where the site does not read.

The private API has no authentication of its own. It listens on a
separate socket that only the tailnet reaches, so the network is the
boundary instead of a token I would have to rotate. The public site
serves none of it.

A push to `main` that changes behavior is a release: checked, built,
deployed with Kamal, and tagged once production answers healthy. The tag
is the version and the history of production. `install.sh` checks the
SHA-256 of the binary and then the Sigstore attestation that ties it to
the workflow run that built it. A missing attestation fails the same as
a wrong one, because a checksum published beside a binary proves nothing
about who produced it.

The site counts what it serves without tracking anyone. No cookie, no
script, no address or user agent stored. A visitor is a hash of the day's
random salt, the client address and its user agent; when the day closes,
the salt and the hashes are deleted and only counts remain. Nobody can be
followed across days, including me.

Version skew follows Kubernetes' policy rather than matching numbers, and
the policy is a test. Every type the API serves or reads is rendered as
JSON Schema and compared against a snapshot on each build, so a breaking
change fails until it ships as a breaking release.
