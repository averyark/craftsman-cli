---
name: craftsman
description: Expert help for a Roblox game built on Craftsman and Project Control. Use whenever a project has a `craftsman.toml`, `craftsman.lock`, `CraftsmanPackages`, a `Control.Declare` module or the `craftsman` CLI, or the user mentions Project Control, Craftsman, operations.craftsman.systems, Craftsman Kit or Lifecycle. Covers setting a game up, planning a system, features and configs, Use Fallback and server overrides, commands, `Control.Run`, the Konsole bar, entities, reducers, Ledger stores, MemoryStore, migrations, game passes, developer products, receipts, gifts, refunds, assets, releases, in-Roblox tests, the console's pages, and the kit (Entity, Tag, StateMachine, Pathfind, Queue, Debounce, Concurrency, TweenUtil, Spring, SoundUtil, AnimationUtil, InputUtil, TouchUtil, InterfaceUtil, WorldUtil, MathUtil, StringUtil, TableUtil, Inspect) and Lifecycle (Init, Start, Stop, Dependencies, Keeper). Also use before writing a feature flag, a tunable constant, an admin or moderation command, a kick or ban, a DataStore or MemoryStore wrapper, a ProcessReceipt handler, an ownership check, a raw asset id, a module loader, a cooldown, a tween helper or an input handler, because Craftsman already provides each one and the console can only see what is declared through it. Also use to review Craftsman code, to diagnose something the console cannot see, and when a change touches saved data.
license: MIT
metadata:
  author: averyark
  repository: craftsman-cli
  cli-version: "0.11.9"
  control-version: "0.11.9"
  kit-version: "0.10.1"
  lifecycle-version: "2.0.0"
  ledger-version: "7.0.5"
---

# Craftsman

Craftsman is how a Roblox game gets run live. Four packages go in the game: **Control** declares what the console may see and act on, **Lifecycle** loads the game's modules, the **kit** is a framework of entities, state machines, pathfinding and utilities, and **deps** holds the third party code they share, Ledger included. **Project Control** is the console at `https://operations.craftsman.systems`, and the `craftsman` CLI joins the two.

**The rule of this skill: use the whole product.** What is declared through Control can be switched off, changed, run, read, reverted and audited by a person, with no release. What is written by hand works in the game and is invisible to the console. Before you write a flag, an admin list, a data wrapper, a loader, a cooldown or a tween helper, check the table below.

**This file is judgement, not reference.** The references hold the API. Do not state a signature, a default or a limit from memory. Read it, then answer.

## Three checks, before anything else

**Is this project on Craftsman?** Look for `craftsman.toml`, `craftsman.lock`, a `CraftsmanPackages` folder, `Control.Declare`, or `craftsman` in `rokit.toml`. If none is there and they want it, go to `references/setup.md`. If none is there and they did not ask, say so and stop: nothing here applies to another framework.

Someone **deciding whether to adopt it** gets an honest answer: every player question starts from a key because there is no search across players, a command is live for two minutes unless it is durable, saved data needs Ledger, and somebody must hold a passkey to deploy to a protected group.

**Is this Craftsman itself?** A tree holding `src/Declare.luau` with `.lune/craftsman.luau` is `craftsman-control`. `src/StateMachine/` with `craftsman-package.toml` is the kit. `src/init.luau` with `craftsman-package.toml` naming `lifecycle` is Lifecycle. Those repositories have their own briefs, so stop and follow them. This skill assumes you may not change the packages, and `CraftsmanPackages/` is never edited: `craftsman install` replaces it whole.

**Which version?** Run `craftsman --version`, and read `craftsman.lock`. This file is pinned to the versions above. Then act on it:

- **The same.** Answer normally.
- **Newer than this file.** Say so, and check claims against the source in `CraftsmanPackages/`.
- **Older.** `craftsman doctor --offline` names what is wrong. See `references/updating.md`. Signs: `Control.Flag`, `:Flags`, `Control.FlagService`, a `wally.toml` naming Craftsman, `Craftsman.Component`, `ReplicatedStorage.CraftsmanConfig`, a lock that names a bundle.
- **Nothing to read.** Say so once, and treat every number here as unverified.

## The reference lives here

Three copies, in the order to reach for them.

