## The problem

Platform engineering is easy to read about and hard to practise. Changing
a platform means having one, and wiring Backstage, Argo CD, Crossplane and
Kyverno together until they agree with each other is the part nobody hands
you.

## Constraints

One laptop. One kind node. No cloud account, so an emulator stands in for
AWS. GitHub is the only outside service, because Argo CD has to read what
to deploy from somewhere real. And the lab has to say where it is lying: a
laboratory that hides its own stage set teaches the wrong thing.

## What I built

A cluster that comes up with `just up`, and two identities to see it
through: `dev`, a developer in `team-a`, and `platform-admin`.

From the developer's side there is one service, `hello`, whose repository
holds its code and a short declaration of what it needs from the platform:
name, team, port, size, whether it is public, and a bucket. A push to
`staging` builds an image and deploys it. A pull request from `staging` to
`main` promotes that same image to production once the platform's rules
have checked it. The developer never writes a Kubernetes manifest, never
writes a Dockerfile, and never creates the bucket.

From the platform's side, Argo CD installs and upgrades everything from
Git, itself included. One chart turns what a service declares into
Deployments, Services and routes, with the platform's defaults for probes,
resources and security context. Crossplane serves the platform's own APIs,
so a request for a bucket, a queue, a table or a cache becomes that thing
in the lab's cloud account.

Changing the platform has its own loop, which is the point of the lab.
`just render <api>` prints what a request would create, in a second and
without a cluster, checked against the schemas. `just diff <app>` shows
what applying a folder would change. `just local` hands one Argo CD
Application to your working copy, and `just gitops` hands it back.

The file I care most about is `docs/decisions.md`, which names every place
the lab trades realism for running on one machine and says what a company
would do instead. One node hides high availability, and hides that Argo CD
should deploy from outside the cluster it manages. One emulator with one
dummy key stands where a company would give each stage its own account,
with credentials the teams never see. Traefik replaced ingress-nginx
because ingress-nginx reached end of life in March 2026; the AWS emulator
changed for the same reason, and its replacement is pinned by digest
because it has a single maintainer and was created the day after its
predecessor's sunset. `setup.sh` offers to add you to the `docker` group
and tells you, before you answer, that membership there is equivalent to
root on the machine.

Backstage is the B and is not in yet. Kyverno is installed with no policy
of its own.
