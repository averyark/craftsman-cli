# Review a game's code

Code that passes review and still costs the game something. Each detector says what to search for, why it matters, and what is **not** a match, because a false finding wastes more time than a missed one.

Answer the question asked. Asked about one file, review that file. Propose, and do not rewrite code nobody asked about. Moving live saves is a decision for the person.

## Written by hand, invisible to the console

| Search for | Why it matters | Use | Not a match |
|---|---|---|---|
| `local ENABLED = true`, a tunable constant | Nobody can change it without a release | A feature, a config | A true constant, such as a physics value nobody would ever tune |
| A list of admin UserIds | A second list of who may act, with no audit | Roles, `Control.Access.CanRun` | A dev only check in Studio |
| `:Kick(` behind an admin check | Unaudited, and no reason recorded | A command | A kick that is the game's own rule, such as an anti-cheat reaction |
| `DataStoreService`, `GetDataStore` | The console can never read or erase it | A ledger entity | Data nobody will ever look at or fix, with no player data in it. Ask |
| `MemoryStoreService` | The same | A MemoryStore entity | A hash map, which Open Cloud cannot read |
| `ProcessReceipt`, `UserOwnsGamePassAsync`, `PromptGamePassPurchase` | Awards, gifts, revokes and refunds cannot reach it | `Receipts`, `Control.Commerce` | A game not using `Control.Commerce` by decision |
| `rbxassetid://` | The id is locked in code | `Assets`. `craftsman check` warns on it | An engine default, or a test fixture |
| A RemoteEvent that sends a player's data | A second path for what replication gives | `Control.Replicate` | An event, rather than state |
| A loader that requires a folder | No order, no failure isolation | Lifecycle | A loader for something that must not be a singleton |
| `CollectionService` for status effects | No stacks, expiry or attributes | `Tag` | Tagging for an engine feature or a plugin |
| `task.wait` as a cooldown, a table of last used times | Leaks, and resets wrongly | `Concurrency.KeyedDebounce` | A one off delay |
| `UserInputService.InputBegan` for game actions | No rebinding, and touch needs a second path | `InputUtil`, `TouchUtil` | Reading raw input for a real reason, such as a text field |
| `TweenService:Create` repeated with one shape | No shared templates, and overlaps cancel badly | `TweenUtil` | A one off tween |

## Declarations

| Search for | The bug | Not a match |
|---|---|---|
| `Control.Command(` with no `.Tier(` | It defaults to `structural`: a passkey, 15 minutes and never in game. A moderator cannot use it quickly | A command that really should wait |
| `.Tier("routine")` on something that hands out value, whether marked `.Grants`, `.Transfers` or `.NoImpact()` | A real grant with no budget, because a budget is only checked at `valued` | `.Tier("valued")` with `.Grants` or `.Transfers` |
| A handler with a path that returns nothing | From the console, a live command expires `failed`. A durable one does not settle | An `error()` path, which reports as refused |
| A `Default` the game could not really run on | The fallback and an outage both land there | A `Default` that is a safe off state |
| A boolean config named `Enabled` | The feature's `Active` already is that | A boolean that is not on or off for the feature |
| A shared `gt.meta` type | It mutates the type, so labels cross over | A type used once |
| A secret in a name, or a `Shared` config | Players can read them | A name with nothing in it |
| A renamed feature, config or entity | A rename is a new key, and the hosted value stays behind | A brand new one |
| Module scope reading `game` | It stops the publish | Only naming `game`, or reading it inside a handler |
| A setup outside `OnActivate` | It survives a switch off | Setup with no effect on players |
| A connection in `OnActivate` that skips `keeper` | It leaks, and the feature runs after it is off | One tied to something the keeper already cleans |
| Request time code with no `Feature.Active` check | The feature is off, and its remote still works | Code inside `OnActivate` |

## Data

