---
name: estimate
description: Estimate how long a document takes to read, via the `readtime` CLI (`readtime PATH...`). Works on one file, several files, or a directory tree of markdown/text, with a per-file and per-directory breakdown. Use when asked how long a doc, design doc, README, spec or docs folder takes to read, whether a doc is too long, or to compare doc lengths, even if the user never says "readtime".
user-invocable: true
argument-hint: "[path ...]"
---

# readtime:estimate

## Run the `readtime` CLI

The answer is one command. Do not count words by hand or with `wc -w`: `readtime`
strips markdown formatting and code first, so its count is what a reader reads.

```bash
readtime docs/design/my-doc.md          # one file
readtime README.md docs/                # several paths, one block per path
readtime                                # current directory, recursively
readtime -w 250 docs/                   # reading speed in words per minute (default 200)
readtime -e rst,adoc docs/              # only these extensions (replaces md,txt)
readtime --add-extensions rst docs/     # md,txt plus these
readtime -v docs/                       # also print the paths, extensions and wpm used
```

Paste the CLI's output as the evidence. Do not paraphrase it.

## Reading the output

A directory prints as a tree with a minute count on every file and directory,
then a `Total:` line:

```
plugin/
└── skills/                   2 min
    └── estimate/             2 min
        └── SKILL.md          2 min

Total: 2 min
```

- Each file rounds **up** to the whole minute, so a 10-word file reads as `1 min`.
- A directory's minutes are the **sum of its files' rounded minutes**, not the
  rounded sum of words. Many small files therefore overstate the total.
- With several paths, each directory prints its own tree and `Total:`. Each file
  prints one line with no `Total:`. For a grand total, add them yourself.
- In markdown, fenced code blocks, inline code, link URLs and formatting marks are
  not counted. A code-heavy doc reads shorter than its size suggests.
- Only `md` and `txt` files count by default. Other files in a directory are skipped
  silently. If a tree looks too small, check the extensions with `-v`.

## When the output says `No matching files found`

`readtime` exits `0` even then. It prints `✗ No matching files found` when:

- the path does not exist, or
- the path holds no files with the selected extensions.

Run it again with `-v` to see which. It prints `Skipping <path>: <error>` for a path
it could not read. Never report `No matching files found` as "0 minutes".

## Config

`~/.config/readtime/readtime.yml`, then `./readtime.yml`, or `-c <file>`:

```yaml
extensions: [md, txt, rst]
```

Logs go to `~/.local/share/readtime/logs/readtime.log`.

## Not installed?

If `readtime` is not on `PATH`:

```bash
cargo install --git https://github.com/tatari-tv/readtime
```

Or download a prebuilt binary from the
[GitHub releases](https://github.com/tatari-tv/readtime/releases).