1. **`references/docs-bundle.md`**, the whole documentation in one file, about 8,500 lines. Search for `## Page: ` to list its pages. Search it, do not read it through. It is here so an agent with no web access still has it.
2. **The source**, in `CraftsmanPackages/`. When it is there, it is the truth. `references/verify.md` says which file answers which question.
3. **The site**, if you can fetch: `https://operations.craftsman.systems/llms.txt` lists every article as markdown. A fetch of `/docs/...` itself returns an empty shell.

When the docs and the source disagree, the source wins, and you say so. If you can reach neither, answer and say you could not check it, naming the page that would settle it.

Ledger has its own skill. For anything past `Control.Store`, such as a reason, a trade or a lost balance, tell the person to install it if they have not, and use it:

```bash
npx skills add XoifaiI/Ledger
```

## Use the whole product

Walk this for any new system, before you write code.

| Question | Use | A person then can | Read |
|---|---|---|---|
| Could this ever need switching off? | A feature's `Active` | Switch it off from the console | `features.md` |
| Is a number or a text a guess? | A config with a `Default` | Change it live, or hold it on `Default` | `features.md` |
| Will somebody act on it live? | A command | Run it from the console or the Konsole bar, audited | `commands.md`, `operations.md` |
| Must it wait for an offline player? | `.Durable()` | Queue it for their next join | `operations.md` |
| Does it save data? | A ledger entity with reducers | Read, edit, revert and investigate one document | `entities.md`, `data.md` |
| Is the data short lived? | A MemoryStore entity | Read it, and change it through reducers | `data.md` |
| Does a client need that data? | `Control.Replicate`, `Control.Replica` | Nothing, and you write no RemoteEvent | `entities.md` |
| Does it sell something? | `craftsman store`, `Receipts`, `Control.Commerce` | Award, gift, revoke and handle refunds | `store.md` |
| Does it use media? | `craftsman asset`, `Assets` | Replace or roll back, with ids out of code | `cli.md` |
| Can a change corrupt saves? | `Migrations` on the entity | Read a save data check, and get a snapshot before deploy | `data.md` |
| How do you know it works in Roblox? | `craftsman test-roblox` | Require a pass before a deploy | `shipping.md` |
| Do its modules need an order? | Lifecycle, `Dependencies`, `Keeper` | Nothing | `lifecycle.md` |
| Does a character have states? | `Entity`, `State`, `Machine`, `Tag` | Nothing | `kit-entities.md`, `kit-state.md` |
| Does an NPC move? | `Pathfind` | Nothing | `kit-entities.md` |
| Must jobs not overlap? | `Queue`, `Concurrency`, `Debounce` | Nothing | `kit-tasks.md` |
| Does it animate, or play sound? | `TweenUtil`, `Spring`, `AnimationUtil`, `SoundUtil` | Nothing | `kit-animation-audio.md` |
| Does it take input, or draw UI? | `InputUtil`, `TouchUtil`, `InterfaceUtil` | Nothing | `kit-input-ui.md` |
| Casts, math, strings, tables? | `WorldUtil`, `MathUtil`, `StringUtil`, `TableUtil` | Nothing | `kit-world-data.md` |

## How the pieces join

A game's feature is one folder, and each package does one job:

- **Control** declares it in `Definitions`, and answers whether it should run now.
- **Lifecycle** loads its module, and answers whether its code is wired up. That is once, at boot.
- **The kit** gives it entities, tags, machines and helpers.

So a module registers with Control in `Start`, and works in the handler. Switching the feature off in the console then tears it down:

```luau
local Grass = {}

function Grass:Start()
	self.Keeper:Add(Definitions:OnActivate(function(keeper)
		-- Everything made here goes through keeper, cleaned at each switch off.
	end))
end

return Grass
```

Never load `ServerScriptService.Craftsman.Control` with Lifecycle. `Stores` must open when it is required, because the console's sessions run no Script.

## What each action touches

Classify by the command, not by your intent.

