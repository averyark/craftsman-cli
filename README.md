# craftsman

The `craftsman` command-line tool for Project Control: it publishes a game's
declaration, builds release candidates and runs the in-Roblox tests. This
repository holds only the released binaries. The source is developed with the
`craftsman-control` framework.

## Install

With [Rokit](https://github.com/rojo-rbx/rokit), from your project's folder:

```sh
rokit add averyark/craftsman-cli@0.7.0 craftsman
```

The last argument names the command. Without it, Rokit names the command after
the repository, `craftsman-cli`. The same thing as a line in `rokit.toml`:

```toml
[tools]
craftsman = "averyark/craftsman-cli@0.7.0"
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
| 0.1.x, 0.2.x, 0.3.x, 0.4.x, 0.5.x, 0.6.x | `>=0.1.0 <0.2.0` |
| 0.7.x | `>=0.2.0 <0.3.0` |

`craftsman --version` prints the CLI's version, the range it accepts and the
framework it finds. A framework outside the range is refused, naming both
versions.

**Upgrading to 0.7.x.** CLI 0.7.0 goes with framework 0.2.0, which replaced
flags with features and configs. `Control.Flag`, `:Flags`,
`Control.FlagService`, `KilledValue`, `Schedulable` and `SnapshotUrl` were
removed with no alias, and hosted flag values, kills and schedules do not carry
over. Move both pins together and follow
[Migrating to configuration](https://github.com/averyark/craftsman-control/blob/main/docs/migrating-to-configuration.md);
[Features and configs](https://github.com/averyark/craftsman-control/blob/main/docs/features.md)
is the reference.

## Commands

| Command | What it does |
|---|---|
| `craftsman init` | Writes what a feature-routed game needs for Project Control. Never overwrites a file |
| `craftsman serve` | Routes `src/Features` into `default.project.json` and runs `rojo serve`, restarting it when the routing changes or when Rojo misses a change |
| `craftsman project [--check]` | Routes `src/Features` into `default.project.json` and `release.project.json`, or checks they are current |
| `craftsman packages [--no-types]` | Runs `wally install`, writes `sourcemap.json`, and gives luau-lsp the packages' types |
| `craftsman publish` | Checks the declaration's manifest with Project Control and builds a release |
| `craftsman store plan \| import` | Prints the store catalogue's plan, or imports what Roblox already sells |
| `craftsman release <place.json>` | Builds a release candidate place file and its report |
| `craftsman test-roblox [<place.json>]` | Builds this checkout and runs `verify/` against it in Roblox |
| `craftsman login` | Signs this machine in to Project Control with GitHub |
| `craftsman logout` | Signs this machine out and forgets its session |
| `craftsman whoami` | Says who this machine is signed in as, until when, and in which projects |
| `craftsman summary [<dir>]` | Prints the release reports in `<dir>` (default `dist`) as Markdown |
| `craftsman skills [--check]` | Mirrors `.agents/skills` into `.claude/skills`, or checks that it matches |
| `craftsman --version` | Prints the versions described above |
| `craftsman help [<command>]` | Lists every command, or one command's options and examples |

Every command also takes `--help`. An option a command does not have is
refused, naming the nearest one (`--aply`: *Did you mean --apply?*), rather
than ignored.

To wire the framework into a game (the package, the `Stores` ModuleScript, the
declaration and its signing key, the game key's secret, the place file), follow
[Integrate into your game](#integrate-into-your-game) below.

### Place groups and activating

A publish **activates nothing**: a release's declarations go live in each place
group when it is deployed there, from the console. `--group <group>` builds for
that one group and checks the manifest against it, and still activates nothing.
To change one group's live features, configs, operations and permissions now,
without a release, say so:

```sh
craftsman publish --group Production --activate --no-build --apply
```

It ends with the line saying what changed where, e.g. *Activated in place group
Production (universe 71234567890)*.

**Changed in 0.4.0.** `--activate` is new. In 0.3.x, `--group G --apply`
activated in G at once, and `--group G --no-build --apply` was how you changed
declarations without a release. From 0.4.0, `--group` only scopes the build,
so scoping a build can never change a live game by accident.
`--group G --no-build --apply` without `--activate` is refused with exit 2.
0.3.x ignores `--activate` and `--save-place-version`, so everything in this
README needs 0.4.0 or later.

### Features by folder: `init`, `serve`, `project`

Keep each feature in one folder, `src/Features/<Feature>/`, and let the CLI
write the Rojo project. `base.project.json` is the part you edit;
`default.project.json` and `release.project.json` are generated, and
committed.

| In `src/Features/<Feature>/` | Becomes |
|---|---|
| `Shared/` | `ReplicatedStorage/Features/<Feature>` |
| `Server/` | `ServerScriptService/Features/<Feature>` |
| `Client/` | `ReplicatedStorage/Features/Client/<Feature>` |
| `Controller.luau` or `Controller/` | `StarterPlayer/StarterPlayerScripts/Controllers/<Feature>` |
| `Handler.luau` or `Handler/` | `ServerScriptService/Handlers/<Feature>` |
| `Network`, `Definitions`, `Types`, `Lookup` | inside the feature's `Shared` module, or a Folder in its place |

Names are matched exactly, case included. A `features.json` beside
`base.project.json` replaces the table:

```json
{
  "folder": "src/Features",
  "routes": {
    "Server": "ServerScriptService/Features",
    "Client": "StarterPlayer/StarterPlayerScripts/Features",
    "Types": { "into": "Server" }
  }
}
```

- `craftsman init --place <placeId>` writes `base.project.json` (a copy of
  your `default.project.json` if you have one), `src/Features/`, the
  `Stores`, `Definitions` and `Start` files, `places/main.place.json`, the
  release workflow, a VS Code task running `craftsman serve`, and
  `.gitignore` entries. It never overwrites, so run it again to add what is
  missing, e.g. a second place with `--role lobby`.
- `craftsman serve` regenerates the project and runs `rojo serve`. Adding,
  removing or renaming a feature or a routed file restarts `rojo serve`, and
  the Studio plugin must reconnect. Edits inside a feature restart nothing.
  It also checks that Rojo is serving what is on disk: every 3 s, and at once
  when Rojo prints an error, it compares the source of every script Rojo
  holds with your files. When Rojo has missed a change, `serve` names the
  files and restarts it, and the Studio plugin must reconnect then too.
  This happens with Rojo 7.7: a file routed on its own, like `Handler.luau`
  or `Types.luau`, that is deleted and recreated, even for a moment, is
  never watched again, and Rojo keeps serving the old copy while the plugin
  still shows as connected.
- `craftsman project` regenerates once; `--check` exits 1 when the files are
  out of date, for CI.

Every route's container exists even while no feature uses it, so a place's
`ownedPaths` does not change as features are added. When it misses
something, `init`, `serve` and `project` print the paths to add.
`craftsman release` refuses a generated overlay that is out of date, naming
`craftsman project`, so a build cannot leave a feature out.

### `packages`

`craftsman packages` runs `wally install`, routes `src/Features` when there is
a `base.project.json`, writes `sourcemap.json` from `default.project.json`,
then runs `wally-package-types` on `Packages/`, `ServerPackages/` and
`DevPackages/`, and puts back the generic type defaults it drops (Jecs's
`Entity<T = nil>` would otherwise become `Entity<T>`). It needs `wally`,
`rojo` and `wally-package-types` in `rokit.toml`, and names any that are
missing before it runs anything. `--no-types` skips `wally-package-types`.

### `test-roblox`

By default the code is tested as a private model, and no place version is
saved. `--save-place-version` (formerly `--place`, still accepted with a
warning) saves the whole build as a place version instead, and Project Control
allows that only on a place in an open, non-default place group, with
`craftsman.releases.deploy.open`: the default group's saves are what every
build starts from, and a protected group takes a passkey.

## Integrate into your game

There are nine steps, done in order. Each one names the exact place things go,
because two of them fail silently when they are wrong.

**1. Install the tools.** With [Rokit](https://github.com/rojo-rbx/rokit), in
your project's folder:

```sh
rokit add averyark/craftsman-cli@0.7.0 craftsman
rokit add rojo-rbx/rojo
rokit add UpliftGames/wally
```

The last argument of the first line names the command `craftsman`. The CLI
carries no framework: it loads the package from your project's `Packages/`.

**2. Add the package.** In `wally.toml`, then run `wally install`:

```toml
[dependencies]
CraftsmanControl = "averyark/craftsman-control@0.2.0"
Ledger = "xoifaii/ledger@5.2.1"
```

`Ledger` is only needed if you open a data store (step 4), and must be the
version the framework depends on. `wally install` also brings
`averyark/keeper`, which the runtime hands to a feature's `OnActivate`. Map
`Packages` to `ReplicatedStorage.Packages`. Every example here requires
`@game/ReplicatedStorage/Packages/CraftsmanControl`.

**3. Declare.** A **feature** is a part of the game Project Control can switch
on and off, and a **config** is a typed value on a feature that the console can
change without a release. Declare each feature in a ModuleScript of its own, for
example `ServerScriptService.Craftsman.Control.Shop`:

```lua
local Control = require("@game/ReplicatedStorage/Packages/CraftsmanControl")
local gt = Control.GreenTea

local Shop = Control.Scope("Shop"):Feature({
	Description = "The in-game shop.",
}):Config({
	DiscountPercent = Control.Config(gt.number({ integer = true, range = "[0, 75]" }), {
		Default = 0,
		Description = "Store item discounts.",
	}),
})

return Shop
```

The feature's `Active` is its on/off switch, `true` unless declared
`Active = false`. Every config needs a `Default` that fits its GreenTea type:
it is what the game runs on when Project Control is unreachable.

Then put a **ModuleScript** at
`ServerScriptService.Craftsman.Control.Definitions` that includes every
feature module. Neither module may touch `game` or use `const` at module scope,
because the CLI loads them outside Roblox to publish them:

```lua
local Control = require("@game/ReplicatedStorage/Packages/CraftsmanControl")
local Shop = require("./Shop")

return Control.Declare({
	Universe = 10202097921, -- the universe id, not a place id
	EventCeiling = 500,
	-- SigningKey = "<paste it from Settings → Signing in the console>",
	Includes = { Shop.Definition },
})
```

`SigningKey` is the public half of your project's signing key. Leave the line
commented out until you have copied the key from **Settings → Signing** in the
console, because a publish refuses anything that is not a key. Without it a
server trusts unsigned commands and documents, and warns at boot. With it, a
server refuses anything unsigned or signed by another key.

**4. Open your stores from a ModuleScript**, at exactly
`ServerScriptService.Craftsman.Control.Stores`. It returns `Control`:

```lua
local Control = require("@game/ReplicatedStorage/Packages/CraftsmanControl")
local Ledger = require("@game/ReplicatedStorage/Packages/Ledger")
local Player = require("./Player")

Control.Store({ Entity = Player.Profile, Ledger = Ledger })

return Control
```

**Use a ModuleScript, never a Script.** The console reads a document through a
Luau Execution session, which loads the place without running its Scripts. A
store opened by a Script works in the game but is invisible to the console.
If you have no entity declared yet, keep the module and let it only return
`Control`, because step 6 requires it.

**5. Save the game key in Roblox's Secrets Store.** The console shows it once,
under Settings → Universes. On the Creator Hub, open the experience's
**Secrets** page and add it as **`CRAFTSMAN_CONTROL`**. The framework reads no
fixed name: your game passes this one to `HttpService:GetSecret` in step 6, and
every console screen uses it. Two traps, both silent:

- Set the secret's **domain** to `operations.craftsman.systems`. With any other
  domain `GetSecret` still succeeds and every report is refused.
- A **local playtest cannot read the Creator Hub's secrets**. Check it from
  Team Test or a live server, or add the same value in Studio under
  File → Experience Settings → Security → Local Secrets.

**6. Start the runtime** from a **Script** under `ServerScriptService`. Put it
inside `ServerScriptService/Craftsman` if your code builds it (step 7), because
a build replaces only what `ownedPaths` names:

```lua
local HttpService = game:GetService("HttpService")

local Control = require("@game/ServerScriptService/Craftsman/Control/Stores")
local Definitions = require("@game/ServerScriptService/Craftsman/Control/Definitions")

Control.Reporter.Start({
	declaration = Definitions,
	secret = HttpService:GetSecret("CRAFTSMAN_CONTROL"),
	endpoint = "https://operations.craftsman.systems/api",
	ceiling = Definitions.event_ceiling,
})

local Runtime: Control.Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
})
```

Register any `Control.OnCommand` handlers before `Control.Start`. The Overview's
setup checklist ticks off *A server has reported* when the first report gets
through.

The code that runs a feature reads it through the module that declared it. A
feature is started and stopped by the runtime, possibly many times in one
server's life, so setup goes in `OnActivate`, and everything it creates or
connects goes through the Keeper it is handed, which is cleaned when the feature
switches off. A config is read with a colon:

```lua
local Shop = require("@game/ServerScriptService/Craftsman/Control/Shop")

