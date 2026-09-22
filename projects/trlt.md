## The problem

Translating a paragraph from the terminal is a one-line job, and every
tool that does it sends the paragraph to somebody else's server. Some
text should not leave the machine, and that should be a choice you make
per invocation rather than a tool you stop using.

## Constraints

The same interface whatever answers, so switching providers is a flag and
not a rewrite of your muscle memory. A local model has to be a first
class provider rather than a degraded fallback. Text has to arrive as an
argument, a file or a pipe, because that is how the terminal works.

## What I built

A Rust CLI with OpenAI, Anthropic and Ollama behind one provider
interface. The source language is detected unless you name it. Defaults
for the target language, the provider and the model live in a config file
written on first run, so the common call is `trlt translate "..." --to
en`. Shell completions ship for bash, zsh, fish and PowerShell, and
`--copy` puts the result on the clipboard when you ask for it.

It began in November 2024 as a script that called OpenAI and did one
thing. The rewrite in March 2026 was mostly not about translation: it
added the apparatus a crate needs to be trusted by someone who is not me.
CI and coverage, CodeQL scanning, Dependabot, `cargo-deny` over the
license and advisory databases, conventional commits enforced by a
commit-msg hook, and a release that publishes itself from a tag.

That second part is the reason it is on this page. Turning a working
script into something with a supply chain you can inspect is a different
skill from making it work, and it is the one that matters once anyone
else runs it.
