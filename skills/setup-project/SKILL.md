---
name: setup-project
license: MIT
metadata:
  author: averyark
  repository: craftsman-cli
  cli-version: "0.10.2"
description: Set up Project Control in a Roblox game repository, end to end - install the craftsman CLI and Rojo, install Craftsman's packages (Control, the kit, Lifecycle and their deps) from Project Control's registry with craftsman init, write the Definitions and Stores ModuleScripts and the start Script, describe the place, add the release workflow, sign in, publish, and add the signing key - while handing the person an exact checklist for the steps that need their browser, passkey or Roblox's Creator Hub. Use when the user asks to set up, onboard, integrate, connect or install Project Control, craftsman-control or the craftsman CLI in their game, follows operations.craftsman.systems/onboard, or has a half-finished setup that fails to publish or report. Also use to check an existing setup for the traps that fail silently.
---

# Set up Project Control

Take a game repository from nothing to a published release. Do every step that
the terminal and the files can do, skip whatever is already done, and give the
person one short checklist for the rest. The same steps, for a person, are at
`https://operations.craftsman.systems/onboard`, numbered 1 to 19; this skill
uses the same numbers so the two can be read side by side.

## Rules

- **Survey before writing.** Every step checks whether it is already done and
  skips it if so. Running the skill again must change nothing that is right.
- **Never touch a secret.** The game key belongs only in Roblox's Secrets, the
  Open Cloud key only in the console. Never ask for either, never put either in
  a file, `.env` or a command. If the person pastes one, say it does not belong
  here, don't repeat it, and suggest rotating it in the console. The
  `SigningKey` (step 17) is a public key and is fine.
- **Ask for IDs, resolve what can be resolved.** A place ID is enough to find
  its universe (step 0).
- **Confirm before anything outward-facing**: `craftsman publish --apply`, a
  commit, a push. Follow the repository's own commit style.
- **Follow the project's code style** (its `CLAUDE.md`, `stylua.toml`,
  `selene.toml`) in every Luau file you write. The snippets below are
  style-neutral.

## 0. Survey

Run these from the repository root and read the results before doing anything:

```bash
git rev-parse --show-toplevel && git remote get-url origin
ls rokit.toml craftsman.toml craftsman.lock wally.toml ember.toml *.project.json places .github/workflows 2>/dev/null
craftsman --version
craftsman whoami
craftsman doctor --offline
```

- `craftsman --version` exits 1 when Craftsman is missing or its Control is
  another version than the CLI, and fails outright when the CLI is not
  installed. Both mean step 7 or 8 is left.
- `craftsman doctor --offline` names what is wrong with an existing setup and
  how to fix each, reading only. A `craftsman.lock` that names a *bundle*, or a
  `wally.toml` naming `averyark/craftsman-control`, `-kit` or `-lifecycle`, is
  an older game: step 8 moves it with `craftsman update`.
- `craftsman whoami` names the person and the projects their GitHub account is
  attached to. *Not signed in* means step 15 is left, and the project missing
  from that list means step 14 is.
- Find the Rojo project that builds the game (`base.project.json` with
  `init`'s layout, else usually `default.project.json`) and read where it maps
  `ReplicatedStorage`, `ServerScriptService` and `CraftsmanPackages`.
- Look for existing `Definitions`, `Stores`, `[places]` in `craftsman.toml` and
  `.github/workflows/release.yml`.

Ask for the **place ID** of the game's start place (the number in
`roblox.com/games/<placeId>/…`) and resolve its universe:

```bash
curl -s https://apis.roblox.com/universes/v1/places/<placeId>/universe
```

It answers `{"universeId": …}`. Say the universe ID back to the person: it is
the one they add in the console, and a place ID there is the most common
mistake.

Then show a table of all 19 steps: **done**, **I'll do it**, or **needs you**.

## 1. Hand over the console steps first

These need a browser, a passkey or the Creator Hub, so no agent can do them.
Give them as one checklist **now**, so the person works through it while you
wire the repository, and only mark the ones the survey could not prove done:

1. Create an account at `https://operations.craftsman.systems/signup`.
2. Accept the project invitation on the Projects page.
3. **Settings → Universes**: add universe `<universeId>`, the universe ID and
   not a place ID.
