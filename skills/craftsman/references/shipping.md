# Shipping: CLI, tests, changelogs, settings

`craftsman help <command>` lists a command's options, and every command takes
`--help`. An unknown option exits 2.

## Reading state, with nothing changed

| Command | Tells you |
|---|---|
| `craftsman whoami` | Who is signed in, until when, and on which projects |
| `craftsman status [--universe <id>] [--json]` | For each place group: manifest against this checkout, last deploy, features off, fallbacks and held values. One read |
| `craftsman doctor [--offline] [--json]` | Toolchain, Framework, Packages, Settings, Shared packages, Project file, Generated declarations, Stores, Places and Session, each with its fix |
| `craftsman check [<path>…] [--json]` | Types and lint against `check-baseline.json`. Exit 1 only when over the baseline. `--explain <rule>` says why |
| `craftsman publish` | A preview. Exits 0 even with problems, so read it |
| `craftsman summary [<dir>]` | Release reports as Markdown |

Prefer `status` and `doctor` over guessing why the console can't see something.
`--json` is for you to parse, and the plain output is for people.

## Publishing

- `craftsman publish --apply` builds what is **pushed**, from `HEAD`. Uncommitted
  work isn't in it. It activates nothing.
- Starting the build uses the person's own GitHub login: `GH_TOKEN`, then
  `GITHUB_TOKEN`, `gh auth token`, then Git's stored login. An SSH-only machine
  needs `gh auth login`. `craftsman login` is separate, and uses a passkey.
- Useful options: `--group G` to scope a build, `--no-test`, `--no-watch`,
  `--watch` to attach to the newest build of this commit (add `--test` to test it).
- `--group G --activate --no-build --apply` changes G's declarations with no
  release. A protected group waits for a passkey.

| Result | Means |
|---|---|
| Release candidate | Built and listed. A person deploys it |
| Needs a Studio place | No Studio version could be fetched. A person uploads one on the release |
| Built, and not handed over | The run is green, but Project Control recorded nothing |
| The build failed | Read the run's log |

Exit 1 is a verdict about the change, so re-running unchanged gets the same
answer. Exit 3 is not about the change, so try again later.

## In-Roblox tests

`craftsman test-roblox [<role>]` runs specs on a real Roblox server against this
checkout, with no commit or release.

- **Default:** uploads the owned code as a private test model, and saves no place
  version. Needs `asset:read` and `asset:write`, which Roblox grants only on a
  group; a user-owned universe works with its owner's own key.
- **`--save-place-version`:** only on an open, non-default group's place, with
  `craftsman.releases.deploy.open`.
- **Exits:** 0 passed, 1 failed or refused, 2 usage, 3 no verdict. A moderation
  hold is no verdict, not a failure.
- **Cost:** one Luau Execution task, from a shared budget of five a minute.

**A game has no spec runner by default.** `craftsman init` doesn't write one, and
no package ships it. `test-roblox` fails with *The model has no
ServerScriptService/Craftsman/Verify* until the game has it. A publish's test then
falls back to a baseline check that stores and entities open, and a protected
group needs a **passed** verdict to deploy.

To add specs, the game needs:

- A ModuleScript at `ServerScriptService.Craftsman.Verify`, with
  `Run({ Scope, CanCompile, CompileProblem })` returning `{ ok, … }`.
- `ReplicatedStorage.CraftsmanDevPackages` in what the place owns, because Jest
  resolves from it.
- Spec files named `*.spec`, with `testMatch = { "**/*.spec" }` in the Jest config.
- **Every write through a run-scoped helper**, because this is the real universe.

`verify/` in the `craftsman-control` repository is a working example, though its
paths are that repository's. Ask before adding this to a game.

## Changelogs

If the game keeps them, write `changelogs/<yyyy-mm-dd>-<topic>/CHANGELOG.md`
(lowercase, hyphenated, at most 80 characters) and commit it **with the change it
describes**. The release sends the ones the range added, and the console shows
them on the release.

A file that is empty or over 64 KB, or in a badly named folder, is skipped with a
warning, and never fails a build. Editing an older changelog attaches it to
nothing. No deploy reads one, so it is prose and never a gate.

## craftsman.toml

| Section | Holds |
|---|---|
| `[craftsman]` | `version` |
| `[console]` | `endpoint`, only the hosted console or this machine |
| `[features]`, `[[features.routes]]` | `folder`; `name`, `to` or `into` |
| `[assets]` | `folder`, `module` |
| `[places.<role>]` | `universe`, `place`, `source`, `baseline`, `overlay`, `definitions`, `owned` |

- Edit it as text, and keep its comments.
- `CraftsmanPackages/Settings.luau` is generated from it and **ships to every
  client**, so a secret must never go in it.
- A feature folder's children route by exact name: `Shared`, `Server`, `Client`,
  `Controller`, `Handler`, `Network`, `Definitions`, `Types`, `Lookup`.
- `default.project.json` and `release.project.json` are generated and gitignored.
  Run `craftsman project` rather than editing them.
- `craftsman serve` is `project` on every change, plus `rojo serve`.

## Assets and the catalogue

`asset` and `store` take `--dry-run`, `--yes` and `--plain`. With `--yes`, a
question with no default refuses and names its flag, so pass the flag.

- `asset replace` always makes a new id.
- Downloaded audio is always OGG.
- A video costs 2,000 Robux.
- A pass or product is never deleted.
- Ids reach a game only through a release, and the index in the repository is the
  source of truth.

`store` fields: `--kind`, `--name`, `--key`, `--description`, `--price`,
`--for-sale`, `--not-for-sale`, `--icon`, `--on-refund`, `--gift`, `--no-gift`.

## Leave alone

`craftsman package` is for Craftsman's own repositories. Never use it in a game,
and never edit `CraftsmanPackages/`.
