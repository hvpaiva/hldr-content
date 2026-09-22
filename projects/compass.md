## The problem

Every AI coding tool is built so that you write less code. That is the
right goal when shipping is the point. It is the wrong goal when the
project is how you were going to learn the thing, because the tool takes
exactly the part you wanted.

## Constraints

Asking a model not to write code does not work; it has to be unable to.
The output has to survive the session, so a conversation that produced a
good decision is worth nothing if the decision lives in scrollback. And
the human has to stay the one who decides, which means the tool never
advances on its own.

## What I built

A Claude Code skill whose sub-agents cannot produce production code. It
is enforced twice, in the tools each sub-agent is granted and in its
system prompt, rather than requested once in an instruction the model may
weigh against being helpful.

Six phases, each with a gate the human opens. Frame settles mission,
scope and non-goals by interrogation, not by proposal. Research produces
dossiers with real citations. Architect takes an architecture the human
proposes and argues with it, listing the trade-offs it accepted without
saying so. Decide writes the result as an ADR in MADR format. Spec works
through edge cases and invariants until the behavior is written down.
Build decomposes the spec into work units with acceptance criteria, and
then supervises: a rubber duck that returns questions instead of answers,
an idiom check that names the anti-pattern without writing the fix, a
scope guard that compares recent changes against the spec and the
decisions, a test audit that finds what the human's tests do not cover,
and a review against the ADRs and the module boundaries.

Everything lands in `.compass/` as files in the repository, so the
artifacts outlive the session and the next one starts from them.

The inversion is the whole product. The human implements; the model
orients, researches, challenges and tracks drift. What it optimizes for
is not how fast the code appears but whether the person who ships it
understands it.
