# readtime

Estimate reading time for markdown and text documents: one file, several files, or a whole directory tree.

## Install

```bash
cargo install --git https://github.com/tatari-tv/readtime
```

Or grab a prebuilt binary (macOS arm64/x86_64, Linux amd64/arm64) from the [GitHub releases](https://github.com/tatari-tv/readtime/releases).

## Usage

```bash
readtime docs/design/my-doc.md          # one file
readtime README.md docs/                # several paths
readtime                                # current directory, recursively
readtime -w 250 docs/                   # words per minute (default 200)
readtime -e rst,adoc docs/              # only these extensions (replaces md,txt)
readtime --add-extensions rst docs/     # md,txt plus these
readtime -v docs/                       # print the paths, extensions and wpm used
```

A directory prints as a tree with minutes per file and directory, then a total:

```
plugin/
└── skills/                   2 min
    └── estimate/             2 min
        └── SKILL.md          2 min

Total: 2 min
```

- Each file rounds up to the whole minute; a directory sums its files' minutes.
- Markdown formatting, code blocks, inline code and link URLs are not counted.

## Config

`~/.config/readtime/readtime.yml`, then `./readtime.yml`, or `-c <file>`:

```yaml
extensions: [md, txt, rst]
```

Logs go to `~/.local/share/readtime/logs/readtime.log`.

## Claude Code plugin

`plugin/` is a Claude Code plugin with one skill, `readtime:estimate`. `tatari-skills` lists it as a `git-subdir` pointer pinned to a release tag of this repo. To change the skill, edit `plugin/` here and cut a release; the pin in `tatari-skills` moves to the new tag.
