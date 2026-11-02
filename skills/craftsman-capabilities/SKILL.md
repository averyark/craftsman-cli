---
name: craftsman-capabilities
description: Use when planning or building a whole system on Project Control, when asked what craftsman-control can do or how to get the most out of it, or when auditing a game for hand-rolled code the console could run instead. Also use for anything past basic declaration - choosing a command's tier, delivery or per-server reach, Use Fallback and server overrides, entity migrations and save-data checks, clearing a field with Control.None, the player roster and Control.Metadata, server messages and the Servers page, MemoryStore queues and sorted maps, an edit, revert or history of a player's data, in-Roblox tests and `craftsman test-roblox`, release changelogs, `craftsman status`, `doctor` and `check`, or `craftsman.toml`. For exact declaration syntax use craftsman-control; for first-time setup use setup-project.
license: MIT
metadata:
  author: averyark
  repository: craftsman-cli
  framework-version: "0.11.9"
  cli-version: "0.11.9"
---

# Craftsman capabilities

`craftsman-control` says how to declare things. This skill says what is possible,
which capability fits, what each one costs a person at run time, and what fails
silently. Read `craftsman-control` too when you write the code.

## Plan with the whole product

For any new system, walk this list before writing code. Each row is something
the game gets without building it, as long as it is declared.

| Question | Capability | Then a person can |
|---|---|---|
| Could this ever need switching off? | A feature's `Active` | Switch it off from the console, with no release |
| Is any number or text a guess? | A config with a `Default` | Change it live, and hold it on `Default` with Use Fallback |
| Will a moderator or developer act on it live? | A command | Run it from the console, the Konsole bar or game code, audited |
| Must the action wait for an offline player? | `.Durable()` | Queue it for the player's next join |
| Does it save data? | A ledger entity with reducers | Read, edit and revert one document, and see its timeline |
| Is the data short-lived (a pool, queue or leaderboard)? | A MemoryStore entity | Read it, and change it through reducers |
| Does it sell something? | `craftsman store` and `Receipts` | Award, gift, revoke and handle refunds |
| Does it use media? | `craftsman asset` | Replace or roll back an asset, with ids kept out of code |
| Can a change corrupt saves? | `Migrations` on the entity | Get a save-data comparison and a snapshot before deploy |
| How will you know it works in Roblox? | `craftsman test-roblox` | Run the specs on a real server before a deploy |

[operations](references/operations.md) covers commands, tiers, delivery and live
brakes. [data](references/data.md) covers entities, migrations, MemoryStore and
history. [shipping](references/shipping.md) covers the CLI, tests, changelogs and
`craftsman.toml`.

## Adopting it in an existing game

Search for these and propose the replacement. Don't rewrite code nobody asked
about.

| Found in the game | Replace with |
|---|---|
| `local ENABLED = true`, or a tunable constant | A feature and config |
| A hard-coded admin list | Roles in the console, checked with `Control.Access.CanRun` |
| `:Kick()` or a ban behind an admin check | A command, or `craftsman.player.ban_in_game` |
| `DataStoreService` for player data | A ledger entity |
| `MemoryStoreService` | A MemoryStore entity |
| `ProcessReceipt` or `UserOwnsGamePassAsync` | `Receipts` and `Control.Commerce.Owns` |
| `rbxassetid://` in code | `Assets` |

A migration of live saves is a decision for the person, not a tidy-up. Say what
happens to existing documents before changing where they live.

## What an agent can and cannot do

| An agent can | Only a person can |
|---|---|
| Write declarations, handlers and specs | Tap a passkey, including for `craftsman login` |
| Run `publish`, `status`, `doctor`, `check` and `project` | Deploy a release from the console |
| Run `asset` and `store` with `--dry-run` | Use Fallback, feature switches, edits and reverts in the console |
| Run `release`, which sends nothing | Add members (Craftsman Staff), assign roles, approve an operation |

Ask before anything that leaves the machine: `publish --apply`, `--activate`, an
`asset` or `store` write, `test-roblox`, a commit or a push. Never handle the
game key or an Open Cloud key. Nothing in the CLI reads a player's document or
runs a command: those are the console's, so hand the person the step.

## Don't call these

`Control` also exports its own internals: `Schema`, `Manifest`, `Resolve`,
`Directives.Apply`, `Endpoint`, `Read`, `History`, `Version`, `Confirm`,
`CommerceApply`, and `package` on the CLI. They exist for the console, the CLI
and Craftsman's own repositories. Declaring is the interface, and using them
from game code skips the gates and the audit.

## Check your work

1. `craftsman project`, if a `:Declare` call or a feature folder changed.
2. `craftsman check`, then `craftsman doctor`.
3. `craftsman publish`, a preview that exits 0 even when it lists problems, so
   read it.
4. `craftsman test-roblox`, with permission, if the game has specs.
5. A changelog beside the change, if the game keeps them.