4. Save an Open Cloud key on it and press **Test** until it passes. The key
   form lists what is needed to start. One of them, `asset:read`, Roblox grants
   **only on a group**: for a group-owned game, grant it on the group that owns
   the game, either on this key or on the universe's separate **asset key**
   (same screen). For a game owned by a user, that user's own key needs no
   grant, and nobody else's key can have it. **Test** does not check it.
5. Save the game key shown once in the experience's **Secrets** as
   `CRAFTSMAN_CONTROL`, with the domain `operations.craftsman.systems`, and turn
   on **Allow HTTP Requests** in Studio's Game Settings → Security.
6. **Settings → Places**: map place `<placeId>` to the default place group.
13. **Settings → Releases**: name the repository, `<owner>/<repository>` from
    `git remote get-url origin`.
14. On the **Account** page: **Link a GitHub account** (the one this browser
    uses on GitHub, which is also the one `craftsman login` will use), then
    **Confirm with a passkey**. Under the account, choose the project and
    **Request attachment**; it shows *Waiting*. Another administrator approves
    it in **Approvals** and it shows *Attached*; the project's only
    administrator approves their own. Linking alone gives no access.

Three of these fail without an error, so say them plainly: a secret whose
domain does not cover `operations.craftsman.systems` is read fine and every
report is then refused; a local playtest cannot read Secrets at all (Team Test
or a live server can; Studio's File → Experience Settings → Security → Local
Secrets is a separate store for local runs); and a key that passes **Test**
without `asset:read` still cannot read a document or deploy, because Test does
not check it.

## 2. Wire the repository

**Steps 14 and 15 come before step 8.** Installing Craftsman downloads its
packages from Project Control's registry, which needs `craftsman login` and a
GitHub account attached to the project. Do step 7, then have the person finish
14 and run 15 (below), then step 8.

### Step 7: tools

If `rokit` itself is missing, stop and ask the person to install it
(`https://github.com/rojo-rbx/rokit`); don't download an installer yourself.
Otherwise add only what `rokit.toml` lacks:

```bash
rokit add averyark/craftsman-cli@0.10.2 craftsman
rokit add rojo-rbx/rojo
```

The last word of the first line names the command `craftsman`. Wally is not
needed for Craftsman, which never comes from Wally; keep `UpliftGames/wally`
only when the game installs packages of its own with it. An older `craftsman`
pin moves to 0.10.2, then `rokit install`, and step 8 runs `craftsman update`.
If the game uses framework 0.1.x (`Control.Flag`, `:Flags`,
`Control.FlagService`), stop and tell the person: moving it is a migration, not
a pin bump, and follows "Upgrading older declarations" in this repository's
README.

### Step 8: install Craftsman

Only after step 15's `craftsman whoami` lists the project. Then, for a game
with no `craftsman.toml`:

```bash
craftsman init --place <placeId>
```

`init` downloads Craftsman's four packages (Control, always the CLI's version;
the kit; Lifecycle; and deps, the third-party code they share, Ledger and Keeper
included) into `CraftsmanPackages/` and `CraftsmanDevPackages/`, writes
`craftsman.toml` and `craftsman.lock`, and writes steps 9 to 13's files for a
project whose features live in `src/Features/`, keeping any that exist. Read
what it wrote against the steps below rather than writing them by hand.

- **A game that already has `craftsman.toml`:** run `craftsman update`, which
  moves every package to the newest set for this CLI, including a lock that
  still names a bundle (CLI 0.9 or 0.10.0). Its `release.yml` must then be
  replaced with the template (step 13): the old one cannot read the new lock.
- **A game whose `wally.toml` names Craftsman:** run `craftsman update`, which
  moves it off Wally all or nothing (removes those packages from `wally.toml`,
  rewrites requires of `ReplicatedStorage/Packages/<name>` to
  `CraftsmanPackages/<name>`, maps both folders, moves `features.json` and
  `src/CraftsmanConfig.luau` into `craftsman.toml`), and lists what it could
  not change safely. Show the person that list.

The package folders are gitignored. `craftsman.toml` and `craftsman.lock` are
committed: `craftsman install` and CI install exactly what the lock names. Don't
add any Craftsman package to `wally.toml`. If the project uses `ember.toml`,
ask before changing anything about its package manager.