Shop:OnActivate(function(keeper)
	local stand = keeper:Clone(ShopStand)
	stand.Parent = workspace

	keeper:Connect(Players.PlayerAdded, GreetShopper)
end)

local function Buy(player, item)
	if not Shop.Active then
		return false, "The shop is closed"
	end

	local percent = Shop.DiscountPercent:Get()
	-- ...
end

Shop.DiscountPercent:Observe(function(value, source)
	SetDiscount(value)
end)
```

**Features on the client.** A feature declared `Client = true` also runs on
clients, and a config a client reads must be `Shared = true`. A client can only
require a module it can see, so declare such a feature in a module under
`ReplicatedStorage` (in `craftsman init`'s layout, the feature's
`Definitions.luau`), still listed in `Includes` and inside a folder
`ownedPaths` names (step 7). Then a LocalScript requires it and starts the
client runtime once. Without that call a client feature never runs on a client,
with no error:

```lua
local Client: Control.ClientRuntime = Control.StartClient()

Client.OnReady(function()
	LoadingScreen.Enabled = false
end)
```

Every feature and config name in a client-readable module can be read by
players, so nothing secret belongs in a name; an unshared config's value never
leaves the server.

**7. Describe the place.** A release is your Studio place with your code laid
over it. `places/<role>.place.json` says how to build one. The file name is the
place's role, e.g. `places/main.place.json` for the role `main`:

```json
{
  "universeId": 10202097921,
  "placeId": 123021532395166,
  "source": "studio-download",
  "baseline": "places/main.rbxl",
  "overlayProject": "default.project.json",
  "definitions": "src/Control/Definitions",
  "ownedPaths": ["ReplicatedStorage/Packages", "ServerScriptService/Craftsman"]
}
```

| Field | What it is for |
|---|---|
| `universeId`, `placeId` | The place this file builds. The universe must be one the declaration names. |
| `source` | Where the Studio place comes from. `"studio-download"` is a place file you commit. `"control"` is the Studio place Project Control holds, handed to the build, with no `baseline`. |
| `baseline` | For `"studio-download"` only: the committed `.rbxl` (File → Download a Copy in Studio). Track it with Git LFS: `*.rbxl filter=lfs diff=lfs merge=lfs -text` in `.gitattributes`. |
| `overlayProject` | The Rojo project that builds your code. It may map only inside `ownedPaths`, or the build stops. |
| `definitions` | The declaration's path in the repository, without `.luau`. |
| `ownedPaths` | The folders your code owns. The build replaces them and proves nothing outside them changed. Include `ServerScriptService/Craftsman`, because the check that the `Stores` module exists runs only when it is owned. |

An overlay project for this example maps `Packages` and the `Craftsman` folder,
and nothing else:

```json
{
  "name": "game",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "Packages": { "$path": "Packages" }
    },
    "ServerScriptService": {
      "Craftsman": {
        "$className": "Folder",
        "Control": { "$path": "src/Control" },
        "Start": { "$path": "src/Start.server.luau" }
      }
    }
  }
}
```

`craftsman release places/main.place.json` builds it on your machine and sends
nothing, which is a quick check of this step.

**8. Sign in**, once per machine. Link GitHub on the console's Account page
(`https://operations.craftsman.systems/account`) and have an administrator
attach it to the project. Then run:

