---
name: craftsman-control
description: Use when writing or reviewing Roblox Luau code in a game that depends on Project Control (a `craftsman.toml` or `craftsman.lock`, `require(...CraftsmanPackages.CraftsmanControl)`, a `Control.Declare` Definitions module, or the `craftsman` CLI in rokit.toml), or when the user mentions Project Control, Craftsman Control, operations.craftsman.systems, a feature, config, off switch, Use Fallback, server override, command, `Control.Run`, the Konsole bar, an entity, reducer, Ledger store, game pass, developer product, subscription, receipt, gift, refund, or `craftsman publish`. Also use before hand-rolling a feature flag, a tunable constant, a remote admin or moderation command, a kick or ban, a DataStore or MemoryStore wrapper, a ProcessReceipt handler or an ownership check, because Project Control already provides each one and the console can only see what is declared through it. Gives the API, call syntax and silent traps. For first-time setup use setup-project.
license: MIT
metadata:
  author: averyark
  repository: craftsman-cli
  framework-version: "0.11.1"
  cli-version: "0.11.1"
---

# Craftsman Control

Project Control is a console (`https://operations.craftsman.systems`) for
running live Roblox experiences. `craftsman-control` is the game half: the
game **declares** what the console may see and act on, and the runtime reads
what the console changed. The `craftsman` CLI publishes the declaration and
builds releases.

**Declare it through Control before writing your own.** A hand-rolled flag,
admin command or DataStore wrapper works in the game and is invisible to the
console: nobody can switch it off, run it, read it or audit it without a
release.

This skill covers *what to call*. It says nothing about code style: follow the
project's own (its `CLAUDE.md`, `stylua.toml`). Examples here are style-neutral.

## Access

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea
```

- GreenTea (`gt`) is vendored in Control. Every type in a declaration is a
  GreenTea type; don't add GreenTea as a separate dependency.
- The game's declaration is the module returning `Control.Declare({...})`,
  usually `src/Control/Definitions.luau`. Find it, and the `Stores` module and
  the Script calling `Control.Start`, before changing anything.
- Control, the kit (`CraftsmanPackages/Craftsman`), Lifecycle and the
  third-party code they share (Keeper, Promise, Signal, ByteNet, Ledger,
  Konsole) are installed by `craftsman install` into `CraftsmanPackages/`,
  exactly as `craftsman.lock` names them. Control's version is always the CLI's;
  `craftsman --version` names both and refuses a mismatch. Never add any of
  them to `wally.toml`, and never edit `CraftsmanPackages/`: `install` replaces
  it wholesale. `craftsman update` moves the packages; ask before running it.

## Pick the right API

| Need | Use | Reference |
|---|---|---|
| A part of the game that can be switched off from the console without a release | `Control.Scope("Shop"):Feature({...})`, code in `Shop:OnActivate(fn(keeper))`, gates on `Shop.Active` | [features](references/features.md) |
| A tunable value (price, rate, radius, message text) | `:Config({ X = Control.Config(gtType, { Default = ... }) })`, read with `Shop.X:Get()` / `:Observe(fn)` | [features](references/features.md) |
| The same feature or value on clients | `Client = true` on the feature, `Shared = true` on the config, `Control.StartClient()` once | [features](references/features.md) |
| Declare a config or command in the file that uses it | `Feature:Declare("Name", { Feature, Config, Commands })` + `craftsman project` | [features](references/features.md) |
| An action a moderator runs on a live player or server (kick, teleport, message, effect) | `Control.Command({ params }).NoImpact().Tier("routine").Handles(fn)` under a scope's `:Commands` | [commands](references/commands.md) |
| Run that action from game code or chat for an attached admin | `Control.Run(player, key, target, params)` | [commands](references/commands.md) |
| An in-game admin command bar | `Control.Konsole.Attach(Konsole, { Declaration })` + `AttachClient` | [commands](references/commands.md) |
| Ban from inside the game | `Control.Run(admin, "craftsman.player.ban_in_game", userId, {...})` | [commands](references/commands.md) |
| "Does this player have admin X here?" | `Control.Access.CanRun(userId, key)` (never `HasPermission` for a button) | [commands](references/commands.md) |
| Saved player or world data the console can read, edit and revert | `Scope:Entity({ Store, KeyedBy, State, Default })` + reducers via `.Writes(fields).Then(fn)` | [entities](references/entities.md) |
| Open that data store and write to it | `Control.Store({ Entity, Ledger })` in the `Stores` ModuleScript; `store.Load` / `Apply` / `Edit` | [entities](references/entities.md) |
| Stream a player's data to clients | `Control.Replicate(store, share)` on the server, `Control.Replica(entity)` on the client | [entities](references/entities.md) |
| A matchmaking pool, leaderboard or queue in MemoryStore | `:Entity({ Memory = { Structure = "SortedMap" \| "Queue", Name } })` | [entities](references/entities.md) |
| Game passes, developer products, subscriptions, gifts, refunds | `craftsman store add` (ids in `Assets`), `Receipts` in `Declare`, `Control.Commerce` | [store](references/store.md) |
| An animation, sound, image or video id | `craftsman asset add`, then `Assets.Animations.<Name>` from `ReplicatedStorage.Craftsman.Assets`; never a raw `rbxassetid://` | [cli](references/cli.md) |
| "Does the player own the pass?" | `Control.Commerce.Owns(player, key)`, never `UserOwnsGamePassAsync` | [store](references/store.md) |
| Publish, build, test in Roblox, regenerate declarations | `craftsman publish`, `craftsman test-roblox`, `craftsman project` | [cli](references/cli.md) |

