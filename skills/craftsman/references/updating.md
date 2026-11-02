# Updating a game's packages

Craftsman is four packages, each at its own version. Control is always the CLI's version. `craftsman.lock` names the set, and CI installs exactly what it names. Ask before moving any of them: it changes what the game runs.

Full steps are in `docs-bundle.md`, under `/docs/project-setup/update-packages` and `/docs/project-setup/upgrade-from-older-versions`.

## Move

| To move | Do |
|---|---|
| Everything for this CLI | `craftsman update` |
| Only the kit | `craftsman update kit` |
| The kit to one version | `craftsman update kit@0.10.1` |
| Control | Change the `craftsman` pin in `rokit.toml`, run `rokit install`, then `craftsman update` |

`update` prints the `rokit.toml` line when a newer Control exists, and never edits that file itself.

## Then

1. Run `craftsman doctor`, `check` and `publish`, and read each.
2. Read the package's changelog, and search the game for what it says changed.
3. With permission, run `craftsman test-roblox`.
4. Commit `craftsman.lock`, `rokit.toml` and anything the update rewrote.
5. A person deploys it.

## Older games

| Sign | What it is | Do |
|---|---|---|
| `wally.toml` names Craftsman | Before the registry | `craftsman update` moves it, all or nothing, and lists what it could not change. Show the person that list |
| A lock that names a bundle | CLI 0.9 or 0.10.0 | `craftsman update`, then replace `release.yml` with the template, because the old one cannot read the new lock |
| `Control.Flag`, `:Flags`, `Control.FlagService` | Framework 0.1.x | A migration, not a pin. Stop, and tell the person |
| `Craftsman.Component` | Kit 0.9 | Move to Lifecycle: `Lifecycle:LoadModulesAsync(folder)`, and `Dependencies` on the modules |
| `ReplicatedStorage.CraftsmanConfig` | Before `craftsman.toml` | Move its values under `[kit.*]`, and delete it. The kit warns once |
| `places/<role>.place.json` | Before `[places]` | `craftsman install` moves it |

## Ledger

Ledger arrives in deps. **Ledger 7 cannot read Ledger 5 saves**, so moving a live game across that line means every store starts fresh under a new name ending in `V7`, and every server shuts down at the publish, because the two must never run side by side. That is a decision for the person, and the Ledger skill has the steps. Never treat it as a pin bump.

## Updating this skill

This skill is pinned in its `metadata`. If the game runs something newer, reinstall it:

```bash
npx skills add averyark/craftsman-cli
```