| Action | Leaves this machine? | Do |
|---|---|---|
| `craftsman whoami`, `status`, `doctor`, `check`, `project`, `release`, `publish` with no flags | Reads only, or writes local files | Run it |
| `craftsman asset` or `store` with `--dry-run` | No | Run it |
| `craftsman install` | Downloads what the lock names, or with no lock the newest set, then writes the packages, lock, `craftsman.toml` and project files locally. `--frozen` writes only the packages | Run it |
| `craftsman update` | Changes what the game runs | **Ask** |
| `craftsman test-roblox` | Runs on a real Roblox server, from a budget of five a minute shared with everyone on the project | **Ask** |
| `craftsman asset` or `store` writes | Writes to Roblox. Spends quota, a video costs 2,000 Robux, and a pass or product is never deleted | **Ask, every time** |
| `craftsman publish --apply` | Builds from what is **pushed**, and activates nothing, unless an open group has auto deploy on | **Ask** |
| `publish --group G --activate --no-build --apply` | Changes live declarations with no release | **Ask, every time** |
| A commit or a push | Outward | **Ask** |

## What only a person can do

Hand these over as exact steps. `references/console.md` has them.

- Tap a passkey, including for `craftsman login`. Ask them to run it: `! craftsman login` in Claude Code.
- Deploy a release, switch a feature, Use Fallback, edit or revert a document, run or approve an operation.
- Add members, and give roles. Craftsman Staff add members directly, and Staff or a project's Superpower can invite with `/onboard invite`.
- Save the game key in Roblox's Secrets, and the Open Cloud key in the console.

Nothing in the CLI reads a player's document or runs a command. If you need one read, ask the person to open it in the console and paste what it shows.

## Stop before these

A migration of live saves, a change to `State`, a new `Store.Name`, a renamed feature or config key, a renamed store key, or anything that changes where existing documents live.

1. Say what happens to existing documents and hosted values.
2. Say whether the change is compatible with servers still running.
3. Get a yes for this change.

A rename is a new key: its hosted value does not follow, and a new store name reads as an empty store with no error.

## Prove it

Do not reason about what a declaration does when you can ask. Stop at the first rung that settles it.

1. Read the source. `references/verify.md` says which file.
2. Run `craftsman doctor`, `check` and `publish`. They read only.
3. With permission, run `craftsman test-roblox`.
4. Hand over a snippet to paste, and ask what it printed.
5. Say you do not know, and name what would settle it.

Never skip from 1 to 5. For anything Ledger itself decides, its skill describes a mock.

## Code is not data

The stop rules are about live data and live games, not code. Asked to fix a reducer, fix the reducer. Do not edit files nobody named, do not tidy beside the thing you were asked to fix, and follow the project's own style, not the snippets here.

## Where to look

Answer the question asked. Load one of these, not all of them. Files are in `references/`. A `/docs/` path is a page in the bundle.

| The person says | Go to |
|---|---|
| "set up", "install", "onboard", a publish that fails on a new game | `setup.md` |
| "what can this do", "plan this system" | The table above, then `operations.md`, `data.md`, `shipping.md` |
| "add a feature", "a config", "an off switch" | `features.md` |
| "an admin command", "kick", "ban", "Konsole" | `commands.md`, `operations.md` |
| "which tier", "why can't a moderator run it" | `operations.md`, `/docs/concepts/tiers` |
| "save data", "reducer", "a store" | `entities.md`, `data.md` |
| "a migration", "add a field", "save data check" | `data.md` |
| "a queue", "a leaderboard", "matchmaking" | `data.md`, `/docs/players-and-data/memorystore-entities` |
| "a pass", "a product", "receipt", "refund", "gift" | `store.md`, `/docs/store-and-purchases/handle-refunds` |
| "an animation id", "upload an asset" | `cli.md`, `/docs/assets/assets` |
| "release", "deploy", "CI" | `shipping.md`, `/docs/releases/deploy-a-release` |
| "tests in Roblox" | `shipping.md`, `/docs/releases/test-a-build` |
| "`craftsman.toml`", "feature folders" | `shipping.md`, `/docs/project-setup/craftsman-toml` |
| "update the packages" | `updating.md` |
| "the module loader", "Init", "Start", "cleanup" | `lifecycle.md` |
| "a character", "status effect", "stun" | `kit-entities.md`, `kit-state.md` |
| "NPC", "pathfinding" | `kit-entities.md` |
| "cooldown", "retry", "debounce" | `kit-tasks.md` |
| "tween", "spring", "sound", "animation" | `kit-animation-audio.md` |
| "keybind", "mobile button", "UI scale" | `kit-input-ui.md` |
| "raycast", "lerp", "string helper" | `kit-world-data.md` |
| "how do I do this in the console" | `console.md` |
| "it is not showing", "it did nothing", "failed" | `triage.md` |
| "the game is on fire", "turn it off" | `console.md`, `/docs/emergencies/turn-a-feature-off` |
| "a player's data went wrong" | `console.md`, then the Ledger skill's forensics, before any change |
| "delete a player", "GDPR", "erasure" | `/docs/privacy/erasure-requests` |
| "review this", "audit this game" | `review.md` |
| you are about to claim something | `verify.md` |