| Search for | The bug | Not a match |
|---|---|---|
| `os.time()`, `tick()`, `math.random` inside `.Then(` or `Apply` | A reducer must decide only from `state` and `fields` | A time passed in as a field |
| A `Then` that returns the whole state | It should return the fields that changed | A reducer that really changes them all |
| A `Then` with `nil` meant as "clear this" | `nil` refuses. Clear with `Control.None` | A real refusal |
| A generic `SetField` reducer | It bypasses the rules, and the console already has an edit | A named operation with a real rule |
| A retry around a grant on `Unresolved` | It may have landed, so a retry pays twice | A retry that resends the same id. See the Ledger skill |
| `Control.Store` in a Script | The console cannot see it | One in `Stores` |
| A `State` change with no new migration | The release reports it, and a person decides | A change to a comment |
| A `State` field in neither `Default` nor a migration's `Fields` | The declaration errors | An optional field named in `Fields` |
| `:Flush():Wait()`, or `OnLoadFailed` that returns a value | Written for an older Ledger. `Flush` answers `(ok, reason, info)`, and having a handler at all stops the kick | |
| A removed or retyped field with no `IsBreaking = true` | Older servers cannot read it | A step naming only added fields in `Fields` |
| An edited or deleted shipped migration | It breaks every document past it | A new one added at the end |
| A grant that does not stamp `PurchaseId` | `Reverse` then removes "one of these" | A plain currency amount |
| `Owner` missing where `_` fields are not enough | The owner receives every field not starting `_` | Fields the player may see |
| A queue handler slower than `Invisibility` | A late `Discard` answers `false` with `Expired`, and the item returns | A handler safe to repeat |

## Kit and Lifecycle

| Search for | The bug | Not a match |
|---|---|---|
| A `:Connect` with no owner | It leaks | One through a `Keeper` |
| A yield in `Init` | It holds up every module | A yield in `Start` |
| A game module required but not in `Dependencies` | No order, and a failed module is not skipped | A plain helper that is not a singleton |
| A non singleton in a loaded folder | The loader runs anything with `Init` or `Start` | A folder marked `CraftsmanLifecycleIgnore` |
| `WorldUtil.` with a dot | The arguments shift | `WorldUtil:` |
| `Pathfind` on a player character | It takes network ownership | An NPC |
| `Pathfind:Destroy()` expecting a halt | It does not call `Stop` | `Stop` first |
| `entity.Died` | Signals are on the module: `Entity.Died` | |
| `Entity.Get` on a body that may be dead | It makes a new wrapper | A check of `Entity.Entities[model]` first |
| `Machine:Update` never called | Nothing drives it | A loop that does |
| A machine replicated with nothing that ends it | It leaks | `Unreplicate` or `Destroy` from an owner's Keeper |
| `Tag` removed from a client | A client cannot remove a server tag | A tag made on that client |
| A `Queue` promise with no `:catch` | Promise warns, and the failure is lost | One returned to a caller who catches it |
| A table or Instance key in `Concurrency` | Keys are weak, so it can reset | A string, number or Player |
| `Animate:Destroy()` on a shared `Animate` | The cache keeps the dead one | One from `.new` |

## Operating it

| Search for | The bug | Not a match |
|---|---|---|
| A timer or heartbeat reaching Project Control | A quiet server must send nothing | A timer writing only to the game's own storage |
| `Control.OnServerMessage` drawing text as is | It is not passed through Roblox's text filter | Text a person typed in the console, shown knowingly |
| An uncommitted `craftsman.lock` or `src/Control/Assets.luau` | CI cannot build what you ran | A gitignored `CraftsmanPackages` |
| A player's data in a store no manifest declares | Erasure cannot reach it | Data with no player in it |
| `SigningKey` missing | A server accepts unsigned commands | A game before its first publish |

## Report

For each finding: the file and line, what happens to whom, the fix, and how sure you are. If you did not run it, say so. Most severe first, and stop at what matters.
