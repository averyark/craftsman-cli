# The store: passes, products and subscriptions

Items are made with the CLI and live in the repository's asset index
(`assets/index.toml`); the game reads their ids from the generated module
`ReplicatedStorage.Craftsman.Assets` (`src/Control/Assets.luau`). What a
product's receipt does is declared in code as `Receipts`. `Control.Commerce`
takes the receipts and keeps each player's **book**: what they hold, bought,
gifted or awarded. Roblox has no API to grant or revoke a pass, so ownership
must be asked of Commerce.

**Never write a `ProcessReceipt` handler or call `UserOwnsGamePassAsync` in a
game using this.** Ask `Control.Commerce`. Never write an id into code: there
is no `Control.Catalogue` any more.

## Making items (the CLI)

| Command | Does |
|---|---|
| `craftsman store add` | Asks pass or product, name, key, description, price, for sale, icon (drag a PNG/JPEG), a product's refund policy, and "Also create a gift product?". Then **Create now / Save as draft (default) / Cancel** |
| `craftsman store edit <key>` | Changes fields; sends only what changed to every universe |
| `craftsman store gift <key>` | Makes the product `<key>.gift`, the gift of `<key>`; it takes its own price and for-sale, defaulting to `<key>`'s |
| `craftsman store rollback <key>` | Puts a history entry's fields back in every universe |
| `craftsman store push` | Creates drafts in the universes that lack them, and sends what was edited in `assets/index.toml` by hand |
| `craftsman store import` | Adopts passes and products Roblox already lists, matched by name, and subscriptions by their `EXP-…` id. Asks each new key, or takes `--key "<name or id>=<key>"` (repeatable); the default is the key Project Control already holds, and a 0.10 gift product (`<Name> (Gift)`) comes in as `<key>.gift`. Needs `game-pass:read` and `developer-product:read` on each universe's key, and exits 1 naming them when they are missing |
| `craftsman store list` / `status` | `status` exits 1 on a draft, a hand edit not yet pushed (*edited locally — push to apply*), a missing icon or a stale module |

- An item is created in **every universe of `craftsman.toml`'s `[places]`**,
  with its own id in each. A write in a universe holding a protected place
  group waits for a passkey in the browser.
- **A key is what the game names**: letters in any case, digits, dots and
  underscores (`^[A-Za-z0-9._]+$`), at most 59 bytes, and case-sensitive.
  `Assets.Passes.vip`, `Commerce.Owns(player, "vip")`.
- **A gift is a product keyed `<key>.gift`**, the gift of pass `<key>` (or of
  product `<key>` when there is no such pass). Its existing is what makes the
  item giftable. Its id is `Assets.Gifts.<key>` (never under `Products`). To
  gift, call `Control.Commerce.PromptGift(giver, recipientUserId, "<key>")`
  with the original key: that grants to the recipient. Prompting the raw gift
  id directly does not.
- **Never rename a key**, in the CLI or by hand: players' purchase records are
  filed under it, so `push`, `edit` and `status` refuse one renamed by hand.
  Rename it back and change its name instead.
- A product's refund policy is `propose` (a person approves the reversal, the
  default) or `reverse` (at once, which needs `Reverse`).
- Subscriptions are made in Creator Hub and only ever imported.
- Commit `assets/` and `src/Control/Assets.luau` with the change. `release` and
  `publish` refuse a module that is not what the index generates; run
  `craftsman project`. A draft is absent from the module, so code naming it
  does not type-check.
- Ask before `store add`, `edit`, `gift`, `rollback`, `push` or `import` without
  `--dry-run`: they write to Roblox, and passes and products are never deleted.

## Declaring receipts

```luau
-- in the declaration
Control.Declare({
	-- ...
	Receipts = {
		coins_500 = {
			Grants = Control.Op(PlayerData, "AddCoins", { Amount = 500 }),
			Reverse = Control.Op(PlayerData, "RemoveCoins", { Amount = 500 }),
			-- Once = true, for a product a player may buy only once
		},
	},
})
```

- The key is the product's key in the index. A product with no entry is sold
  and grants nothing (its effect is the game's own).
- `Grants` and `Reverse` name a reducer of an entity (see entities.md). Every
  grant also receives the `PurchaseId`; **stamp it on what the grant gives**,
  so `Reverse` removes that exact copy and never "one of these".

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
| `Control.Commerce.Prompt(player, key)` | Prompts a purchase; refuses one that is not for sale here, already held, or a `Once` product already bought |
| `Control.Commerce.PromptGift(giver, recipientUserId, key)` | Gifts an item that has a `<key>.gift` product; answers `prompted` or a refusal word (`not_giftable`, `self`, `already_held`, `price_level`...) and anything but `prompted` showed nothing |
| `Control.Commerce.Changed` | Fires `(player, key, holds)` when holdings change, including at join |
| `Control.Commerce.Gifted` | Fires `(recipient, key, giverUserId, purchaseId)`; the toast is the game's |
| `Control.Commerce.Known()` | The ids this server read from the module |

- A book that fails to load never kicks the player: ownership falls back to
  Roblox's answer, and gifts and awards are unseen until it loads.
- Refunds, awards and revocations are done from the console; the game does not
  need code for them beyond `Changed`.