### Steps 9-11: the code

These files go at **exact** paths in the DataModel. `init`'s
`base.project.json` maps them there; with the game's own Rojo project, map them
the same way:

| File | DataModel path | Kind |
|---|---|---|
| `src/Control/Definitions.luau` | `ReplicatedStorage.Craftsman.Definitions` | ModuleScript |
| `src/Features/Shop/Definitions.luau` (one per feature) | `ReplicatedStorage.Features.Shop.Definitions` | ModuleScript |
| `src/Control/Stores.luau` | `ServerScriptService.Craftsman.Control.Stores` | ModuleScript |
| `src/Start.server.luau` | `ServerScriptService.Craftsman.Start` | Script |

```json
"ReplicatedStorage": {
  "CraftsmanPackages": { "$path": "CraftsmanPackages" },
  "CraftsmanDevPackages": { "$path": "CraftsmanDevPackages" },
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
```

The declaration sits in `ReplicatedStorage` so clients can read it for client
features. `craftsman feature <Name> "<description>"` adds a feature's folder and
`Definitions.luau` and its entry in `Includes`.

**Definitions** (step 9). Ask what the game should declare first; a feature
they already have, so it can be switched off, is a good start. Don't invent a
feature or config nobody asked for. Each feature is declared in its folder's
`Definitions.luau` (here `src/Features/Shop/Definitions.luau`); a feature's
`Active` is its on/off switch, and every config needs a `Default` that fits its
GreenTea type:

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

`Definitions` (which `init` writes with the universe) includes every feature
module. `../Features` resolves the same on disk and in Roblox, from
`ReplicatedStorage.Craftsman`:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Shop = require("../Features/Shop/Definitions")

return Control.Declare({
	Universe = <universeId>,
	EventCeiling = 500,
	Includes = { Shop.Definition },
})
```

The CLI loads these modules under Lune, outside Roblox, to publish them:
**nothing at module scope may read `game`, `workspace`, `script` or `Enum`, and
no `const`.** Declaration keys are PascalCase. `Universe` takes a list
(`{ a, b }`) when the same code runs in several universes.

A feature the client runs is declared `Client = true`, with every config the
client reads `Shared = true`, in a module under `ReplicatedStorage` (in
`craftsman init`'s layout, the feature's `Definitions.luau`), and needs a
LocalScript that requires the declaration and calls `Control.StartClient()`
once; without that call it never runs on a client, with no error. `init`'s
`src/Bootstrap.client.luau` already does both, so check it is there rather than
writing another. Feature and config names are readable by
players, so nothing secret goes in a name.

The game's own code reads the feature through the module that declared it:
setup in `Shop:OnActivate(function(keeper) ... end)`, connecting through the
Keeper; a request-time check as `if not Shop.Active then`; a value as
`Shop.DiscountPercent:Get()` or `:Observe(fn)`, always with a colon. Don't
rewrite game code the person did not ask about.

**Stores** (step 10). A ModuleScript that returns `Control`. With no entity
declared yet it is only the first and last lines:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")

return Control
```

**Never make it a Script.** The console reads data through a Luau Execution
session, which loads the place without running its Scripts, so a store opened
by a Script works in the game and is invisible to the console. When the game
later declares an entity, its `Control.Store({ Entity = …, Ledger = Ledger })`
goes here, with `Ledger` required from
`@game/ReplicatedStorage/CraftsmanPackages/Ledger`.

**Start** (step 11). A Script inside `ServerScriptService.Craftsman`. Any
`Control.OnCommand` handlers are registered before `Control.Start`.

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

If the repository's requires use another form (instance paths, a different
alias), match it. `init` also writes `src/Bootstrap.server.luau` and
`src/Bootstrap.client.luau`, which start the kit's state machine and load the
game's `Handler`s and `Controller`s with Lifecycle; keep them unless the game
already starts those another way, and then ask.

### Step 12: the place

A `[places.<role>]` table in `craftsman.toml`; the role is `main` unless the
person says otherwise. `craftsman init --place <placeId>` writes it. With
`init`'s layout (a `base.project.json`, so `release.project.json` is generated)
the two ids are all it needs:

```toml
[places.main]
universe = <universeId>
place = <placeId>
```

