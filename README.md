# craftsman

The `craftsman` command-line tool for Project Control: it publishes a game's
declaration, builds release candidates and runs the in-Roblox tests. This
repository holds only the released binaries. The source is developed with the
`craftsman-control` framework.

## Install

With [Rokit](https://github.com/rojo-rbx/rokit), from your project's folder:

```sh
rokit add averyark/craftsman-cli@0.4.0 craftsman
```

The last argument names the command. Without it, Rokit names the command after
the repository, `craftsman-cli`. The same thing as a line in `rokit.toml`:

```toml
[tools]
craftsman = "averyark/craftsman-cli@0.4.0"
```

Then run `rokit install`. Builds exist for Windows x86_64, Linux x86_64 and
aarch64, and macOS x86_64 and aarch64.

`craftsman release` also needs Rojo, so a project's `rokit.toml` should list
`rojo` too.

## Framework version

The CLI does not carry the framework. It loads `craftsman-control` from the
project's own `Packages/`, found from the working directory upward (the nearest
folder holding `wally.toml`).

| CLI | Framework |
|---|---|
| 0.1.x, 0.2.x, 0.3.x, 0.4.x | `>=0.1.0 <0.2.0` |

`craftsman --version` prints the CLI's version, the range it accepts and the
framework it finds. A framework outside the range is refused, naming both
versions and exiting 2.

## Commands

| Command | What it does |
|---|---|
| `craftsman publish` | Checks the declaration's manifest with Project Control and builds a release. `--activate` changes a place group's live declarations (see below) |
| `craftsman release <place.json>` | Builds a release candidate place file and its report |
| `craftsman test-roblox` | Builds this checkout and runs `verify/` against it in Roblox |
| `craftsman login` | Signs this machine in to Project Control with GitHub |
| `craftsman logout` | Signs this machine out and forgets its session |
| `craftsman whoami` | Says who this machine is signed in as, until when, and in which projects |
| `craftsman store plan` | Prints what applying the declared store catalogue would change on Roblox. Applying is done in the console's Store tab |
| `craftsman store import --universe <id>` | Writes declarations for what Roblox already sells and no key is bound to (`store/Imported.luau`); `--bind` also binds them |
| `craftsman summary [<dir>]` | Prints the release reports in `<dir>` (default `dist`) as Markdown |
| `craftsman skills [--check]` | Mirrors `.agents/skills` into `.claude/skills`, or checks that it matches |
| `craftsman --version` | Prints the versions described above |
| `craftsman help [<command>]` | Lists every command, or one command's options and examples (also `<command> --help`) |

An option a command does not have is refused before anything is sent, naming
the nearest one.

## Activating declarations

`--group` only chooses which place group a build is for. It never changes that
group's live flags, operations or permissions. Activating is its own option:

```sh
craftsman publish --group Development --activate --apply
```

Add `--no-build` to change declarations without a release. Without `--apply`,
`--activate` is a dry run that says what activating would change. A protected
place group waits for someone to confirm the activation with a passkey in the
console, and the CLI prints the link and waits.

**Changed in 0.4.0.** In 0.3.x, `--group G --apply` also activated in G. Now it
builds for G and activates nothing, and `--group G --no-build --apply` without
`--activate` exits 2. Update any script or runbook that relied on the old
meaning.

## Exit codes

| Code | Means |
|---|---|
| 0 | Done |
| 1 | A verdict against you: refused, a failed test, a build that was not handed over |
| 2 | The command line or configuration is wrong, and nothing was sent |
| 3 | Could not tell, including any 5xx answer from Project Control |

## Release workflow

Copy [`templates/release.yml`](templates/release.yml) to `.github/workflows/release.yml`.
A project with an `ember.toml` must commit `ember.lock`, and should install
Ember outside `Packages/` (see the
[Craftsman Kit README](https://github.com/averyark/craftsman-kit#alongside-wally)).

## Which console

The CLI talks to the hosted console, `https://operations.craftsman.systems/api`,
by default. `CONTROL_ENDPOINT`, in the environment or the project's `.env`,
points it elsewhere, such as a local stack at
`http://127.0.0.1:54321/functions/v1`. A project's `.env` may name only the
hosted console or this machine; any other host is refused with exit 2, so a
cloned repository cannot send your sign-in elsewhere.

## Signing in

`craftsman login` signs in with GitHub's device flow: it prints a code and a
link, and you enter the code on GitHub. First link your GitHub account on the
console's Account page, or Project Control will not know who you are. The
session is stored at `~/.craftsman/session`, one per `CONTROL_ENDPOINT`.
`craftsman whoami` shows it and `craftsman logout` ends it. The first line every
command prints names the console it is talking to, and whether that console is
hosted or local.

Activating in a protected place group does not happen straight away. The CLI
prints a link and waits while someone confirms the activation with a passkey in
the console. A build that follows starts only once the activation has applied.

## The game key

The game key, `CONTROL_SIGNING_SECRET`, belongs only in Roblox's secret store,
where a live server uses it. The CLI never reads it, and Project Control refuses
a publish signed with it, even beside a good session. CLI 0.2.0 and earlier sign
with it when there is no session, and are refused with:

> This console no longer accepts the game key from a terminal; update the CLI and run `craftsman login`.

If you see that, move to 0.4.0, delete `CONTROL_SIGNING_SECRET` from the
project's `.env`, and run `craftsman login`.

## Windows

On Windows the CLI asks PowerShell whether its output is a terminal. When it
cannot tell, it prints plain progress lines and says so once. `--live` forces
the moving progress line and `--plain` silences the note.

## License

MIT. See [LICENSE](LICENSE).
