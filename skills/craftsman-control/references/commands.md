# Commands, in-game access and the Konsole bar

A **command** is an operation that reaches a running game server: a kick, a
teleport, a message on screen, an effect. The console dispatches it, Roblox
delivers it over MessagingService, and the handler the game registered does it.

## Declaring

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

local Reason = gt.string({ bytes = "[1, 200]" })

local Kick = {}

Kick.Now = Control.Command({ Reason = Reason })
	.Label("Kick Player")
	.NoImpact()
	.Tier("routine")
	.Handles(function(directive)
		local player = game:GetService("Players"):GetPlayerByUserId(tonumber(directive.target) or 0)
		if player == nil then
			return false, "that player is not in this server"
		end
		player:Kick(directive.params.Reason)
		return true
	end)

return Control.Scope("Kick"):Commands(Kick)
```

Placed under `Control.Scope("Player"):Scopes({ Kick = Kick })`, the keys are
`player.kick.now` and so on: snake_case segments of the scope path.

- `Control.Command(params)` takes a table of GreenTea types (or one built
  type). Params are checked against it before the handler runs; a mismatch is
  reported `malformed` and never reaches the handler.
- **An impact is required**: `.NoImpact()`, or `.Grants(param)`,
  `.Transfers(param)`, `.Prices(param?)` when it hands out value. Without one
  the declaration errors.
- `.Handles(fn)` ends the chain and registers the handler. `.Declared()` ends it
  with no handler, for a command handled elsewhere.
- The handler gets `directive = { id, op, target, params }` and returns
  `true, "what happened"` or `false, "why not"`. The console shows the reason
  verbatim, so name the case. `target` is a string (a UserId for a player
  command). It runs in Roblox only, so `game` is fine inside it.
- Label a param for the console with `gt.meta(type, { Label = "..." })`.
  **`gt.meta` mutates the type it is handed**: build one type per command
  rather than sharing a labelled one.

| Builder | Meaning |
|---|---|
| `.Label(text)` | Name in the console |
| `.Tier("routine" \| "valued" \| "structural")` | Default `structural`: passkey, a 15-minute delay, and never runnable in-game. Lower it for anything a moderator should do quickly |
| `.Permission(key)` | Permission key; defaults to the command key |
| `.Factor(f)`, `.Approver(bool)`, `.Constrain(c)` | Extra gates the console enforces |
| `.Durable()` | Waits, without expiring, until a server holding the player acknowledges it (for "on next join"). The handler must cope with the player being absent |
| `.PerServer()` | Targets a server, not a player; may also go to every server at once |
| `.SingleServer()` | Per-server, and never sent to every server (shutdown-like actions) |

Without `.Durable()`, a command is **live**: this session or not at all, two
minutes, no retry. Silence is reported `failed`, never `refused`. A live
command's whole payload rides one MessagingService message, so keep params small
(about 1 KB); a larger one is refused at dispatch.

**Register handlers before `Control.Start`**, which means the modules declaring
them are required before it. A command arriving with no handler is skipped and
expires as `failed`.

`Control.OnCommand(key, fn)` registers a handler for a key declared elsewhere.
Prefer `.Handles`, which also checks params.

For a durable command to be collected, `Control.Start` needs
`FetchCommands = Control.Reporter.Commands`. A per-server command reaches only a
server started with `Registry = true`.

## Running from game code: `Control.Run`

```luau
local result = Control.Run(admin, "player.kick.now", tostring(target.UserId), { Reason = "afk" })

if result.Outcome ~= "applied" then
	warn(result.Reason)
end
```

`Control.Run(who: Player | userId, key, target?, params?)` runs an operation
for a player whose Roblox account is attached to the project and whose role
allows it.

| `Outcome` | Meaning |
|---|---|
| `applied` | It ran |
| `refused` | It was asked and said no (the handler returned false, or Roblox or a reducer refused) |
| `denied` | Not allowed. `result.Code` is a stable word (`structural`, `not_attached`, `not_held`, `passkey`, `halted`, `malformed`...) and `result.Reason` a sentence |
| `failed` | Unknown whether it ran (`errored`, `no_handler`, `unknown`, `not_started`) |

- A command runs on this server from the access document it already holds,
  with **no request** to Project Control, and does not yield.
- **A `structural` command never runs in-game.** `.Tier("routine")` or
  `.Tier("valued")` is the only switch. A permission needing a passkey, or a
  grant limited by a budget, also keeps it console-only.
- Anything else (a reducer, a data edit, a ban) is sent to Project Control,
  which decides; `Run` yields then.
- Every in-game run is audited, in batches sent at most once a minute and only
  when there are some. It needs `Control.Reporter.Start` and `Control.Start`
  with `Access` left on.

### Banning in-game

```luau
Control.Run(admin, "craftsman.player.ban_in_game", tostring(userId), {
	display_reason = "Exploiting",          -- shown to the player, 1 to 400 bytes
	private_reason = "speed hack in match 7", -- optional, moderators only
	duration_seconds = 86400,               -- leave out for permanent
	exclude_alt_accounts = false,
})
```

It goes through Roblox's ban API via Project Control. There is no in-game unban.
`craftsman.player.ban` (the console's) always comes back `denied`/`passkey` from
a game. Don't hand-roll bans with a DataStore.

## Checking access

`Control.Access` reads the access document Project Control writes into the
universe's DataStore.

- **`Access.CanRun(userId, commandKey) -> (ok, sentence?, reason?)`**: whether
  this server may run that command for this player now. Use this to show or
  grey a button.
- `Access.HasPermission(userId, key)`: whether the key is held at all. True even
  when a passkey, budget or halt would stop it, so **not** for buttons.
- `Access.GetMember(userId)`, `Access.GetPermissions(userId)`,
  `Access.IsReady()`, `Access.OnChanged(fn)`.
- It fails closed: until a document is read nobody holds anything. It is read
  from the real DataStore, so Studio needs API access and an attached account.

Don't build a separate admin list: roles and grants live in the console.

## The Konsole command bar

Optional. The game installs `Konsole = "kyrorblx/konsole@0.1.11"` itself.

```luau
-- server Script, after Control.Reporter.Start
Control.Konsole.Attach(Konsole, {
	Declaration = Definitions,
	Kick = "player.kick.now", -- the game's own kick command, if any
})

-- client LocalScript under StarterPlayerScripts
Control.Konsole.AttachClient(Konsole)
```

- Every declared command becomes a bar command named after its key's last
  segment (`player.effects.sparkle` is `sparkle`), and each run goes through
  `Control.Run`, so access, tier and audit apply. Ledger operations are not in
  the bar.
- A player command takes targets first (`me`, `all`, `others`, a name or
  prefix), then params, required first. Only string, number, boolean, enum and
  optional params work; a command with table params is left out with a warning.
- `features`, `configs [key]`, `override <key> <value>`, `clearoverride <key>`
  and `clearoverrides` read and override configuration on this server.
- `ban <target> <reason> [seconds] [alts]` bans one player; `all`/`others`
  are refused. The bar never kicks or bans the player typing.
- The bar opens (`T` or `/konsole`) only for a player holding at least one
  command's permission. Studio and the game's creator get no rank by default.