```sh
craftsman login
```

**9. Publish:**

```sh
craftsman publish            # check the manifest, and show what a release would carry
craftsman publish --apply    # build a release for every place group, watch it, test it
```

A publish activates nothing. A release's declarations go live in each place
group when it is deployed there, from the console's Releases. `craftsman help`
lists every command, and `craftsman <command> --help` shows its options and
examples.

## Agent skill

`skills/setup-project/` is an [Agent Skill](https://agentskills.io) that sets
a game repository up for Project Control: it does every step of
[Integrate into your game](#integrate-into-your-game) that a terminal can, and
hands you a checklist for the ones that need the console or the Creator Hub.
It never asks for or handles the game key or an Open Cloud key.

- **Any agent, via the skills CLI:** `npx skills add averyark/craftsman-cli`
- **Manually:** copy `skills/setup-project` into `.claude/skills/` (Claude Code)
  or `.agents/skills/` (Codex and others).

Then ask the agent to set up Project Control. Its steps are numbered like the
console's guide at `https://operations.craftsman.systems/onboard`.

## Exit codes

| Code | Meaning |
|---|---|
| 0 | Done: what was asked for happened, or a preview was printed |
| 1 | A verdict against you: refused, a failed build, or failed tests |
| 2 | The command line or its configuration is wrong: nothing was sent |
| 3 | Could not tell: unreachable, an unreadable answer, or no verdict |

A publish without `--apply` is a preview and exits 0. A test that could not be
run exits 3, never 0. An error from Project Control itself (a 5xx answer) exits
3, because an outage is not a verdict on your change.

## Release workflow

Copy [`templates/release.yml`](templates/release.yml) to `.github/workflows/release.yml`.
A project with an `ember.toml` must commit `ember.lock`, and should install
Ember outside `Packages/` (see the
[Craftsman Kit README](https://github.com/averyark/craftsman-kit#alongside-wally)).

## Which console

The CLI talks to the hosted console, `https://operations.craftsman.systems/api`,
by default. `CONTROL_ENDPOINT`, in the environment or the project's `.env`,
points it elsewhere, such as a local stack at
`http://127.0.0.1:54321/functions/v1`. A `.env` may name only the hosted
console or this machine (`127.0.0.1`, `localhost` or `[::1]`). The CLI refuses any
other host named by a file in the project, with exit 2, so a cloned repository
cannot send your session somewhere else. To reach another host, set
`CONTROL_ENDPOINT` in your shell's environment.

## Signing in

`craftsman login` signs in with GitHub's device flow: it prints a code and a
link, and you enter the code on GitHub. First link your GitHub account on the
console's Account page (`https://operations.craftsman.systems/account`), or
Project Control will not know who you are; a refusal that names the page prints
its address. The
session is stored at `~/.craftsman/session`, one per `CONTROL_ENDPOINT`.
`craftsman whoami` shows it and `craftsman logout` ends it. The first line every
command prints names the console it is talking to, and whether that console is
hosted or local.

Activating a protected place group (`--activate`) does not happen straight
away. The CLI prints a link and waits while someone confirms the activation with
a passkey in the console. A build that follows starts only once the activation
has applied.

## The game key

The game key belongs only in Roblox's Secret Store, saved under the name
`CRAFTSMAN_CONTROL`, where a live server reads it with
`HttpService:GetSecret("CRAFTSMAN_CONTROL")`. The framework reads no fixed name;
that is the one the game passes, and the one every console screen uses. Set the
secret's domain to `operations.craftsman.systems`, and check it from Team Test
or a live server, because a local playtest cannot read secrets.

The CLI never reads the game key, and Project Control refuses a publish signed
with it, even beside a good session. CLI 0.2.0 and earlier sign with it when
there is no session, reading it from `.env` as `CONTROL_SIGNING_SECRET`, and are
refused with:

> This console no longer accepts the game key from a terminal; update the CLI and run `craftsman login`.

If you see that, move to 0.3.0 or later, delete the `CONTROL_SIGNING_SECRET`
line from the project's `.env`, and run `craftsman login`.

## Windows

The CLI decides whether its output is a terminal by asking PowerShell
(`[Console]::IsOutputRedirected`) on Windows, and `sh -c "test -t 1"`
elsewhere. When it cannot tell, it prints plain progress lines and says so once,
on stderr. `--live` forces the moving line, and `--plain` turns the note off.

## License

MIT. See [LICENSE](LICENSE).