Read the linked reference before using an area for the first time in a task.

## Rules that apply everywhere

1. **Declaration modules run outside Roblox.** The CLI loads `Definitions` and
   every module it requires under Lune to publish them. At module scope: no
   `game`, `workspace`, `script` or `Enum`, and no `const`. Inside a handler or
   a `Then` body, `game` is fine, because it runs only in Roblox.
2. **Stores open in a ModuleScript, never a Script**, at
   `ServerScriptService.Craftsman.Control.Stores`, which returns `Control`. The
   console reads data through a Luau Execution session that requires that
   module and never runs the place's Scripts. A store opened from a Script works
   in the game and the console says the place has no store.
3. **Register command handlers before `Control.Start`.** A command that arrives
   with no handler is skipped, not refused, and expires as `failed`.
4. **A feature's setup goes in `OnActivate`, through its Keeper.** A feature
   can start and stop many times in one server. Everything it creates or
   connects goes through the `keeper` it is handed, which is cleaned when it
   switches off. Gate request-time code on `Feature.Active`.
5. **A config needs a `Default` that fits its type.** It is what the game runs
   on when Project Control is unreachable, and before the runtime starts.
6. **Dot vs colon.**

   | Call with | Functions |
   |---|---|
   | `:` | `Scope:Feature`, `:Config`, `:Commands`, `:Scopes`, `:Entity`, `:Reducers`, `:Declare`; `Feature:OnActivate`, `:OnDeactivate`, `:ObserveActive`, `:IsReady`, `:OnReady`; `Config:Get`, `:Observe`; `Feature.Heartbeat:Connect`; `session:Flush` |
   | `.` | `Control.*` (`Scope`, `Config`, `Command`, `Declare`, `Start`, `StartClient`, `Store`, `Run`, `OnCommand`, `Replicate`, `Replica`), command and reducer builders (`.NoImpact()`, `.Tier()`, `.Handles()`, `.Writes()`, `.Then()`), store handles (`store.Load`, `store.Apply`, `store.Edit`, `store.Session`), `Control.Access.*`, `Control.Commerce.*`, `entity.Writes(...)` |

7. **A reducer never reads a clock or random.** Ledger replays every stored op
   through the reducer on every read, so `os.time()` inside `Then` gives a
   different answer each time. Pass the instant in as a field.
8. **Reserved names.** Keys starting `craftsman.` are Project Control's, and
   member names starting `_` are reserved. Don't declare either.
9. **A quiet server sends nothing to Project Control.** No heartbeat, no timer
   reaching it: the pricing rests on it. A timer writing only to the game's own
   DataStore or MemoryStore is fine.
10. **Nothing secret goes in a declared name.** Feature, config and command
    names in a client-readable module can be read by players. Values of
    unshared configs never reach a client.
11. **The game key is a Roblox Secret, never a string.** It is read with
    `HttpService:GetSecret("CRAFTSMAN_CONTROL")` and goes only to
    `Control.Reporter.Start`. Never put it, or an Open Cloud key, in a file or
    `.env`, and never ask for it.

## Skeleton

```
src/
  Control/
    Definitions.luau   -- ModuleScript: return Control.Declare({...})
    Stores.luau        -- ModuleScript: opens stores, returns Control
  Features/Shop/Definitions.luau  -- the Shop feature (craftsman init's layout)
  Start.server.luau    -- Script under ServerScriptService.Craftsman
```

```luau
-- Definitions
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Shop = require("../Features/Shop/Definitions")

return Control.Declare({
	Universe = 10202097921, -- a universe id, never a place id; { a, b } for several
	EventCeiling = 500,
	SigningKey = "<public key from Settings → Signing>",
	Includes = { Shop.Definition },
})
```

```luau
-- Start.server.luau
local HttpService = game:GetService("HttpService")

local Control = require("@game/ServerScriptService/Craftsman/Control/Stores")
local Definitions = require("@game/ServerScriptService/Craftsman/Control/Definitions")

Control.Reporter.Start({
	declaration = Definitions,
	secret = HttpService:GetSecret("CRAFTSMAN_CONTROL"),
	endpoint = "https://operations.craftsman.systems/api",
	ceiling = Definitions.event_ceiling,
})

-- Control.OnCommand / .Handles registrations happen before this line
local Runtime: Control.Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
	Registry = true, -- the console's Servers page and server overrides
})
```

A client that runs `Client = true` features calls `Control.StartClient()` once
from a LocalScript, after requiring their declaration modules.

## After changing a declaration

Run `craftsman publish` (a preview: it sends nothing that changes anything and
exits 0, so read its output) and fix what it reports. A publish with `--apply`
builds a release and **activates nothing**; declarations go live when a release
is deployed from the console. Ask before `--apply`, `--activate`, a commit or a
push. See [cli](references/cli.md).
