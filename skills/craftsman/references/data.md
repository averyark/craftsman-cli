# Data: entities, migrations, MemoryStore, history

## What declaring a ledger entity gives you

With no extra code, the console gets a reader for one document at a time, a
provided edit (`craftsman.data.edit.<entity>`), a timeline, and revert. So:

- **Don't write a generic "set field" reducer.** The provided edit covers the
  whole of `State`, at `structural`.
- **Write a named reducer for each thing people do often**, such as `add_coins`,
  at a lower tier and with a real impact.
- **Reads start from a key.** There is no search across players, so don't design
  anything that needs one.

## Clearing a field

A `Then` that returns `nil` is a refusal, and a `nil` value can't clear a field.
Use `Control.None`:

```luau
local Clear = Profile.Writes({}).Then(function(_, _)
	return { Note = Control.None }
end)
```

`Note` must still be in `Default` or a migration's `Fields`.

A reducer must also never read a clock or random, because Ledger runs it again
whenever it judges an op on a newer saved state. Pass the instant in as a field.

## Migrations

```luau
Migrations = {
	{ Run = function(state) return next end, Fields = { "Gems" } }, -- adds fields
	{ Run = function(state) return next end, IsBreaking = true }, -- removes or retypes
},
```

- Declared on the entity. `Control.Store` refuses them, and only a ledger entity
  takes them.
- Each step names its `Fields` or says `IsBreaking = true`, never both. Any other
  key errors.

A release compares each entity with the last deployed release, and these are
problems it reports:

| Change | Needs |
|---|---|
| Any change to `State` | A new migration, naming any added field in `Fields` |
| A field removed or retyped | A new migration with `IsBreaking = true`, because an older server can't read it |
| A deployed migration deleted, or its `IsBreaking` or `Fields` changed | Don't. Add a new one |

A breaking step is a breaking change, because older servers can no longer read
those documents. A `Fields` step adds fields only, and Ledger lets an older
server keep working because the step names them.

A deploy with a save-data change takes a DataStore snapshot first. The key needs
  `universe-datastores.control:snapshot`, so the first one may stop and name it.
  Roblox allows one snapshot a day per experience.

Say which it is in your change, because a person decides whether to deploy.

## The player roster

`Control.Metadata({ Ledger = Ledger })` in the `Stores` ModuleScript keeps first
and last seen for each player, written once per join. It's the console's own
roster of players, and `craftsman init` doesn't write it. Nothing may edit it.

## MemoryStore

Reads go through the console, and writes **only through the entity's reducers**.
No history, so no timeline or revert. No hash maps.

```luau
local pool = Control.Store({
	Entity = Pool,
	Expiry = 300,                               -- required; 1 to 3888000 seconds
	SortKey = function(state) return state.Skill end, -- sorted map
})
pool.Remove(key)                                -- sorted map

local queue = Control.Store({
	Entity = Jobs,
	Expiry = 3600,
	Invisibility = 30,                          -- a Take hides items this long
})
local items, readId = queue.Take(10)
queue.Discard(readId)
```

Traps, each measured on Roblox:

- **A queue read from the console hides its items from the game** for the
  invisibility window, so it is an operation there, never a browse.
- **A `Discard` after that window answers `false` with `Expired`, and the item
  returns.** Roblox itself answers success and does nothing, and Control catches
  it. Keep work shorter than `Invisibility`, and make handling safe to repeat.
- `Unresolved` from a queue add means it **may have landed**. Never retry blindly.
- An entity with no `Default` has no operations, and is read-only in the console.
- Names starting `Craftsman` or ending `_CraftsmanApplied` are reserved.
- An entity opens once per server.

## Replication and privacy

`Control.Replicate(store, share)` sends the owner every field not starting `_`
**unless `Owner` says otherwise**. Keep anti-cheat scores and the like out with an
`Owner` list, or an underscore name.

- `Replica:Get()` is `nil` until the first snapshot arrives.
- `Of` stays `nil` for a viewer who is refused.
- A watched key is at most 50 bytes, with at most 64 per viewer.

## Load failures

A store that can't load a player kicks them by default, which protects their save.
Change it with `OnLoadFailed`, only if you've thought about what they then
overwrite.

## History

The timeline, revert and the **Investigate** card are in the console, and an agent
hands those to a person. Tell them, or design for, these limits:

- A real DataStore keeps **one version per key per UTC hour**. The newest write in
  an hour replaces that hour's version, so several operations can share one.
- A version id isn't stable until its hour ends.
- History lists a key's whole 30 days of saves.
- Revert writes a new edit under the same `structural` gates, so nothing is
  rewritten and nothing is undone without a person.
- A log entry can't be removed. A wrong grant is corrected by another operation.

Right to erasure runs from the console: a verified Roblox request deletes the
player's key from every store a manifest declares as keyed by player, seven days
later. So keep a player's data in entities keyed by `Player`. A store nothing
declares can't be erased for them.
