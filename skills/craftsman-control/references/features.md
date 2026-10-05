# Features and configs

A **feature** is a part of the game the console can switch on and off. A
**config** is a typed value on a feature, edited in the console without a
release. Both are declared on a scope and read through the object that declared
them.

## Declaring

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

local Shop = Control.Scope("Shop"):Feature({
	Client = true,
	Description = "The in-game shop.",
}):Config({
	DiscountPercent = Control.Config(gt.number({ integer = true, range = "[0, 75]" }), {
		Default = 0,
		Shared = true,
		Description = "Store item discounts.",
	}),
	Prices = Control.Config(gt.dictionary(gt.string(), gt.number({ integer = true })), {
		Default = { Sword = 100, Shield = 150 },
	}),
})

return Shop
```

The declaration's `Includes` lists `Shop.Definition`.

`:Feature(options)`, every option optional:

| Option | Default | Meaning |
|---|---|---|
| `Active` | `true` | The code default. `false` ships it switched off. |
| `CanOverride` | `true` | `false` locks `Active`: the console, server overrides and the hosted value are ignored. |
| `Client` | `false` | The feature also runs on clients. |
| `WaitForDocument` | `false` | Hold activation until the hosted values load, or `DocumentTimeout` passes. |
| `Description` | | Shown in the console. |

`Control.Config(gtType, { Default, Shared, Description })`: `Default` is
required and must fit the type. `Shared = true` sends the value to clients.

- **A config belongs to a feature**: call `:Feature()` before `:Config()`.
- **Keys come from the scope path**: `Scope("Shop")` is `shop`, its
  `DiscountPercent` is `shop.discount_percent`. A rename is a new key, and the
  hosted value of the old one does not follow.
- **A child feature** is a scope under a feature, declared with `:Scopes` so the
  parent carries it typed:

  ```luau
  local Seasonal = Control.Scope("Seasonal"):Feature():Config({ ... }):Scopes({
  	Banner = Control.Scope("Banner"):Feature({ Client = true }):Config({ ... }),
  })

  Seasonal.Banner.Message:Get()
  ```

  A child runs only while its parent runs, and keeps its own state meanwhile.
- A config type may not be `gt.optional` as a whole (a field inside may), and no
  table in it may be empty.

### Types

| GreenTea | In the console |
|---|---|
| `gt.boolean()` | true / false |
| `gt.number({ integer, range = "[0, 75]" })` | a number |
| `gt.string({ bytes = "[1, 80]", graphemes, pattern })` | a string; a pattern over 256 bytes is refused |
| a union of `gt.literal`s | one of the options |
| `gt.table({...})`, `gt.array(x)`, `gt.dictionary(gt.string(), x)` | an object or array |
| `gt.Color3()`, `gt.Vector3()` | `"#rrggbb"`, `[x, y, z]` |

`DateTime` and `EnumItem` are refused: use Unix seconds or an ISO string, and a
union of literals of the enum item's names.

## A boolean "enabled" setting is the feature's `Active`, not a config

Don't declare `Enabled = Control.Config(gt.boolean(), ...)`. Make the thing a
feature and use its `Active`, which is the off switch the console, the bot's
`/feature <key> off` and the attention list all understand.

## Running code

```luau
Shop:OnActivate(function(keeper)
	local stand = keeper:Clone(ShopStand)
	stand.Parent = workspace
	keeper:Connect(Players.PlayerAdded, GreetShopper)
end)

Shop:OnDeactivate(function()
	print("[Shop] Closed")
end)

local function Buy(player, item)
	if not Shop.Active then
		return false, "The shop is closed"
	end
	local percent = Shop.DiscountPercent:Get()
	-- ...
end

Shop.DiscountPercent:Observe(function(value, source)
	SetDiscount(value)
end)

Shop:ObserveActive(function(active)
	ShopButton.Visible = active
end)

