# gemini

A Pointman extension that puts Gemini CLI, Google's coding agent, on your machines. It's a client: nothing
runs beside it. The extension is only the name of npm's package, `@google/gemini-cli`, and each node installs
it.

Nodes don't run agents on Gemini CLI yet: it installs, but spawning a `gemini` agent is refused, with why. Agents run on Claude Code (`claude`) and Codex CLI (`codex`) for now.

```
pointman add gemini
```

## How it works

- **Installing:** `pointman add gemini` installs npm's newest Gemini CLI on every node of the workspace that
  hasn't got it, and on a node that joins later. `--nodes <machine>` adds it there only. It goes in the node's
  own folder, never npm's global one, so it needs no root and never clashes with a Homebrew install. The
  machine needs npm (Node.js).
- **Yours stays yours:** a Gemini CLI you installed yourself (Homebrew, npm, nix) counts as installed and
  comes first. `pointman add` leaves it as it is, `pointman upgrade` doesn't touch it, and `pointman remove`
  never takes it away.
- **Signing in:** sign Gemini CLI in on each machine as it asks. `pointman health` can't tell whether it's
  signed in: Gemini CLI doesn't say.

## Upgrade, remove, versions

- `pointman upgrade` moves the copies `pointman add` installed to npm's newest.
- `pointman remove gemini` takes the node's own copy away, and nothing else.
- `pointman list` shows Gemini CLI on each machine, `added` (by `pointman add`) or `yours`, and
  `pointman health` its version and the newest there is.
- It takes no version: a node always installs npm's newest. This extension's own version only marks changes
  to its manifest.

## Commands and settings

None. Gemini CLI keeps its own settings (`~/.gemini/settings.json`), as it does outside Pointman.

## The manifest

```toml
[client]
npm = "@google/gemini-cli"
```

That's all a client is: npm's package. A client has no `[run]`, behaviour, files, links, rules or settings.
Each node installs from its own table of packages, which has to agree with this one.

## Releasing

Bump `version` in extension.toml, commit, and make a GitHub release (not a draft or prerelease) tagged
`v<version>`. There's nothing to build, so it has no assets. The marketplace lists it within 30 minutes.

## Licence

MIT. See `LICENSE`.
