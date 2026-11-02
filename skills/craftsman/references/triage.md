# Symptom to cause

Most Craftsman failures are silent: the game runs, and the console cannot see it. Run `craftsman doctor` first, then `craftsman status`. They read only, and name many of these.

## The console cannot see it

| Symptom | Likely cause | Settle it |
|---|---|---|
| The console says the place has no store | The store opens in a Script, not the `Stores` ModuleScript. A Luau Execution session runs no Script | Read `ServerScriptService.Craftsman.Control.Stores`. It must be a ModuleScript that returns `Control` |
| An entity lists nothing | `Store.Name` differs from what the game writes, or nothing has written yet. A store exists only once written | Compare the declaration with `Control.Store`, then write one key |
| A new feature or command is not listed | The manifest active in that group is older | `craftsman status`, then deploy a release, or `publish --group G --activate --no-build --apply` with permission |
| Nothing declared in a group | No release was deployed there, and a new group declares nothing until something is activated | `craftsman status` |
| No **Servers** rows | `Registry = true` is missing, or no server is up. An empty registry looks the same either way | Read `Control.Start`'s options |
| The Account row has no roster | `Control.Metadata({ Ledger = Ledger })` is missing from `Stores`. `craftsman init` does not write it | Read `Stores` |
| The console shows the old value after an edit | Nothing is wrong yet: a server holding the player sees a console edit only at the session's next save | See `entities.md` |

## A server does not report

Check in this order:

1. **Allow HTTP Requests** is on in Game Settings, under Security.
2. The secret is named `CRAFTSMAN_CONTROL`.
3. The secret's domain covers `operations.craftsman.systems`. A wrong domain is read fine, and every report is then refused.
4. `Start` is a Script under `ServerScriptService.Craftsman`.
5. It is not a local playtest. A local playtest cannot read Secrets, but Team Test or a live server can.

Without `Control.Reporter.Start`, an action still lands and the console records `failed`, because nothing came back.

## A command did not arrive

| Symptom | Likely cause | Settle it |
|---|---|---|
| Expires `failed`, and the game shows nothing | The handler is not registered before `Control.Start`, so the command is skipped | Read the order in the `Start` Script |
| Expires `failed`, and the action happened | A live command's handler has a path that returns nothing, or `Control.Reporter.Start` is missing | Read each path, and the `Start` Script |
| Expires `failed`, and the player left | A live command lasts two minutes, and nobody had the player | Use `.Durable()` if it must wait |
| `waiting` for days | Normal for a durable command, while the player is away | Nothing |
| A durable command is never collected | `FetchCommands = Control.Reporter.Commands` is missing | Read `Control.Start`'s options |
| `refused`: *The params do not match this command's declared type* | The params do not fit the type, or the game's schema is out of date | Compare the declaration with what was sent |
| Refused at dispatch for size | A live command's payload rides one MessagingService message, about 1 KB | Send a key, not the data |
| `craftsman.server.message` is refused | No `Control.OnServerMessage` listener | Add one |

## `Control.Run` says no

| `result.Code` | Means | Do |
|---|---|---|
| `structural` | The command's tier never runs in game | `.Tier("routine")` or `.Tier("valued")`, if that is right |
| `not_attached` | The player's Roblox account is not attached to the project | They link it on the Account page, and the project attaches it |
| `not_held` | The role does not grant it | Give a role in the console |
| `passkey` | The permission needs a passkey | Run it from the console, or change the permission |
| `halted` | Operations are halted | See **Overview** |
| `malformed` | The params do not fit | Fix the call |
| `not_started` | `Control.Reporter.Start` has not run, so a key this server does not know as a command cannot reach Project Control | Fix the `Start` Script |
| `clamped` | Limited by a budget or a cap | Run it from the console |
| `no_handler` | Nothing on this server handles the key | Register the handler before `Control.Start` |
| `frozen`, `expired` | The account is frozen, or the grant ended | Ask an administrator |
| `not_ready`, `unmapped`, `not_a_command` | The access document is not read yet, the place is in no group, or the key is no command | Read the reason in `result.Reason` |
| `errored`, `unknown` | The handler threw, or nobody knows if it ran. Both come with `failed` | Check before running it again |

