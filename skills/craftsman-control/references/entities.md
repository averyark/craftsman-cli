# Entities, reducers and stores

An **entity** is saved data the console can read by key, edit, revert and
investigate: a player profile, an inventory, a world document, a matchmaking
pool. Its shape is declared once as a GreenTea type, and it changes only
through **reducers**. Data entities sit on Ledger (`xoifaii/ledger@5.2.1`) in a
DataStore; MemoryStore entities sit on a sorted map or a queue.

Don't write a raw DataStore wrapper for data an operator may ever need to look
at or fix. The console can only reach a store opened through `Control.Store`.

Entities are declared in `Definitions` modules, never with `:Declare` in the
file that uses them.

## Declaring a data entity

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

local Amount = gt.number({ integer = true, range = "[1, 100000]" })

local State = gt.build(gt.table({
	Coins = gt.meta(gt.number({ integer = true, range = "[0, 1000000]" }), { Currency = true }),
	Level = gt.number({ integer = true, range = "[1, 100]" }),
	Visits = gt.number({ integer = true, range = "[0, 1000000]" }),
}))

local Profile = Control.Scope("Profile"):Entity({
	Label = "Player profile",
	Store = { Name = "PlayerData" },
	KeyedBy = "Player",
	State = State,
	Default = { Coins = 100, Level = 1, Visits = 0 },
	Shared = { Level = true },
})

local Reducers = {}

Reducers.AddCoins = Profile.Writes({ Amount = Amount }).Grants("Amount").Then(function(state, fields)
	return { Coins = state.Coins + fields.Amount }
end)

Reducers.LevelUp = Profile.Writes({}).NoImpact().Then(function(state)
	return { Level = state.Level + 1 }
end)

Reducers.RecordVisit = Profile.Writes({}).Then(function(state)
	return { Visits = state.Visits + 1 }
end)

return Profile:Reducers(Reducers)
```

| Entity option | Meaning |
|---|---|
| `Store = { Name = "..." }` | The DataStore. The name is also what the console lists, so a mismatch with what the game really writes reads as an empty store, silently |
| `KeyedBy` | `"Player"` (keyed by UserId, with sessions) or `"String"` (any key, written by key) |
| `State` | `gt.build(gt.table({...}))`: the whole shape. The console's editor, params and revert are derived from it |
| `Default` | The document a new key starts from. Required for reducers |
| `Shared` | Fields other players may receive through replication |
| `Owner` | Fields the owner receives; without it they receive every field not starting `_` |
| `Migrations` | Save-data migrations. They belong here, never on `Control.Store` |
| `Label` | Name in the console |

## Reducers

`Entity.Writes(fieldTypes).Then(function(state, fields) ... end)`:

- `Then` returns **a table of the fields that changed**, or **`nil` to refuse**.
- **Never read a clock, `math.random` or anything outside `state` and
  `fields`.** Ledger replays every stored op on every read. Pass an instant in
  as a field.
- A bare `.Writes(...).Then(fn)` is **private to the game**. Chaining an impact
  (`.Grants(field)`, `.Transfers(field)`, `.Prices(field?)`, `.NoImpact()`) or
  any refinement makes it an **operation** in the console's catalogue, and then
  the impact is required.
- Refinements: `.Label(text)`, `.Named(key)`, `.Permission(key)`,
  `.Tier("routine" | "valued" | "structural")`, `.Irreversible()`,
  `.Factor(f)`, `.Approver(bool)`, `.Constrain(c)`.
- `.Grants("Amount")` names the field that hands out value, which budgets meter.
- Don't write a generic "SetField" reducer for admins: the console already gets
  a provided edit operation for every field of `State`.
- Names beginning `_` are reserved.

## Opening the store

In the **`Stores` ModuleScript** (`ServerScriptService.Craftsman.Control.Stores`),
which returns `Control`. Never in a Script: the console's sessions require this
module and run no Scripts.

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Ledger = require("@game/ReplicatedStorage/CraftsmanPackages/Ledger")
local Player = require("./Player")

local profiles = Control.Store({
	Entity = Player.Profile,
	Ledger = Ledger,
	-- OnApplied = function(key, kind, fields, state, opId) end,
	-- OnLoadFailed = function(player, reason) return keepThem end,
})

return Control
```

The module that opens a store can also export the handle for game code, or the
game reaches it through the entity's reducers, below.

## Writing

A player-keyed store holds a **session** per player:

```luau
Players.PlayerAdded:Connect(function(player)
	profiles.Load(player) -- yields
	if profiles.Session(player) == nil then
		return -- the load failed
	end
	Player.Profile.RecordVisit(player, {})
end)

Players.PlayerRemoving:Connect(function(player)
	profiles.Unload(player)
end)
```

- `Entity.Reducer(target, fields) -> (ok, reason?, detail?)` calls a reducer by
  name. On a player-keyed entity the target is the Player and it applies to
  their session; on a string-keyed or MemoryStore entity it is the key and it
  writes through.
- The same through the handle: `store.Apply(player, "AddCoins", fields)` (a
  session) and `store.Edit(key, "AddCoins", fields)` (by key, durable once it
  returns true). `store.Read(key)` reads a document.
- `reason` is Ledger's word (`Refused`, `Unresolved`, `Busy`...). `Unresolved`
  means it may have landed: never retry a grant on it blindly.
- **A console edit lands in the DataStore**, and a server holding that player's
  session sees it only at the session's next save. Call
  `profiles.Session(player):Flush():Wait()` when told an edit landed
  (`OnApplied`, or a MessagingService nudge) to read it back.

## Replicating to clients

Data entities only. On the server, after the store opens:

```luau
Control.Replicate(profiles, {
	Level = function(subject: Player, viewer: Player)
		return subject.Team == viewer.Team
	end,
})
```

The share table names exactly the entity's `Shared` keys. A player-keyed store
also streams each player's own document to them (`Owner` narrows it; pass
`{ Owner = false }` as a third argument to stop it). A string-keyed store
streams a key only after `Share(key)`.

On the client, require the same declaration:

```luau
local replica = Control.Replica(Player.Profile)

replica.Observe().Subscribe(function(state)
	label.Text = `Coins: {state.Coins}`
end)

replica.Select(function(state) return state.Coins end).Subscribe(PlayCoinSound)

replica.Of(other.UserId).Observe().Subscribe(function(shared)
	nameplate.Text = `Lv {shared.Level}`
end)
```

Don't build a separate RemoteEvent for this data.

## MemoryStore entities

```luau
local Pool = Control.Scope("Pool"):Entity({
	Label = "Matchmaking pool",
	Memory = { Structure = "SortedMap", Name = "MatchmakingPool" }, -- or "Queue"
	State = State,
	Default = { Region = "none", Skill = 0, QueuedAt = 0 },
})

local Enter = Pool.Writes({ Region = Region, Skill = Skill, QueuedAt = QueuedAt }).Then(function(_, fields)
	return { Region = fields.Region, Skill = fields.Skill, QueuedAt = fields.QueuedAt }
end)

return Pool:Reducers({ Enter = Enter })
```

```luau
-- in Stores; no Ledger
Control.Store({
	Entity = Pool,
	Expiry = 600,
	SortKey = function(state) return state.Skill end, -- sorted map only
})
```

- No hash maps (Open Cloud cannot read them). No history, so no revert.
- A sorted-map key is at most 127 characters. A queue reducer takes no key and
  must be `.Irreversible()`.
- Reading a queue from the console hides its items from the game for a while,
  so the console treats it as an operation, never a browse.
- MemoryStore entities do not replicate.
