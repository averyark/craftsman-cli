# Commands, gates and live brakes

## Choosing a tier

The tier decides what a person goes through at run time. An operation's default
depends on its kind, so state the tier you mean.

| Tier | A person must | Use for | In game? |
|---|---|---|---|
| `routine` | Hold the permission | Kick, message, a visual effect | Yes |
| `valued` | Hold the permission, within any budget | Handing out currency or items | Yes, unless clamped |
| `structural` | Tap a passkey and wait 15 minutes, with a second approver when anybody else could approve | Anything hard to undo | **Never** |

- A **command** defaults to `structural`. A moderator's command with no
  `.Tier("routine")` can't be run quickly, and can't be run in game at all.
- A **ledger reducer** defaults to `routine`, or `valued` when it `.Grants` or
  `.Transfers`.
- `.Irreversible()` forces `structural`, and a lower `.Tier` then errors.
- `.Factor("passkey")` adds a passkey without the delay, and `.Approver(true)` asks
  for a second person. A passkey-gated command can't run in game: `Control.Run`
  answers `denied` with the code `passkey`.
- `.Constrain(gtType)` limits parameters beyond their type.

An impact is required: `.NoImpact()`, `.Grants(param)`, `.Transfers(param)` or
`.Prices(param?)`.

## Choosing delivery and reach

| Builder | Reaches | Traps |
|---|---|---|
| none (live) | Servers running now, for two minutes | One attempt. Payload about 1 KB, refused at dispatch if larger. Silence is `failed` |
| `.Durable()` | The player's next join, with no expiry | Needs `FetchCommands = Control.Reporter.Commands`, or it is never collected. Refused with `.PerServer()` |
| `.PerServer()` | One server, or every server | Going to every server also needs `craftsman.server.every` |
| `.SingleServer()` | One server, never all | Use for anything shutdown-like |

`waiting` on a durable command is normal and can last days: nobody has been asked
because the player isn't here. Only the acknowledgement settles it, so a server
that fetches and dies leaves the work for the next join.

When every server runs a command, each reports back after a random 0–5 s delay.
The first `applied` settles it.

## Handler outcomes

| The handler | From the console | Through `Control.Run` |
|---|---|---|
| Returns `true, "what"` | `applied` | `applied` |
| Returns `false, "why"` | `refused`, with the reason shown | `refused` |
| Returns nothing | **Not reported; expires `failed`** | `refused` |
| Errors | `refused`, "the game's handler errored" | `failed`, code `errored` |

Without `Control.Reporter.Start`, the action still lands and the console records
`failed`, because nothing came back.

`Control.OnCommand` skips parameter checks unless it is given a shape, so prefer
`.Handles`. Check `Control.Directives.Handles(op)` before registering a shared
handler, because a duplicate registration errors.

## Parameters from state

`.Writes` can borrow a field's type rather than repeating it:

```luau
Profile.Writes({ "Coins" })                                -- Coins is the param
Profile.Writes({ Amount = "Coins" })                       -- param Amount, typed as Coins
Profile.Writes({ Amount = { From = "Coins", Label = "Coins to add" } })
```

The field name `Kind` is reserved. A reducer may not be named `Open`, `Take`,
`Credit`, `Close`, `Hold`, `Confirm`, `Release`, `Bump` or `Cut`, or start with
`Debit:` or `_`.

## Live brakes

For an incident, from most to least reach:

| Brake | Where | What it does |
|---|---|---|
| Feature `Active` off | Console | Stops the feature everywhere. No factor to switch off; a passkey to switch on |
| Use Fallback | Console | Holds one config, a feature and everything under it, a group, or the project on `Default`. Never touches `Active`, never expires, keeps the hosted value |
| Server override | Console, or `Control.Override` | One server only. Lives in memory and ends with the server |

```luau
Control.Override(player, "shop.discount_percent", "25")
Control.Override(player, "shop", "off")
Control.ClearOverride(player, "shop.discount_percent")
Control.ClearOverride(player)
```

The value is text, read against the config's type. It needs
`craftsman.config.override`. There is no in-game API for Use Fallback.

Design for these: give every config a `Default` the game can really run on, and
keep `OnActivate` and `OnDeactivate` able to run repeatedly in one server.

## The Servers page

`Registry = true` in `Control.Start` writes each server to MemoryStore every 45
seconds with a 120-second expiry. That's the experience's own quota, and nothing
goes to Project Control. `craftsman init` doesn't add it. An empty registry can
mean zero servers or that nothing sets it.

It brings the provided `craftsman.server.message`, `shutdown` and `kick_all`:

```luau
local disconnect = Control.OnServerMessage(function(text)
	-- draw the message
end)
```

- With no listener, `craftsman.server.message` is **refused**. Only one listener
  is kept.
- The text is **not passed through Roblox's text filter**.
- `Control.Registry.Commit(instance)` is how a build stamps its commit, which the
  Servers page shows. The release build already does this for you.

## In-game access

The access document is read only when its revision moves. Its checks cost no
request.

`Control.Access.CanRun(userId, key)` gives a reason, in this order: `not_ready`,
`halted`, `unmapped`, `not_a_command`, `structural`, `not_attached`, `frozen`,
`not_held`, `passkey`, `expired`, `clamped`.

- Access fails closed: until the document has been read, nobody holds anything.
- `not_attached` means the player's Roblox account isn't attached to the project.
  A person links it on the Account page, and the project attaches it.
- `Control.Access.OnChanged` doesn't fire when a grant expires, so check `CanRun`
  at the moment of use rather than caching it.

`Control.Start` takes `Access = false` to skip the document in a game with no
in-game access.

## Reporter

`Control.Reporter.Start` is required for results to come back. A second `Start` is
ignored, and `Emit` before `Start` does nothing, silently. A quiet server sends
nothing. Never add a timer that reaches Project Control.
