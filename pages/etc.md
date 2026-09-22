# etc

Everything that is not a project. Exercises, tools with one user, and
repositories that found an audience I never asked for.

## Advent of Code

The same puzzles, a new language most years. What survives is never the
solutions. It is the runner: before solving anything I build the thing
that fetches the input, scaffolds the day and times the answer, which
teaches me more about the language than the puzzles do.

`advent-of-code-2021` and `advent-of-code-2022` are Kotlin, and
`advent-of-code-kotlin-template` is what both were cut from.

`aoc-in-go` came first in Go, and then `goaoc`, `goaoc-cli` and
`goaoc-template`, which are the runner it turned into.

`aocr` and `aoc_rust` are December 2024, when I was learning Rust.

`aoc-rb` is the current one. Its runner writes the day's file, downloads
and caches the input, runs the example before the real one, and CI
rewrites the star count in the README on every push.

## Ruby

I write more Ruby than I have any professional reason to.

`exercism-rb` is a CLI called `xrb` that takes about four steps out of
the Exercism workflow: it remembers which exercise you are on, runs
everything from that directory, opens the editor, starts IRB with the
solution already loaded, runs the tests, and submits. `exercism-ruby` is
the solutions themselves.

## Reimplementing things to learn them

`rlox` and `compiler` are both Lox, from Crafting Interpreters, the
second through CodeCrafters. `hecto` is a text editor in Rust following
Philipp Flenker's series, which is an excuse to spend time in raw
terminal mode and in the borrow checker. `codecrafters-shell-rust` is a
shell. `greenlight` is the API from Let's Go Further, in Go.

## Tools with one user

`bones` provisions every package manager on a fresh macOS machine from a
single `packages.toml`, because dotfiles restore your configuration and
nothing restores the software underneath it. `craftty` is a multi-crate
terminal toolkit for Rust, early and honest about being early.
`translate-cli` is where `trlt` started, in Go.

## The desktop

`nvim` is the current Neovim configuration and `nvim-old` the one before
it, with `dotfiles` holding all of it under chezmoi for Arch and macOS.
`omuntu` is Omarchy ported to Ubuntu 24.04, which is mostly the work of
finding every place an Arch assumption is buried. `all-hallows-eve.nvim`
is a colorscheme taken from the old TextMate theme, and one of the
palettes this page can be read in.

## What other people found

`clean-architecture-nestjs` has 372 stars and 51 forks. Its README is
still the NestJS boilerplate, six years on, so every one of those people
read the directory layout and nothing else. `aspdotnet-vuejs` has
twelve. `schedule-system` is CQRS, event sourcing and DDD with Axon in
Kotlin, and `techbank` is the same idea in Java over Kafka. `workflow`
is a small functional workflow engine for Java.

## Before that

Seven Haskell repositories in one month of 2022, which is what a
functional programming course looks like from outside. Two Kotlin
libraries, `compare-with` and `guard4j`. And `FluentDDD`,
`FluentFormatter` and `DijkstraAlgorithm` in C# from 2019, which is as
far back as any of this goes.
