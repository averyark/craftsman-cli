# craftsman

The `craftsman` command-line tool for Project Control: it installs Craftsman
into a game, publishes the game's declaration, builds release candidates and
runs the in-Roblox tests. This repository holds only the released binaries.
The source is developed with the `craftsman-control` framework.

## Install

With [Rokit](https://github.com/rojo-rbx/rokit), from your project's folder:

```sh
rokit add averyark/craftsman-cli@0.11.0 craftsman
```

The last argument names the command. Without it, Rokit names the command after
the repository, `craftsman-cli`. The same thing as a line in `rokit.toml`:

```toml
[tools]
craftsman = "averyark/craftsman-cli@0.11.0"
```

Then run `rokit install`. Builds exist for Windows x86_64, Linux x86_64 and
aarch64, and macOS x86_64 and aarch64.

`craftsman release` also needs Rojo, so a project's `rokit.toml` should list
`rojo` too.

## Craftsman's packages

The CLI does not carry the framework. Craftsman is four packages, each
published on its own and at its own version:

| Package | Installs as | Version |
|---|---|---|
| Control | `CraftsmanPackages/CraftsmanControl` | always the CLI's own |
| Craftsman Kit | `CraftsmanPackages/Craftsman` | its own |
| Lifecycle | `CraftsmanPackages/Lifecycle` | its own |
| deps | the third-party code the three share (Keeper, Promise, Signal, ByteNet, Ledger, Konsole, and Jest in `CraftsmanDevPackages`) | its own |

`craftsman init`, `install` and `update` download them from Project Control's
registry into `CraftsmanPackages/` and `CraftsmanDevPackages/`, which are
gitignored and mapped into `ReplicatedStorage`. Code requires them from there,
e.g. `@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl`. Wally never
installs any of them; a game may keep Wally for packages of its own, in
`Packages/`.

`craftsman.lock` names each package's version and hashes, and is committed:
`craftsman install` installs exactly what it names, and CI does the same. A
package is checked against the lock, and the four against each other, before
anything is written.

- `craftsman update` moves every package to the newest set that fits.
- `craftsman update kit` moves only the kit, keeping the others where they are
  while they still fit. `craftsman update kit@0.10.1` pins it exactly.
- Control moves with the CLI: change the `craftsman` pin in `rokit.toml`, run
  `rokit install`, then `craftsman update`. When a newer Control is published,
  `update` prints the `rokit.toml` line that would run it, and never edits the
  file itself.

Downloading needs `craftsman login` and a GitHub account attached to a project
(see [Signing in](#signing-in)), or in GitHub Actions the run's OIDC token from
a repository bound to a project, which the release workflow template uses.

`craftsman --version` prints the CLI's version and the Control it finds.
`craftsman doctor` checks that what is installed is what `craftsman.lock`
names, and that Control is the CLI's version.

### Moving a game onto the packages

- **A game that still installs Craftsman with Wally** (its `wally.toml` names
  `averyark/craftsman-control`, `-kit` or `-lifecycle`): `craftsman update`
  moves it, all or nothing. It removes those packages and the ones Craftsman
  carries from `wally.toml`, rewrites requires of
  `ReplicatedStorage/Packages/<name>` to `CraftsmanPackages/<name>`, maps both
  folders, moves `features.json` and `src/CraftsmanConfig.luau` into
  `craftsman.toml`, and installs the packages. It lists anything it cannot
  change safely.
- **A game on CLI 0.9 or 0.10.0**, whose `craftsman.lock` names one bundle:
  move the `rokit.toml` pin to 0.11.0, run `rokit install` and
  `craftsman update`, and replace `.github/workflows/release.yml` with
  [`templates/release.yml`](templates/release.yml): the old workflow cannot read
  the new lock. Commit `craftsman.lock`, `craftsman.toml`, `rokit.toml` and the
  workflow. `install` refuses a lock that still names a bundle, saying so.

### Upgrading older declarations

**From flags (framework 0.1.x).** Framework 0.2.0 replaced flags with features
and configs. `Control.Flag`, `:Flags`, `Control.FlagService`, `KilledValue`,
`Schedulable` and `SnapshotUrl` were removed with no alias, and hosted flag
values, kills and schedules do not carry over:

1. Put each flag under a feature: `Control.Scope("Shop"):Feature():Config({ ... })`,
   with each value declared as `Control.Config(gtType, { Default = ... })`. A
   boolean "enabled" flag becomes the feature's `Active`, not a config.
2. Replace `Control.FlagService():Start(...)` with `Control.Start(declaration)`,
   and call `Control.StartClient()` from a LocalScript if any feature has
   `Client = true`.
3. Read values with `:Get()` and `:Observe(fn)`, and move code gated on a flag
   into the feature's `OnActivate`.
4. Publish with `craftsman publish --group <group> --activate --apply`, then set the
   hosted values again in the console's Configuration tab.

**Declaring where it is used.** A config or a command may be declared in the
file that uses it, with `<scope>:Declare(name, spec)` (`craftsman-control`'s
`docs/features.md`, *Declaring where it is used*). Three things can stop you:

- `Declare` is a reserved name. A config, command or child scope called
  `Declare` fails to load, saying a scope answers to that name itself. Rename
  it.
- Once a feature's files call `:Declare(`, `craftsman project` writes that
  feature's `Definitions/Generated.luau`, and `Definitions` must be a folder
  whose `init.luau` adopts it with `require("@self/Generated")`. `release` and
  `publish` refuse a `Generated.luau` that is stale or that nothing adopts. Run
  `craftsman project` (or keep `craftsman serve` running), and commit
  `Generated.luau` with the change.
- The first time a file contains `:Declare(`, the CLI downloads Luau's
  `luau-ast` into `~/.craftsman/tools/` and checks it against a pinned SHA-256.
  On a machine that cannot download it, set `CRAFTSMAN_LUAU_AST` to a
  `luau-ast` you already have. Rokit cannot install it: under that alias it
  installs the Luau interpreter, which runs the file instead of parsing it.

## Commands

| Command | What it does |
|---|---|
| `craftsman init` | Installs Craftsman's packages and writes what a feature-routed game needs for Project Control. Never overwrites a file |
| `craftsman install [--frozen]` | Installs exactly the packages `craftsman.lock` names, and writes `Settings.luau` from `craftsman.toml` |
| `craftsman update [<package>[@<version>]]` | Moves the packages, one or all, and moves a game off Wally |
| `craftsman feature <Name> <description>` | Adds a feature: its folder and `Definitions`, and its entry in the declaration's `Includes` |
| `craftsman serve` | Routes `src/Features` into `default.project.json` and runs `rojo serve`, restarting it when the routing changes or when Rojo misses a change |
| `craftsman project [--check]` | Routes `src/Features` into `default.project.json` and `release.project.json`, or checks they are current |
| `craftsman check` | Type-checks and lints the code with luau-lsp, against `check-baseline.json`, and warns on raw `rbxassetid://` literals |
| `craftsman doctor` | Looks for the mistakes that fail silently, and says how to fix each |
| `craftsman publish` | Checks the declaration's manifest with Project Control and builds a release |
| `craftsman status` | Says what Project Control has for this project: places, releases, the session |
| `craftsman asset <add\|replace\|import\|rollback\|push\|list\|status>` | Adds animations, audio, images (`Assets.Images`, the image id, never the decal; needs the universe's download key) and videos to `assets/index.toml`, uploads them once per owner, and writes `src/Control/Assets.luau`. `status` is a CI gate |
| `craftsman store <add\|edit\|gift\|import\|rollback\|push\|list\|status>` | Creates, changes and adopts game passes, developer products (with an optional gift product) and subscriptions in every universe of `[places]`. `gift <key>` makes the product `<key>.gift`, the gift of `<key>`, which takes its own price and for-sale, defaulting to `<key>`'s |
| `craftsman asset` / `craftsman store` | With no action, in a terminal: browse the index by kind and entry, then copy an entry's `Assets.…` path, rename, replace, push, roll back or edit it, or create a store item's gift |
| `craftsman release [<role>]` | Builds a release candidate place file and its report |
| `craftsman test-roblox [<role>]` | Builds this checkout and runs `verify/` against it in Roblox |
| `craftsman login` | Signs this machine in to Project Control with GitHub |
| `craftsman logout` | Signs this machine out and forgets its session |
| `craftsman whoami` | Says who this machine is signed in as, until when, and in which projects |
| `craftsman summary [<dir>]` | Prints the release reports in `<dir>` (default `dist`) as Markdown |
| `craftsman skills [--check]` | Mirrors `.agents/skills` into `.claude/skills`, or checks that it matches |
| `craftsman package <action>` | Builds and publishes one of Craftsman's own packages. For Craftsman's repositories, never a game's |
| `craftsman --version` | Prints the CLI's version and the Control it finds |
| `craftsman help [<command>]` | Lists every command, or one command's options and examples |

Every command also takes `--help`. An option a command does not have is
refused, naming the nearest one (`--aply`: *Did you mean --apply?*), rather
than ignored.

To wire the framework into a game (the packages, the `Stores` ModuleScript, the
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

Names are matched exactly, case included. `[features]` in `craftsman.toml`
replaces the table:

```toml
[features]
folder = "src/Features"

[[features.routes]]
name = "Server"
to = "ServerScriptService/Features"

[[features.routes]]
name = "Types"
into = "Server"
```

A `features.json` from an older CLI is no longer read: `craftsman install`
moves it into `craftsman.toml` and deletes it.

- `craftsman init --place <placeId>` installs Craftsman's packages (writing
  `craftsman.toml` and `craftsman.lock`), then writes `base.project.json` (a
  copy of your `default.project.json` if you have one), `src/Features/`, the
  `Stores`, `Definitions` and `Start` files, two bootstraps that start the kit
  and Lifecycle, `[places.main]` in `craftsman.toml`, the release workflow, a
  VS Code task running `craftsman serve`, and `.gitignore` entries. It never
  overwrites, so run it again to add what is missing, e.g. a second place with
  `--role lobby`.
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

Every route's container exists even while no feature uses it, and a place
owns whatever the release project maps, so nothing needs listing as features
are added. `default.project.json` and `release.project.json` are generated and
gitignored: `craftsman install` and `serve` write them, and `craftsman release`
writes them again before it builds, so a build cannot leave a feature out.

### `test-roblox`

By default the code is tested as a private model, and no place version is
saved. `--save-place-version` (formerly `--place`, still accepted with a
warning) saves the whole build as a place version instead, and Project Control
allows that only on a place in an open, non-default place group, with
`craftsman.releases.deploy.open`: the default group's saves are what every
build starts from, and a protected group takes a passkey.

## Integrate into your game

There are nine steps, done in order. Each one names the exact place things go,
because two of them fail silently when they are wrong. `craftsman init` writes
most of steps 4 to 8 for you; the steps say what it wrote, so you can check it.

**1. Install the tools.** With [Rokit](https://github.com/rojo-rbx/rokit), in
your project's folder:

```sh
rokit add averyark/craftsman-cli@0.11.0 craftsman
rokit add rojo-rbx/rojo
```

The last argument of the first line names the command `craftsman`. Add
`UpliftGames/wally` only if the game installs packages of its own with Wally;
Craftsman itself never comes from Wally.

**2. Sign in**, once per machine, because downloading Craftsman needs it. Link
GitHub on the console's Account page
(`https://operations.craftsman.systems/account`) and have an administrator
attach it to the project. Then run:

```sh
craftsman login
craftsman whoami   # the project must be in its list
```

**3. Install Craftsman:**

```sh
craftsman init --place <placeId>
```

It downloads Control, the kit, Lifecycle and their deps into
`CraftsmanPackages/` and `CraftsmanDevPackages/`, writes `craftsman.toml` and
`craftsman.lock`, and the files the next steps describe. Commit
`craftsman.toml` and `craftsman.lock`; the package folders are gitignored and
`craftsman install` puts them back. A game that still installs Craftsman with
Wally runs `craftsman update` instead (see
[Moving a game onto the packages](#moving-a-game-onto-the-packages)).

**4. Declare.** A **feature** is a part of the game Project Control can switch
on and off, and a **config** is a typed value on a feature that the console can
change without a release. Declare each feature in its folder, for example
`src/Features/Shop/Definitions.luau`, which the routing puts in
`ReplicatedStorage.Features.Shop`:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
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

Then the declaration, `src/Control/Definitions.luau`, includes every feature.
`init` writes it with your universe, and the routing maps it to
`ReplicatedStorage.Craftsman.Definitions`, where clients can read it too:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Shop = require("../Features/Shop/Definitions")

return Control.Declare({
	Universe = 10202097921, -- the universe id, not a place id
	EventCeiling = 500,
	-- SigningKey = "<paste it from Settings → Signing in the console>",
	Includes = { Shop.Definition },
})
```

Neither module may read `game` or use `const` at module scope, because the CLI
loads them outside Roblox to publish them.

`SigningKey` is the public half of your project's signing key. Leave the line
commented out until you have copied the key from **Settings → Signing** in the
console, because a publish refuses anything that is not a key. Without it a
server trusts unsigned commands and documents, and warns at boot. With it, a
server refuses anything unsigned or signed by another key.

**5. Open your stores from a ModuleScript**, at exactly
`ServerScriptService.Craftsman.Control.Stores` (`src/Control/Stores.luau`,
which `init` writes). It returns `Control`:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Ledger = require("@game/ReplicatedStorage/CraftsmanPackages/Ledger")
local Player = require("@game/ReplicatedStorage/Features/Player/Definitions")

Control.Store({ Entity = Player.Profile, Ledger = Ledger })

return Control
```

**Use a ModuleScript, never a Script.** The console reads a document through a
Luau Execution session, which loads the place without running its Scripts. A
store opened by a Script works in the game but is invisible to the console.
If you have no entity declared yet, keep the module and let it only return
`Control`, as `init` writes it, because step 7 requires it.

**6. Save the game key in Roblox's Secrets Store.** The console shows it once,
under Settings → Universes. On the Creator Hub, open the experience's
**Secrets** page and add it as **`CRAFTSMAN_CONTROL`**. The framework reads no
fixed name: your game passes this one to `HttpService:GetSecret` in step 7, and
every console screen uses it. Two traps, both silent:

- Set the secret's **domain** to `operations.craftsman.systems`. With any other
  domain `GetSecret` still succeeds and every report is refused.
- A **local playtest cannot read the Creator Hub's secrets**. Check it from
  Team Test or a live server, or add the same value in Studio under
  File → Experience Settings → Security → Local Secrets.

**7. Start the runtime** from a **Script** inside `ServerScriptService.Craftsman`
(`src/Start.server.luau`, which `init` writes), because a build replaces only
what the place owns:

```lua
local HttpService = game:GetService("HttpService")

local Control = require("@game/ServerScriptService/Craftsman/Control/Stores")
local Definitions = require("@game/ReplicatedStorage/Craftsman/Definitions")

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
through. `init` also writes `src/Bootstrap.server.luau` and
`src/Bootstrap.client.luau`, which start the kit's state machine and load your
`Handler`s and `Controller`s with Lifecycle.

The code that runs a feature reads it through the module that declared it. A
feature is started and stopped by the runtime, possibly many times in one
server's life, so setup goes in `OnActivate`, and everything it creates or
connects goes through the Keeper it is handed, which is cleaned when the feature
switches off. A config is read with a colon:

```lua
local Shop = require("@game/ReplicatedStorage/Features/Shop/Definitions")

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
require a module it can see, so its declaration must be under
`ReplicatedStorage`, as a feature's `Definitions.luau` is. `init`'s client
bootstrap requires `ReplicatedStorage.Craftsman.Definitions` and calls
`Control.StartClient()` once. Without that call a client feature never runs on a
client, with no error:

```lua
local Client: Control.ClientRuntime = Control.StartClient()

Client.OnReady(function()
	LoadingScreen.Enabled = false
end)
```

Every feature and config name in a client-readable module can be read by
players, so nothing secret belongs in a name; an unshared config's value never
leaves the server.

**8. Describe the place.** A release is your Studio place with your code laid
over it. A `[places.<role>]` table in `craftsman.toml` says how to build one,
e.g. `[places.main]` for the role `main`. `craftsman init --place <placeId>`
writes it, and with `init`'s layout the two ids are all it needs:

```toml
[places.main]
universe = 10202097921
place = 123021532395166
```

| Key | Default | What it is for |
|---|---|---|
| `universe`, `place` | required | The place this builds. The universe must be one the declaration names. |
| `source` | `"control"` | Where the Studio place comes from. `"control"` is the Studio place Project Control holds, handed to the build. `"studio-download"` is a place file you commit. |
| `baseline` | none | For `"studio-download"` only: the committed `.rbxl` (File → Download a Copy in Studio). Track it with Git LFS: `*.rbxl filter=lfs diff=lfs merge=lfs -text` in `.gitattributes`. |
| `overlay` | `release.project.json` | The Rojo project that builds your code. It may map only inside what the place owns, or the build stops. |
| `definitions` | `src/Control/Definitions` | The declaration's path in the repository, without `.luau`. |
| `owned` | none | Folders your code owns beyond what the overlay maps. |

The build replaces what the place owns and proves nothing outside it changed.
With a generated overlay, the place owns everything `release.project.json`
maps, `CraftsmanPackages` and `CraftsmanDevPackages` included, so `owned` is
only for something it does not. With a Rojo project of your own, nothing is
derived and `owned` lists every folder. Include `ServerScriptService/Craftsman`,
because the check that the `Stores` module exists runs only when it is owned:

```toml
[places.main]
universe = 10202097921
place = 123021532395166
source = "studio-download"
baseline = "places/main.rbxl"
overlay = "game.project.json"
owned = ["ReplicatedStorage/CraftsmanPackages", "ReplicatedStorage/Craftsman", "ServerScriptService/Craftsman"]
```

An overlay project for this example maps the packages, the declaration and the
`Craftsman` folder, and nothing else:

```json
{
  "name": "game",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "CraftsmanPackages": { "$path": "CraftsmanPackages" },
      "Craftsman": {
        "$className": "Folder",
        "Definitions": { "$path": "src/Control/Definitions.luau" }
      }
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

`craftsman release main` builds it on your machine and sends nothing, which is
a quick check of this step. A project from before this keeps its places in
`places/<role>.place.json`: `craftsman install` moves each one into
`craftsman.toml` and deletes it.

**9. Publish:**

```sh
craftsman publish            # check the manifest, and show what a release would carry
craftsman publish --apply    # build a release for every place group, watch it, test it
```

A publish activates nothing. A release's declarations go live in each place
group when it is deployed there, from the console's Releases. `craftsman help`
lists every command, and `craftsman <command> --help` shows its options and
examples.

## Agent skills

Two [Agent Skills](https://agentskills.io) live in `skills/`:

- **`setup-project`** sets a game repository up for Project Control: it does
  every step of [Integrate into your game](#integrate-into-your-game) that a
  terminal can, and hands you a checklist for the ones that need the console or
  the Creator Hub. It never asks for or handles the game key or an Open Cloud
  key. Ask the agent to set up Project Control; its steps are numbered like the
  console's guide at `https://operations.craftsman.systems/onboard`.
- **`craftsman-control`** teaches an agent writing game code what Project
  Control already provides (features and configs, commands and `Control.Run`,
  entities and stores, the store and receipts, the CLI) and how to call it, so
  it declares through Control instead of hand-rolling a flag, an admin command,
  a DataStore wrapper or a receipt handler the console cannot see.

Install them with:

- **Any agent, via the skills CLI:** `npx skills add averyark/craftsman-cli`
- **Manually:** copy the folders under `skills/` into `.agents/skills/`, then
  run `craftsman skills` to mirror them into `.claude/skills/` for Claude Code.

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

`craftsman init` writes [`templates/release.yml`](templates/release.yml) to
`.github/workflows/release.yml`; without `init`, copy it there. Its `prepare`
job downloads exactly the packages `craftsman.lock` names, with the run's OIDC
token, and `build` installs them offline with `craftsman install --frozen`, so
the build holds no token. A workflow from CLI 0.10.0 or before cannot read
today's lock: replace it with the template when you move to 0.10.1 or later.
A project with an `ember.toml` must commit `ember.lock`.

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
