# The craftsman CLI

Installed per project with Rokit (`craftsman = "averyark/craftsman-cli@0.10.2"`).
It loads the framework from the project's own `CraftsmanPackages/`, so
`craftsman install` must have run. `craftsman help <command>` lists a command's
options.

| Command | Use it to |
|---|---|
| `craftsman publish` | Check the declaration's manifest with Project Control. A preview: exits 0 even when it lists problems, so read it |
| `craftsman publish --apply` | Build a release for every place group on GitHub, from what is **pushed**, then test it in Roblox. Activates nothing |
| `craftsman publish --group G --activate --no-build --apply` | Change G's live declarations now, without a release. A protected group waits for a passkey in the browser |
| `craftsman project [--check]` | Regenerate `default.project.json` / `release.project.json` from `src/Features`, and `Definitions/Generated.luau` from `:Declare` calls |
| `craftsman serve` | `craftsman project` on every change, plus `rojo serve` |
| `craftsman install [--frozen]` | Install exactly the packages `craftsman.lock` names into `CraftsmanPackages/`, and write `Settings.luau` from `craftsman.toml` |
| `craftsman update [kit[@0.10.1]]` | Move the packages, one or all, and rewrite `craftsman.lock`. Ask first: it changes what the game runs |
| `craftsman doctor` | Find the mistakes that fail silently, and say how to fix each. Reads only |
| `craftsman check` | Type-check and lint with luau-lsp, against `check-baseline.json`. Warns on raw `rbxassetid://` (`--explain RawAssetId`) |
| `craftsman release [<role>]` | Build a release candidate locally for a `[places.<role>]` in `craftsman.toml`. Sends nothing |
| `craftsman test-roblox [<role>]` | Run the `verify/` Jest specs on a real Roblox server against this checkout. No commit, no release |
| `craftsman asset add [paths…]` | Add animations, audio, images (png/jpg/bmp/tga ≤ 8000², recorded as the image id under `Assets.Images`; needs a download key) and videos: copies them into `assets/`, uploads once per owner (or saves drafts, the default), writes `src/Control/Assets.luau`. Also `replace <name>`, `import <id>`, `rollback <name>`, `push`, `list` |
| `craftsman asset status` | CI gate: exit 1 on drafts, changed files, anything not yet approved, a key renamed by hand and not yet pushed, a stale module. `asset push` applies such a rename on Roblox |
| `craftsman store <add\|edit\|import\|rollback\|push\|list\|status>` | Passes, products (and their `.gift` products) and subscriptions in every universe; see [store](store.md) |
| `craftsman asset` / `craftsman store` | No action, in a terminal: a browser of the index (kinds, entries, actions such as Copy path and Rename, which can rewrite references under `src/`) |
| `craftsman init --place <id>` | Install the packages and write the files a feature-routed game needs. Never overwrites |
| `craftsman feature <Name> "<description>"` | Add a feature's folder, its `Definitions.luau` and its entry in `Includes` |
| `craftsman login` / `whoami` / `logout` | The GitHub session the CLI speaks as |
| `craftsman skills [--check]` | Mirror `.agents/skills` into `.claude/skills` |

## Exit codes

| Code | Meaning | Do |
|---|---|---|
| 0 | Done, or a preview | Read the output anyway |
| 1 | A verdict against the change: refused, failed build, failed tests | Fix what it names. Re-running unchanged gets the same answer |
| 2 | The command line or configuration is wrong; nothing was sent | Fix the option, path or version |
| 3 | Could not tell: unreachable, a 5xx, no verdict | Not about the change; try later |

## Working rules for an agent

- **Preview before `--apply`, and ask before `--apply`, `--activate`, a commit
  or a push.** `--apply` builds from the pushed branch, so uncommitted work is
  not in it.
- `craftsman login` waits on GitHub's device flow: ask the person to run it
  (`! craftsman login` in Claude Code). `craftsman whoami` shows whether this
  machine is signed in and to which projects.
- After editing any `:Declare` call or a feature folder, run
  `craftsman project` and include the regenerated files in the change.
- `asset` and `store` take `--yes`, `--dry-run` and `--plain`. Run them with
  `--dry-run` first, and ask before anything that uploads or writes to Roblox:
  uploads spend quota, a video costs 2,000 Robux, and passes and products are
  never deleted. Never write an asset or catalogue id into code; use `Assets`.
- A local console is reached by `CONTROL_ENDPOINT`; the hosted console is the
  default. The CLI never reads the game key, and refuses a `.env` naming another
  host.
- `lune run <command>` is the same CLI from source, inside the
  `craftsman-control` repository only.