`Control.Access` fails closed: until the document is read, nobody holds anything.

## The declaration fails

| Symptom | Likely cause | Do |
|---|---|---|
| The publish stops reading a global | Module scope reads `game`, or branches on it. It may name it, and never read it | Move the read into a handler |
| It refuses `const` | Lune has no `const` | Use `local` in a declaration module |
| A param label is on the wrong command | `gt.meta` mutates the type it is handed | Build one type per command |
| `Generated.luau` is stale | A `:Declare` call changed | `craftsman project` |
| `release` refuses the assets module | The module is not what the index generates | `craftsman project` |
| It refuses a key | `craftsman.` is reserved, and so is a member starting `_` | Rename it |

## Publish, build and deploy

| Exit or word | Means | Do |
|---|---|---|
| Exit 0 | Done, or a preview that may list problems | Read it |
| Exit 1 | A verdict about the change. Running it again unchanged gets the same answer | Fix what it names |
| Exit 2 | The command line or configuration is wrong, and nothing was sent | Fix it, or `craftsman install` |
| Exit 3 | Could not tell | Not about the change. Try later |
| Uncommitted work missing from the build | `--apply` builds what is pushed | Commit and push, with permission |
| **Needs a Studio place** | No Studio version could be fetched | A person uploads one on the release |
| **Built, and not handed over** | The run is green, and Project Control recorded nothing | Read the run's log |
| "Server is busy", or the deploy is held for Team Create | Somebody is in Team Create on that place | Ask them to close Studio. The lock outlives them by a few seconds |
| The first save data deploy stops | The key lacks `universe-datastores.control:snapshot` | A person adds the scope |
| `test-roblox`: *The model has no ServerScriptService/Craftsman/Verify* | A game has no spec runner by default | See `shipping.md` |

## The store

| Symptom | Likely cause | Do |
|---|---|---|
| `[Commerce] No catalogue ids` | `Assets` is not mapped in the Rojo project, or the module names nothing for this universe | Map `ReplicatedStorage.Craftsman.Assets`, or run `craftsman store status`, then release |
| A receipt waits | The store of an entity its `Grants` or `Reverse` names is not open on this server | Open it in `Stores` |
| A product grants nothing | It has no entry in `Receipts`, so its effect is the game's own | Add one |
| `store import` exits 1 | A key lacks `game-pass:read` or `developer-product:read` | A person adds the scope |
| `PromptGift` showed nothing | It answered a refusal word | Read the word it returned |

## The kit and Lifecycle

| Symptom | Likely cause | Do |
|---|---|---|
| A module's `Start` never ran | A dependency failed, so its dependents skip | Read the output for the first failure |
| The load rejects | Two modules share a name, a cycle, or a frozen table | The message names which |
| A nested module is not loaded | A ModuleScript's children are its own | Move it to a Folder |
| `Init` warns it yielded | `Init` must not yield | Move the waiting to `Start` |
| Machine states do nothing on a client | The host is not started, the id differs, or nothing drives `Update` | See `kit-state.md` |
| `WorldUtil` errors about `nil` | It was called with `.`, or a cast has no params | Use `:`, and register `RayParams` |
| A cooldown resets alone | A table or Instance key was collected | Use a string, number or Player |
| Input does nothing | `InputUtil` only gets input on a client, and game processed input never starts an action | Check where it runs |
| `TouchUtil` layout is ignored | The buttons did not exist yet, or the version differs | Make the buttons first |

## Something else

If none of these fits, work down the ladder in `SKILL.md`. For a Ledger reason such as `Unresolved`, `Refused`, `Behind` or `Unreadable`, use the Ledger skill. Never retry a grant to see what happens.
