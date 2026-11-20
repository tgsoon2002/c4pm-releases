# c4pm

**Architecture-first project tool: zoomable C4 map, task boards, simulations.**

c4pm puts your plan, your code and your tasks in one zoomable map. Move from the whole system down to a single
function in three views: Architecture, User flow (IO) and Storage. Link board tasks to the parts they touch, and play
step-by-step simulations of how a request moves through the system. c4pm reads your code, so the map also shows where
the code has drifted from the plan, with one snapshot per git commit. AI agents get code-query tools over MCP. One
binary, no server, no account; the map is one HTML page you open in a browser.

This repo holds the downloads (see Releases) and this page. The source is private.

## Install

macOS or Linux (Intel Mac, Apple Silicon Mac, Linux x64):

```sh
curl -fsSL https://github.com/tgsoon2002/c4pm-releases/releases/latest/download/install.sh | sh
```

Windows: coming in a later release.

The script downloads the binary for your machine, checks it against `SHA256SUMS`, and runs `c4pm install`. That
copies it to `~/.local/bin` and asks before it adds that folder to your
PATH. It never uses sudo. Open a new terminal, then run `c4pm --version`.

A pinned version:

```sh
curl -fsSL https://github.com/tgsoon2002/c4pm-releases/releases/latest/download/install.sh | C4PM_VERSION=0.9.1 sh
```

Extra arguments go to `c4pm install`, for example `... | sh -s -- --yes` (no questions) or `--no-path`.

Manual download: take the file for your system from the latest release (`c4pm-<version>-darwin-arm64`,
`-darwin-x64`, `-linux-x64`) and `SHA256SUMS`. In the download folder:

```sh
grep darwin-arm64 SHA256SUMS | shasum -a 256 -c      # use the name of your file; Linux: sha256sum -c
chmod +x ./c4pm-0.9.1-darwin-arm64
./c4pm-0.9.1-darwin-arm64 install
```

There is also a zip (`c4pm_<version>_<stamp>.zip`) that needs Node 20 or newer. Each release carries
`THIRD-PARTY-NOTICES.txt` and `LICENSE`.

Homebrew (macOS, Linux x64):

```sh
brew install tgsoon2002/c4pm/c4pm
```

## First 5 minutes

```sh
cd your-repo
c4pm init --repo .            # creates the .c4pm/ folder (works without git, safe to repeat)
c4pm view --repo . --open     # scans the code, writes c4pmView.html, opens it
```

`c4pm view` reads TypeScript by default. For other languages add `--lang`, for example
`--lang typescript,python`. Languages read: typescript (and JavaScript), python, rust, csharp, php, cpp, swift.
Pass the same `--lang` to every command.

For AI agents:

```sh
c4pm guide ai                                    # how an agent uses c4pm
c4pm guide ai --skill --out ~/.claude/skills/c4pm/SKILL.md      # a Claude Code skill file
```

To give an AI client the code-query tools (find, show, outline, deps, calls, callers) over MCP, add this to its
`.mcp.json`:

```json
{"mcpServers":{"c4pm":{"command":"c4pm","args":["mcp","--repo","."]}}}
```

Other guides: `c4pm guide c4` (the plan format), `c4pm guide board`. `c4pm --help` lists every command.

## What you will see

- Four zoom levels, C1 to C4: system context, containers, components, then files and functions.
- Three views of the same code: Architecture, IO and Storage.
- Plan vs code: planned parts, done parts, stubs, and code nobody planned (`c4pm check`).
- Boards for tasks, features, issues and bugs, kept in the repo next to the plan.
- Simulations: ordered steps or journeys that light up boxes and arrows on the map, with a sequence diagram.

## Limits and warnings

- No Linux arm64 binary (the parser library has no prebuilt file for it).
- The binaries are not signed. macOS blocks a binary downloaded with a browser. The install script, curl and Homebrew
  do not trigger this. If you downloaded in a browser, run `xattr -d com.apple.quarantine <file>`.
- Each binary is about 115 to 140 MB.
- `c4pm` reads only the languages listed above. Other files can be listed with `--lang files`, without functions.

## Licence

c4pm is source-available under FSL-1.1-MIT (Functional Source License with an MIT future licence): you may use it,
but not to build a competing commercial product, and each version becomes MIT after two years. The plan standard is
under CC BY 4.0. Copyright 2026 Kien Nguyen. See `LICENSE` in each release.

## Feedback

Found a bug or have an idea? Open an issue in this repo.
