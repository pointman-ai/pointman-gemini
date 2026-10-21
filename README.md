# gemini

A Pointman extension that puts Gemini CLI, Google's coding agent, on your machines. It's a client: nothing
runs beside it.
Its manifest says how a node installs it, signs it in and talks to it (`[client]`, v2).

Nodes don't run agents on Gemini CLI yet: it installs, but spawning a `gemini` agent is refused, with why. Agents run on Claude Code (`claude`) and Codex CLI (`codex`) for now.

```
pointman add gemini
```

## How it works

- **Installing:** `pointman add gemini` installs npm's newest Gemini CLI on every node of the workspace that
  hasn't got it, and on a node that joins later. `--nodes <machine>` adds it there only. It installs with the
  node's own Node.js (pinned and checked), never the machine's npm, into the node's own folder, so it needs no
  root and never clashes with a Homebrew install. Nothing else is needed on the machine.
- **Yours stays yours:** a Gemini CLI you installed yourself (Homebrew, npm, nix) counts as installed and
  comes first. `pointman add` leaves it as it is, `pointman upgrade` doesn't touch it, and `pointman remove`
  never takes it away.
- **Signing in:** sign Gemini CLI in on each machine as it asks. `pointman health` can't tell whether it's
  signed in: Gemini CLI doesn't say.

## Upgrade, remove, versions

- `pointman upgrade` moves the copies `pointman add` installed to their newest.
- `pointman remove gemini` takes the node's own copy away, and nothing else.
- `pointman list` shows Gemini CLI on each machine, `added` (by `pointman add`) or `yours`, and
  `pointman health` its version and the newest there is.
- It takes no version: a node always installs the newest. This extension's own version only marks changes
  to its manifest.

## Commands and settings

None. Gemini CLI keeps its own settings (`~/.gemini/settings.json`), as it does outside Pointman.

## The manifest

```toml
[client]
npm = "@google/gemini-cli"                  # where its newest version is said
binaries = ["gemini"]
protocol = "acp"
api_keys = ["GEMINI_API_KEY", "GOOGLE_API_KEY", "GOOGLE_GENAI_USE_VERTEXAI"]   # never passed to a run

[client.install]
from = "npm"                                # the node's own Node.js, never the machine's npm
package = "@google/gemini-cli"

[client.sign_in]
how = "gemini"
```

`[client]` v2 (Pointman release 20) says what a node needs: its programs, how its runs talk (`protocol`), how a
node installs it, how it signs in, how MCP servers reach it, and its vendor's API keys, which never reach a run.
Gemini CLI is built into the node as code, and this manifest has to say what that code does: a test in Pointman
checks they agree. A client still stands alone: no `[run]`, behaviour, files, links, rules or settings. The
format is in Pointman's docs/dev/extensions.md.

`protocol = "acp"`: nodes run Gemini CLI once their ACP host lands. Until then it installs, and a
`gemini` agent is refused.

## Releasing

Bump `version` in extension.toml, commit, and make a GitHub release (not a draft or prerelease) tagged
`v<version>`. There's nothing to build, so it has no assets. The marketplace lists it within 30 minutes.

## Licence

MIT. See `LICENSE`.
