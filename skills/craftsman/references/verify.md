# Check a claim

The source is in `CraftsmanPackages/` when `craftsman install` has run. When it disagrees with the docs or this skill, the source is right. Say so.

## Which file answers which question

| Question | Read |
|---|---|
| What `Control` exports | `CraftsmanControl/init.luau` |
| A feature or config | `CraftsmanControl/Feature.luau`, `Config.luau`, `Scope.luau` |
| A declaration's keys and what is refused | `CraftsmanControl/Declare.luau`, `Manifest.luau` |
| A command's builders | `CraftsmanControl/Command.luau` |
| A command's delivery | `CraftsmanControl/Directives.luau` |
| `Control.Run` and its outcomes | `CraftsmanControl/Actions.luau` |
| `Control.Access` and its reasons | `CraftsmanControl/Access.luau` |
| An entity and its reducers | `CraftsmanControl/Entity.luau`, `Ledger/Producer.luau`, `Ledger/Ops.luau` |
| `Control.Store` on Ledger | `CraftsmanControl/Ledger/Store.luau` |
| MemoryStore | `CraftsmanControl/Memory/Store.luau`, `SortedMap.luau`, `Queue.luau` |
| Replication | `CraftsmanControl/Replica/` |
| Commerce and receipts | `CraftsmanControl/Commerce.luau`, `Receipts.luau`, `Book.luau` |
| The Konsole bar | `CraftsmanControl/Konsole.luau` |
| `Control.Start` and its options | `CraftsmanControl/Runtime.luau` |
| Reporting | `CraftsmanControl/Reporter.luau` |
| The server registry | `CraftsmanControl/Registry.luau` |
| The loader | `Lifecycle/init.luau` |
| A kit module | `Craftsman/<Module>.luau`, `Craftsman/Utilities/<Module>.luau`, `Craftsman/StateMachine/` |
| A kit setting | `Craftsman/Config.luau` |
| Keeper | `Keeper.lua` links to `_Index/averyark_keeper@<version>/keeper/src/init.luau` |
| A CLI command | `craftsman help <command>` |

A file named here can move between releases. If it is gone, search for the name.

## Check the surface

Before you name a method, search for it:

```bash
grep -n "Commerce.Owns\|function Commerce" CraftsmanPackages/CraftsmanControl/Commerce.luau
```

Before you name a CLI flag, ask the CLI. An option a command does not have is refused, naming the nearest one.

```bash
craftsman help publish
```

## Check the project

| You want to know | Run |
|---|---|
| Versions | `craftsman --version` |
| What is wrong with the setup | `craftsman doctor --offline` |
| What the console has, per place group | `craftsman status` |
| Types and lint | `craftsman check` |
| What a publish would report | `craftsman publish` |
| Whether generated files are current | `craftsman project --check` |

## Say what you checked

"I read `Actions.luau`" and "I did not run this" are both useful. A claim about Roblox's own behaviour, such as a limit or an Open Cloud response, is not settled by this source. Say where it came from.