Shop.Heartbeat:Connect(function(dt)
	SpinSign(dt)
end)
```

- `OnActivate(fn)` runs every time the feature becomes active, and at once if
  it already is. The `keeper` (averyark/keeper) is cleaned at every
  deactivation. `OnActivate(fn, { Clear = false })` keeps it across them.
- Both return a function that removes the handler. Each handler runs in its own
  thread; an error warns and stops nothing else.
- Parents start before children; children stop before parents.
- **A feature is dormant until a runtime starts**: `Active` is `false` and no
  handler runs. `:Get()` answers the code default until then.
- `:Observe(fn)` fires at once and on every change of value or source. The
  source is `"override"`, `"fallback"`, `"hosted"` or `"default"`. Table values
  are frozen. Configs stay readable while their feature is inactive.
- Step events: `PreSimulation`, `PostSimulation`, `Heartbeat`, `Stepped` on both
  sides; `PreRender`, `PreAnimation`, `RenderStepped` on clients only. Connected
  only while active, unless `{ Disconnect = false }`.
- Readiness: `Runtime.IsReady()` / `OnReady(fn)`, and the same on each feature
  (`Shop:IsReady()`), mean the hosted values have loaded. A server starts every
  feature on its defaults at once and takes the hosted state when it arrives;
  use `WaitForDocument` only when a default would be wrong to run on.

## The client

- Only `Client = true` features run on a client, and a client reads only their
  `Shared` configs. `:Get()` on anything else errors on the client.
- The declaration module must be under `ReplicatedStorage` for a client to
  require it, and a client must require it: requiring is what registers it.
- `Control.StartClient({ DocumentTimeout = 10 })` once, from a LocalScript.
  Without it a client feature never runs, with no error.
- A client feature is held dormant until the server's first state arrives, then
  does exactly what the server sent.

## Where a value comes from

1. A **server override** (one server, from the console's Servers page or
   in-game).
2. **Use Fallback**, which serves the code default.
3. The **hosted value**, edited in the console.
4. The **code default**.

`Active` skips step 2. `CanOverride = false` skips steps 1 and 3.

In-game, an attached admin overrides on their own server with
`Control.Override(player, "shop.discount_percent", "25")`,
`Control.Override(player, "shop", "off")` and `Control.ClearOverride(player, key?)`.
The value is typed text. A server override reaches only a server started with
`Registry = true`.

## Declaring where it is used (R160)

A config or command can be declared in the file that uses it, even a client
script with `const` and `game` at the top: the CLI parses it, never runs it, and
copies the declaration into `Definitions/Generated.luau`.

The feature's `Definitions` is a folder:

```luau
-- src/Features/Grass/Definitions/init.luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Generated = require("@self/Generated") -- @self, never ./

return Control.Scope("Grass"):Feature({ Description = "Procedural grass." }):Scopes(Generated.Scopes)
```

```luau
-- src/Features/Grass/Client/Bending.luau
const Grass = require("@game/ReplicatedStorage/Features/Grass/Definitions")
const Tune = require("@game/ReplicatedStorage/Features/Grass/Definitions/Tune")

const PUSH = Grass:Declare("Push", {
	Feature = Tune.Group("Grass bending away from characters."),
	Config = {
		Radius = Tune.Number(3.8, "[0, ]"),
	},
})

-- PUSH.Radius:Get(), typed from the spec
```

- `spec.Feature`, `spec.Config` and `spec.Commands` take what `:Feature`,
  `:Config` and `:Commands` take. A child: `Grass.Push:Declare("Child", {...})`.
- **Run `craftsman project`** (or keep `craftsman serve` running) after every
  change, and commit `Generated.luau` with it. At run time `Declare` errors if
  `Generated.luau` disagrees, naming `craftsman project`; `publish` and
  `release` refuse a stale one.
- A spec may use literals, tables, function literals, top-level locals bound to
  those, and `require`s of **`CraftsmanPackages`**, the game's own **`Packages`**
  or **this feature's `Definitions/`** folder (put helpers like `Tune` there). Refused: Roblox globals, a local built
  from one or reassigned, `Declare` inside a function or block, another
  feature's scope, `Entity` (entities stay in `Definitions`), and two files
  declaring the same name.
- A command's handler is never in the spec. Attach it beside the state it
  needs: `REGROW.Now.Handles(function(directive) ... end)`.

## Migrating from flags

`Control.Flag`, `:Flags`, `Control.FlagService` and `KilledValue` were removed
in framework 0.2.0 with no alias, and hosted flag values do not carry over.
Moving a game off them is a migration, not a rename: put each flag under a
feature, make a boolean "enabled" flag the feature's `Active`, replace
`FlagService():Start` with `Control.Start`, move gated code into `OnActivate`,
then re-enter the hosted values in the console. Tell the person before starting
it.
