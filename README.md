# craftsman

The `craftsman` command-line tool for Project Control: it publishes a game's
declaration, builds release candidates and runs the in-Roblox tests. This
repository holds only the released binaries. The source is developed with the
`craftsman-control` framework.

## Install

With [Rokit](https://github.com/rojo-rbx/rokit), from your project's folder:

```sh
rokit add averyark/craftsman-cli craftsman
```

The last argument names the command. Without it, Rokit names the command after
the repository, `craftsman-cli`. The same thing as a line in `rokit.toml`:

```toml
[tools]
craftsman = "averyark/craftsman-cli@0.1.0"
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
| 0.1.x | `>=0.1.0 <0.2.0` |

`craftsman --version` prints the CLI's version, the range it accepts and the
framework it finds. A framework outside the range is refused, naming both
versions.

## Commands

| Command | What it does |
|---|---|
| `craftsman publish` | Checks the declaration's manifest with Project Control and builds a release |
| `craftsman release <place.json>` | Builds a release candidate place file and its report |
| `craftsman test-roblox` | Builds this checkout and runs `verify/` against it in Roblox |
| `craftsman summary [<dir>]` | Prints the release reports in `<dir>` (default `dist`) as Markdown |
| `craftsman skills [--check]` | Mirrors `.agents/skills` into `.claude/skills`, or checks that it matches |
| `craftsman --version` | Prints the versions described above |
| `craftsman help` | Lists every command with its options |

`craftsman login`, `logout` and `whoami` are **not available yet**. Signing in
with GitHub arrives in a later version. Until then, commands that reach Project
Control sign with `CONTROL_SIGNING_SECRET`, read from the environment or from a
`.env` file in the project.

## Windows

The CLI decides whether its output is a terminal by running `sh -c "test -t 1"`.
On Windows, put Git's `sh` on `PATH` (Git for Windows includes it). Without it,
the CLI treats every console as redirected and prints plain progress lines.

## License

MIT. See [LICENSE](LICENSE).
