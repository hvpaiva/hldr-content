# about

{{ site.values.about }}

That arrow is nine years long and the first step was literal: ten months
drawing interfaces for the Ministry of the Environment, the national
rural environmental registry among them. Then full-stack in a software
factory, Java and Vue over Postgres on AWS, across dozens of small
projects, and after that tech lead on an urban mobility product built for
transport cooperatives.

Two and a half years at a bank came next, on fixed income, government
bonds and savings. I spent the first half refactoring legacy services and
the second leading six people through a migration off Java 7 and 8 into
Micronaut microservices, with Kafka and SQS between them.

Then platform engineering, which is the part I would point at first. For
three years I was on the platform team of a marketplace, responsible for
the secrets management service inside its internal PaaS: the thing every
engineer there goes through to get a credential. Encryption and rotation,
IAM and account provisioning across AWS and GCP, and keeping the two in
step. Datadog and Grafana for what it was doing, OpsGenie and an on-call
rotation for when it stopped. The automation and the internal CLIs around
it I wrote in Rust, which is how Rust stopped being a hobby. Since the
start of this year I do platform engineering at an ERP company.

So security is not a field I am moving into. It has been most of the work
for three years, because secrets management is application security with
the volume turned up: everything is credentials, blast radius, and who is
allowed to ask for what. What I am deliberately adding is the half that
happens before deployment, the checks that belong in the pipeline itself.
Most of what is on this site is that same instinct turned on my own
things. The installer checks who built the binary before running it. The
platform lab tells you that joining the `docker` group is equivalent to
root before you accept. The observability lab tests that RBAC actually
refuses.

## Stack

Rust by choice, and at work once it earned its place. Go, Python, Shell
and Terraform. Kotlin and Java for longer than any of them. Kubernetes
when the thing needs it and not before. Arch with Hyprland on the laptop,
NixOS on the machine that serves this page.

## Contact
{{ links }}
