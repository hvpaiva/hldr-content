## The problem

ble.sh suggests the rest of the most recent command that starts with what
you typed. A whole line is only ever right when you are repeating
yourself, and most of the time you are typing a line you have never typed
before. But a single word can be right in a line that is new: in
`kubectl -n staging get po`, `pods` comes from what followed `get` in
every other kubectl command you ran.

## Constraints

The answer has to arrive inside a pause in typing, and a keystroke has to
cancel it. It has to work with no language model at all, because most
machines will not have one. And a suggestion that is merely valid is
worse than no suggestion, because the grey text teaches you to trust it.

## What I built

Two halves, installed separately. A bash file sourced into the
interactive shell, and a Rust engine running as one background process
per shell, talking over a pair of FIFOs that ble.sh manages.

The shell half records and suggests. ble.sh's `PREEXEC` and `POSTEXEC`
hooks append the command, its directory and its timing to one
append-only file shared by every shell, which is how something run in one
terminal is suggested in another a moment later. The suggestion side
registers as the first auto-complete source, runs ble.sh's own completion
for the word under the cursor, tags each candidate as a file name or a
word, and polls for the engine's answer in four-millisecond slices so
that typing cancels the wait.

The engine decides. Completion says what is valid here: installed
commands, subcommands, flags, branches, pods, whatever the completion
scripts list. History says what you use, narrowed by this directory, this
git repository, the previous command and this session. When neither has
an opinion, a local base model served by Ollama picks among the valid
words, using log-probabilities rather than generated text. The model is
optional and off by default.

The rules are where the work is, because a word the completion offers is
valid, not likely. With nothing typed the valid words are every word, so
one is shown only when the history or the model points at it, and a file
name only when the history does. A guess no completion backs must clear a
likelihood bar, and is never shown when it names a file that is not
there: the `notes.txt` you read in another directory is not offered in
this one. Paths arrive one component at a time, because the path you
typed before is as often a sibling of the one you are typing as the path
itself.

augur prefers showing nothing to showing a guess it has no reason for.
