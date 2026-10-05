# The store: passes, products and subscriptions

The catalogue is declared in code, and `Control.Commerce` takes the receipts and
keeps each player's **book**: what they hold, bought, gifted or awarded. Roblox
has no API to grant or revoke a pass, so the book is what makes awards, gifts
and revocations possible, and it is why ownership must be asked of Commerce.

**Never write a `ProcessReceipt` handler or call `UserOwnsGamePassAsync` in a
game using this.** Ask `Control.Commerce`.

## Declaring the catalogue

```luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Player = require("./Player")

local Catalogue = Control.Catalogue

return {
	Passes = {
		vip = Catalogue.Pass({
			Name = "VIP",
			Description = "A golden name and double visits.",
			Price = 49,
			ForSale = true,
			RegionalPricing = true,
			Giftable = true,
		}),
	},
	Products = {
		golden_pickaxe = Catalogue.Product({
			Name = "Golden Pickaxe",
			Price = 25,
			ForSale = true,
			Grants = Catalogue.Op(Player.Inventory, "GrantItem", { ItemId = "golden_pickaxe", Amount = 1 }),
			Reverse = Catalogue.Op(Player.Inventory, "RemoveItem", { ItemId = "golden_pickaxe", Amount = 1 }),
			OnRefund = "Propose", -- or "Reverse", which needs Reverse
			-- Once = true, for a product a player may buy only once
		}),
	},
	Subscriptions = {
		premium = Catalogue.Subscription({ Label = "Premium" }),
	},
}
```

and in the declaration: `Control.Declare({ ..., Store = Catalogue })`.

- A key is unique across passes, products and subscriptions, and at most 59
  bytes.
- `Grants` and `Reverse` name a reducer of an entity (see entities.md). Every
  grant also receives the `PurchaseId`; **stamp it on what the grant gives**, so
  `Reverse` removes that exact copy and never "one of these".
- The Roblox ids are not in code. The console's Store tab applies the
  catalogue to the universe (`craftsman store plan` prints what that would do,
  `craftsman store import` takes in what Roblox already sells), and servers
  receive the ids through the config head. Nothing is for sale on a server
  until they arrive.

## Wiring

```luau
-- Stores ModuleScript, beside the entity stores
Control.Commerce.Open({ Declaration = Definitions, Ledger = Ledger })

-- server Script, once
Control.Commerce.Start({ Declaration = Definitions, Ledger = Ledger })
```

The store of every entity a `Grants` or `Reverse` names must be open on the
server, or its receipts wait.

## Using it

| Call | Does |
|---|---|
| `Control.Commerce.Owns(player, "vip")` | Holds the pass, by purchase, gift or award, and not revoked |
| `Control.Commerce.Subscribed(player, "premium")` | Same for a subscription |
| `Control.Commerce.Prompt(player, key)` | Prompts a purchase; refuses one that is not for sale yet, already held, or a `Once` product already bought |
| `Control.Commerce.PromptGift(giver, recipientUserId, key)` | Gifts a `Giftable` item; answers `prompted` or a refusal word (`not_giftable`, `self`, `already_held`, `price_level`...) and anything but `prompted` showed nothing |
| `Control.Commerce.Changed` | Fires `(player, key, holds)` when holdings change, including at join |
| `Control.Commerce.Gifted` | Fires `(recipient, key, giverUserId, purchaseId)`; the toast is the game's |
| `Control.Commerce.Known()` | The ids this server has received |

- A book that fails to load never kicks the player: ownership falls back to
  Roblox's answer, and gifts and awards are unseen until it loads.
- Refunds, awards and revocations are done from the console; the game does not
  need code for them beyond `Changed`.
