## The problem

An observability stack is easy to assemble by hand and hard to hand to
someone else. The interesting question is not whether Grafana draws a
graph, but whether a single command rebuilds the whole thing, and whether
anything proves the telemetry arrived rather than that the pods started.

## Constraints

One machine. Terraform for every step, with nothing done by hand between
stages. And the validation has to check data, not liveness: a pod in
`Running` proves nothing about a trace reaching Tempo.

## What I built

Six Terraform modules across two stages, with explicit dependencies: the
kind cluster, MinIO as object storage, the observability stack, the
service mesh, the security layer, and a sample application. `make deploy`
runs the lot in about fifteen minutes.

Telemetry takes one path. Traces, metrics and logs all leave the
application over OTLP, a single OpenTelemetry Collector receives them,
and it routes each to its own backend: Prometheus, Tempo and Loki. One
pipeline to reason about instead of three agents with three
configurations.

Linkerd injects its sidecars automatically, encrypts pod-to-pod traffic
with mTLS, and carries retries and timeouts per route. The security layer
is Calico NetworkPolicies that deny by default and allow explicitly, RBAC
roles, and Sealed Secrets, so an encrypted secret can live in the
repository with everything else. On top of that sit recording rules for
an availability SLI, P99 latency and the error budget, which is what
turns a dashboard into something you can page on.

The part worth the most is the verification. Forty-odd checks run after
the deploy and assert behavior: that a trace written by the demo
application comes back out of Tempo, that a firing rule reaches
AlertManager, that a request RBAC should refuse is refused, and that a
connection a NetworkPolicy should drop is dropped. A stack that only
reports its pods healthy is a stack nobody has tested.