## How an AI gets this wrong

These are a model's mistakes, not a person's. Check your answer against them.

- **Writing it by hand.** `DataStoreService`, `MemoryStoreService`, `ProcessReceipt`, `UserOwnsGamePassAsync`, `:Kick()` behind an admin list, a tunable `local`. Each works, and each is invisible to the console.
- **Writing an older Craftsman.** `Control.Flag`, `:Flags`, `killed_value`, `Control.Catalogue`, `Craftsman.Component`, requires from `ReplicatedStorage.Packages`, Wally lines. None of these exist now.
- **Writing an older Ledger.** `:Wait()` on an answer, an op log, a migration of `{ Apply, Compatible }`. Control 0.11.8 moved to Ledger 7, and older posts and memory are mostly Ledger 5. A migration is `{ Run, Fields }` or `{ Run, IsBreaking = true }`.
- **Treating `publish` exit 0 as passed.** A preview exits 0 and lists problems. Read it.
- **Treating `publish --apply` as live.** It builds a candidate. A person deploys it, unless an open group has auto deploy on.
- **Opening a store in a Script.** It works in the game, and the console says the place has no store.
- **Reading `game` at module scope in a declaration.** The CLI loads it outside Roblox. Naming it is fine, and reading it stops the load.
- **Leaving a command at its default tier.** A command defaults to `structural`: a passkey, 15 minutes and never in game. A moderator's kick needs `.Tier("routine")`.
- **Reading `failed` as "it did not happen".** `refused` is a decision. `failed` means nobody knows. Never retry a grant on it.
- **A handler with a path that returns nothing.** From the console, a live command then expires `failed`, and a durable one does not settle.
- **Adding a timer that reaches Project Control.** A quiet server must send it nothing. A timer writing only to the game's own storage is fine.
- **Calling with the wrong punctuation.** `WorldUtil:RayBelow`, but `MathUtil.Lerp`. The wrong one shifts the arguments.
- **Quoting a number from memory.** Read it.

## Never

Say it once, plainly, then do the work.

- Never ask for, read, store or repeat the game key or an Open Cloud key. If one is pasted, say it does not belong here, and suggest rotating it in the console.
- Never put a secret in `craftsman.toml`: `Settings.luau` ships to every client. Never put one in a feature, config or command name, which players can read.
- Never read a clock, `math.random` or anything outside `state` and `fields` in a reducer. Ledger runs it again on newer saved state. Pass the instant as a field.
- Never declare a key starting `craftsman.`, or a member starting `_`.
- Never call `Schema`, `Manifest`, `Resolve`, `Directives.Apply`, `Endpoint`, `Read`, `History`, `Version` or `CommerceApply` from game code, or `craftsman package` in a game. They skip the gates and the audit.
- Never edit `CraftsmanPackages/`, `Definitions/Generated.luau` or a generated project file. Run `craftsman project`.
- Never add a Craftsman package, Ledger or Keeper to `wally.toml`. Two copies of Ledger can open one key twice.
- Never write a raw `rbxassetid://` or a catalogue id into code. Use `Assets`.
- Never rename a store key or a pass key. Players' records are filed under it.
- Never edit, delete or reorder a deployed migration. Add a new one.
- Never write a generic "set field" reducer. The console already has an edit for every field.
- Never add a heartbeat to Project Control.
- Never invent a method. The references and the source list them all.

## What this is checked against

CLI and Control 0.11.9, kit 0.10.1, Lifecycle 2.0.0 and Ledger 7.0.5, on 2026-11-02. If a check in `references/verify.md` fails, the source is right and this file is stale, and you say so.
