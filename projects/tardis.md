## The problem

Scripts and task trackers want an exact datetime. People say "next Monday
at 09:00". Doing that conversion by hand is trivial once and tedious
every day, and there is nowhere in a pipe to do it.

## Constraints

It has to behave in a pipe, reading from an argument or from stdin and
writing one line. It has to be deterministic enough to test, which for
anything that reads the clock means the clock must be an input. And the
format you always use should not be a flag you always type.

## What I built

A Rust CLI, `td`, that parses relative and absolute expressions:
`tomorrow 15:00`, `in 2 hours`, `next Friday`, `yesterday`, `now`. Output
goes through strftime formats, with named presets in a TOML file so
`--format br` means what you decided it means. Time zones are converted
or forced with `--timezone`.

`--now` pins the reference instant. It exists so the test suite can ask
what "yesterday" meant on a given afternoon and get the same answer
forever, and it turns out to be useful in scripts for the same reason.

Configuration resolves in one order, command line over environment over
file, and the file is written on first run at the path the platform
expects: `$XDG_CONFIG_HOME` when it is set, `~/.config` on Linux,
Application Support on macOS, `%APPDATA%` on Windows.

Published on crates.io with documentation on docs.rs. `cargo install
tardis-cli --locked`, because the lockfile is what CI tested.