- **What the place owns** is the folders the build replaces, and it proves
  nothing outside them changed. With a generated overlay, the place owns what
  `release.project.json` maps, `CraftsmanPackages` and `CraftsmanDevPackages`
  included; add `owned = [...]` only for a folder it does not map. With the game's own Rojo project, set `overlay` to it and list every
  owned folder in `owned`, `ReplicatedStorage/CraftsmanPackages` among them,
  plus `ServerScriptService/Craftsman` (the check that `Stores` exists runs
  only when it is owned). Never a whole service unless the
  project really maps the whole service.
- **`overlay`** (default `release.project.json`) may map only inside what the
  place owns, or the build stops.
- **`source`**: ask. The default, `"control"`, uses the Studio place Project
  Control holds. `"studio-download"` needs the Studio place committed as
  `baseline = "places/main.rbxl"` (Studio's File → Download a Copy) and tracked
  with Git LFS: add `*.rbxl filter=lfs diff=lfs merge=lfs -text` to
  `.gitattributes`.
- An old `places/<role>.place.json` is no longer read; `craftsman install`
  moves it into `craftsman.toml`.

With a baseline, check it on this machine; it sends nothing:

```bash
craftsman release main
```

### Step 13 (the file half): the workflow

`craftsman init` writes it. Without `init`:

```bash
mkdir -p .github/workflows
curl -fsSL https://raw.githubusercontent.com/averyark/craftsman-cli/main/templates/release.yml -o .github/workflows/release.yml
```

It holds no secret: the run signs in with GitHub's OIDC token, and the console
accepts only the repository named under Settings → Releases. Its `prepare` job
downloads exactly the packages `craftsman.lock` names, and `build` installs them
offline. A workflow written by CLI 0.10.0 or before cannot read today's lock:
replace it with this template whenever the game moves to 0.10.1 or later.

## 3. Publish

### Step 15: sign in

Done before step 8 (see *Wire the repository*). `craftsman login` runs GitHub's
device flow and waits for the person, so ask them to run it themselves (in
Claude Code: `! craftsman login`). Then check:

```bash
craftsman whoami
```

The project must be in its list. If it says the account is attached to no
project, step 14's attachment is not approved yet: it shows *Waiting* on the
Account page until an administrator approves it in Approvals.

### Step 16: publish

Preview first. It exits 0 even when it lists problems, so read it:

```bash
craftsman publish
```

A publish with `--apply` builds from what is **pushed**, so the new files must
be committed and pushed first. Ask before committing, pushing, and then:

```bash
craftsman publish --apply
```

It builds a release for every place group and tests it in Roblox. **It
activates nothing**; a release goes live when it is deployed from the console.

| Exit | Meaning | What to do |
|---|---|---|
| 0 | Done, or a preview | Read the output anyway |
| 1 | A verdict about the change | Read the refusal; fix the file or the console step it names. Running it again unchanged gets the same answer |
| 2 | The command line or configuration is wrong; nothing was sent | Fix the option or the declaration path, or run `craftsman install` / `craftsman update` when Craftsman is missing or its Control is another version |
| 3 | Could not tell (unreachable, a 5xx, no verdict) | Not about the change; try again later |

A refusal that names the Account page ends with its address: it is step 14's
attachment.

### Step 17: signing key

Ask the person to open **Settings → Signing**, sign what is published, and
paste the `SigningKey` line it shows. Add it to `Control.Declare`:

```lua
	SigningKey = "<the key they pasted>",
```

Then preview, commit, push and publish with `--apply` again, with the same
confirmations. Tell them to **pin** the key in Settings → Signing once it says
it is safe to. Without a `SigningKey` a server accepts unsigned commands and
warns at boot; with one it refuses anything unsigned.

## 4. Hand back

The last two steps are the person's, in the console:

18. **Releases**: deploy to the default place group first, then the rest.
19. Optional: `/onboard invite` in their Discord server, then a role for each
    person under **Members**.

Finish with the table from step 0, updated: what you did (files written,
commands run), what they still have to do, and the one check that proves it
all works: start a **Team Test** session or a live server and watch the
project's Overview, where *A server has reported* ticks off when the first
report gets through. If it never does, check in this order: Allow HTTP
Requests, the secret's name, the secret's domain, and that `Start` is a Script
under `ServerScriptService.Craftsman`.
