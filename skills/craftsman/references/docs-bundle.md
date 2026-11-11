# Project Control documentation

The whole documentation, one file. An article's address is its heading's path under https://operations.craftsman.systems. Generated: do not edit.

---

# Collection: Getting started

What Project Control is, and how to install the CLI and sign in from your terminal.

---

## Page: /docs/getting-started/what-is-project-control

**What Project Control is.** The three parts of Project Control in your game's repository, and how they fit together.

Project Control lets you change a live Roblox game without shipping a new release. You can switch a feature off, change a value, run a command on a server, or deploy a build. Three parts of it live in your repository.

### The declaration

The declaration says what the console may see and change. It is a Luau module at `src/Control/Definitions.luau`. It names your universes and every feature the game includes.

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Shop = require("../Features/Shop/Definitions")

return Control.Declare({
	Universe = 10202097921, -- the universe ID, not a place ID
	EventCeiling = 500,
	Includes = { Shop.Definition },
})
```

A feature can carry configs, commands and entities. A config is a typed value with a code value the game falls back to.

### The runtime

The runtime is the Control package your game runs. A server Script calls `Control.Start(Definitions, ...)`. A client calls `Control.StartClient()` when any feature runs on clients.

It reads the values the console sets, starts and stops features, and runs commands. If the console cannot be reached, the game runs on its code values.

Craftsman's code installs into `CraftsmanPackages/` and maps into `ReplicatedStorage`.

### The CLI

`craftsman` installs the packages, writes your Rojo project from your feature folders, uploads media and store items, checks your declaration with the console, and builds releases.

```bash
craftsman init --place 137372027987942   # set the repository up
craftsman serve                          # work in Studio with Rojo
craftsman publish                        # preview what a release would carry
craftsman publish --apply                # build a release on GitHub
```

A publish never changes a live game by itself. A release goes live in a place group when someone deploys it from the console.

Next: [What you get](/docs/getting-started/what-you-get)

---

## Page: /docs/getting-started/what-you-get

**What you get.** One console for your live game: configs, player data, live servers, releases, health and erasure requests.

Project Control is one console for a live Roblox game. Here is what it does for you.

### Change it live

Change any config on every server in seconds. Switch a feature off from the console or from Discord. See [Configs](/docs/features/configs) and [Features](/docs/features/features).

### Fix player data

Open any player's save and see how it changed. Put a field back, through your game's own rules. See [Entities](/docs/players-and-data/entities).

### Act on live servers

Ban through Roblox, kick, or run your own commands on one server or all of them. See [Commands](/docs/servers-and-commands/commands) and [Send commands to servers](/docs/servers-and-commands/commands-page).

### Ship from GitHub

Each build is tested inside Roblox, then deployed to the places you pick. A deploy that changes save data takes a snapshot first. See [The release workflow](/docs/releases/release-workflow).

### See how it is going

See live player counts, each release checked against the one before, and what is left of your Roblox limits. See [Read the Overview](/docs/monitoring/read-the-overview).

### Stop something fast

Turn off what is breaking your game, without waiting for a deploy. See [Turn a feature off](/docs/emergencies/turn-a-feature-off).

### Handle erasure requests

Roblox's right-to-erasure requests are verified and carried out across your data stores on their own. See [Erasure requests](/docs/privacy/erasure-requests).

Next: [Create your account](/docs/getting-started/create-your-account)

---

## Page: /docs/getting-started/create-your-account

**Create your account.** Open your sign-up link, confirm your Discord account and save a passkey.

Signing up is by invitation. Craftsman Staff send you a link made for one Discord username. It lasts 24 hours.

### Sign up

1. Open your link. It goes straight to Discord.
2. Sign in to Discord with the account the link names.
3. Press **Save a passkey**. Your device asks for a fingerprint, face or PIN.
4. Press **Open the console**.

Signing up means you agree to the Terms of Service and have read the Privacy Policy.

### Save a second passkey

A lost passkey cannot be recovered. The last screen offers **Add a second passkey**, so use it while you are there. See [Add a passkey](/docs/team-and-access/add-a-passkey).

### Get on a project

A new account has no access to anything. Staff put you on a project, then an administrator gives you roles. See [Add a member](/docs/team-and-access/add-a-member).

### If it goes wrong

| The screen says | Do this |
|---|---|
| This link is for another Discord account | Switch accounts in Discord, then open the link again. The link still works. |
| Discord sign-in failed | Open the link again. Nothing was created. |
| That was cancelled or timed out | Open the link again. Nothing was created. |
| This sign-up link no longer works | Ask Craftsman Staff for a new one. |
| You already have an account | Sign in with your passkey. |
| This account is frozen | Speak to Craftsman. |
| This browser cannot do this | Use a current Chrome, Safari, Edge or Firefox. |

Next: [Find your way around](/docs/getting-started/find-your-way)

---

## Page: /docs/getting-started/find-your-way

**Find your way around.** The top bar, the sidebar, the place group menu and where each page is.

The console has a top bar and a sidebar. Every project page sits between them. Your projects are listed at `/dashboard/`, most used first.

### Pages

| Page | What it does | Start with |
| --- | --- | --- |
| **Overview** | Shows what needs attention and how the game is doing. | [Read the Overview](/docs/monitoring/read-the-overview) |
| **Configuration** | Switches features and changes config values. | [Change a config](/docs/features/change-a-config) |
| **Players & Data** | Opens players and saved documents, and acts on them. | [Find a player](/docs/players-and-data/find-a-player) |
| **Servers** | Lists live servers. | [See your live servers](/docs/servers-and-commands/see-live-servers) |
| **Commands** | Sends commands to servers. | [Send commands](/docs/servers-and-commands/commands-page) |
| **Purchases** | Shows purchases that failed, and refunds. | [Look up purchases](/docs/store-and-purchases/look-up-purchases) |
| **Privacy** | Handles erasure requests. | [Track an erasure request](/docs/privacy/erasure-requests) |
| **Releases** | Tests and deploys builds. | [Deploy a release](/docs/releases/deploy-a-release) |
| **Store** | Lists passes, products and subscriptions. | [Store catalogue](/docs/store-and-purchases/store-catalogue) |
| **Assets** | Lists uploaded media. | [Assets](/docs/assets/assets) |
| **Health** | Shows your key and your Roblox limits. | [Check health](/docs/monitoring/check-health) |
| **Audit** | Lists every change. | [Search the audit log](/docs/monitoring/search-the-audit-log) |
| **Approvals** | Lists requests waiting on a person. | [Approve a request](/docs/team-and-access/approve-a-request) |
| **Settings** | Holds the project setup. | [General settings](/docs/project-settings/general-settings) |

The sidebar shows a page only if you may read it. **Overview** and **Settings** always show.

If something has gone wrong, go to [Stop something fast](/docs/emergencies).

### The top bar

| Item | What it does |
| --- | --- |
| **Project Control** | Goes to your projects. |
| Project name | Goes to **Overview**. |
| Place group | Changes the place group. |
| Lock | Shows how long your passkey still covers you. See [Tap a passkey once](/docs/concepts/passkey-window). |
| Bell | Shows notifications and requests waiting on you. See [Notifications](/docs/monitoring/notifications). |
| Account menu | Opens **Your account**, **All Projects** and **Sign out**. |

### Place groups

A place group is a set of places that share one configuration, such as Production or Development. A page acts on one group at a time.

The menu shows on **Overview**, **Configuration**, **Players & Data** and **Commands**. Your choice follows you between pages. The address holds it as `?group=`.

| Badge | Means |
| --- | --- |
| **protected** | A deploy needs a passkey and a passing test. |
| **default** | Builds come from the Studio place of this group. |

Changing the group also drops any server you picked. **Manage Place Groups** opens the settings.

### The sidebar

Press **Sidebar** at the bottom to change how it looks. The choice is saved in this browser.

| Mode | What it does |
| --- | --- |
| **Expanded** | Shows icons and names. |
| **Collapsed** | Shows icons only. |
| **Expand on hover** | Shows icons, and opens over the page when you point at it. |

On a phone, **Menu** replaces the sidebar.

### Addresses

A project page is `/dashboard/<project id>/<page>`. You can copy the address and send it to someone on the project.

Next: [Install the CLI](/docs/getting-started/install-the-cli)

---

## Page: /docs/getting-started/install-the-cli

**Install the CLI.** Install the craftsman command and the tools it needs, pinned per project with Rokit.

You install the CLI per project with [Rokit](https://github.com/rojo-rbx/rokit). Everyone on the project, and CI, then runs the same version.

### What you need

| Tool | Why |
|---|---|
| Rokit | Installs and pins the tools below. Install it first, from its README. |
| `craftsman` | The CLI. Builds exist for Windows x86_64, Linux x86_64 and aarch64, and macOS x86_64 and aarch64. |
| Rojo | Builds places and serves code to Studio. `release`, `serve` and `test-roblox` run it. |
| Git | `publish` reads your branch and starts the build from what is pushed. |

You do not need Lune. The binary carries its own runtime.

You do not need Wally unless your game installs its own packages with it. If `wally.toml` lists any, `craftsman init` and `install` run `wally install` for you, then `wally-package-types`. Add both tools to `rokit.toml`.

### Install it

1. Open a terminal in your project's folder.
2. Add the CLI and Rojo:

   ```bash
   rokit add averyark/craftsman-cli@0.11.11 craftsman
   rokit add rojo-rbx/rojo
   ```

   The last word of the first line names the command. Without it, the command is `craftsman-cli`.
3. Check it:

   ```bash
   craftsman --version
   ```

Commit `rokit.toml`. It now holds both tools:

```toml
[tools]
craftsman = "averyark/craftsman-cli@0.11.11"
rojo = "rojo-rbx/rojo@7.7.1"
```

On another machine, or after you pull a change to `rokit.toml`, run `rokit install`. Rokit may ask you to trust a tool the first time.

### Change version

Edit the `craftsman` pin in `rokit.toml`, run `rokit install`, then `craftsman update`. Commit `craftsman.lock` and `craftsman.toml`. Craftsman's Control package always matches the CLI's version. See [Update packages](/docs/project-setup/update-packages).

### On Windows

- Restart your terminal after installing Rokit, so its tools are on your `PATH`.
- Use the exact case of every file and folder name. Windows treats `server/` and `Server/` as one folder, but the release build runs on Linux and does not. The CLI warns about a feature folder that differs only by case.
- If the CLI cannot tell whether its output is a terminal, it prints plain lines and says so once. Pass `--live` to force the moving line, or `--plain` to turn the note off.

Next: [Sign in from the terminal](/docs/getting-started/sign-in-from-the-terminal)

---

## Page: /docs/getting-started/sign-in-from-the-terminal

**Sign in from the terminal.** Sign the CLI in with your passkey, check who it is signed in as, and choose which console it talks to.

The CLI acts as you. You need to sign in to install packages, publish and run tests in Roblox. 
### Before you start

You need a console account with a passkey, CLI 0.11.7 or newer, and to be on the project. See [Create your account](/docs/getting-started/create-your-account). Craftsman Staff add people to a project.

### Sign in

1. Run:

   ```bash
   craftsman login
   ```

2. Your browser opens the console. Tap your passkey, then go back to the terminal. The browser must be on the same computer, so over SSH, forward the port the CLI prints first. The CLI stops waiting after five minutes.
3. Check that your project is listed:

   ```bash
   craftsman whoami
   ```

`whoami` shows your name, when this machine signed in, when the session ends, and every project you are on. If the project is not listed, ask Craftsman Staff to add you.

A session ends after a stretch unused, and at a fixed date at the latest. When it ends, the next command says so. Run `craftsman login` again.

### Sign out

```bash
craftsman logout
```

This ends the session in the console and removes it from this machine. If the console cannot be reached, it is still removed locally. It stays live until it expires, or until you revoke it from your Account page.

The session lives in `~/.craftsman/session`, or `%USERPROFILE%\.craftsman\session` on Windows. There is one per console.

### Choose a console

The CLI talks to the hosted console by default. Every command that reaches a console names it on its first line:

```
[Publish] to https://operations.craftsman.systems (hosted)
```

Most projects never change this. To point somewhere else, set `CONTROL_ENDPOINT`. The CLI reads it from your shell, then the project's `.env`, then `endpoint` under `[console]` in `craftsman.toml`.

> [!NOTE]
> A file in the project may name only the hosted console or this machine. Any other host is refused with exit code 2, so a cloned repository cannot send your sign-in elsewhere. To reach another host, set it in your own shell.

### Sign in from GitHub Actions

CI does not run `craftsman login`. The release workflow signs in with the run's GitHub OIDC token, from the repository your project names for releases. See [The release workflow](/docs/releases/release-workflow).

GitHub still builds your releases, so `craftsman publish --apply` also needs your own GitHub login on this machine. See [publish](/docs/cli/publish-and-release).

Next: [Initialize a project](/docs/getting-started/initialize-a-project)

---

## Page: /docs/getting-started/initialize-a-project

**Initialize a project.** Run craftsman init to install Craftsman and write the files a feature-routed game needs.

`craftsman init` installs Craftsman's packages and writes everything else your game needs. It never overwrites a file, so you can run it again.

You need the [CLI](/docs/getting-started/install-the-cli), Rojo and your start place's ID. You also need to be [signed in](/docs/getting-started/sign-in-from-the-terminal) with `craftsman whoami` listing your project, because `init` downloads the packages from the console.

If your game still installs Craftsman with Wally, `init` refuses. Run `craftsman update`. See [Update packages](/docs/project-setup/update-packages).

### Run it

From the project's folder:

```bash
craftsman init --place 137372027987942
```

`init` looks up the place's universe. If that fails, pass `--universe <id>` too.

To add a second place later, give it a role, up to 32 lowercase letters, digits and hyphens, starting with a letter:

```bash
craftsman init --place 140212345678901 --role lobby
```

If `init` prints **Still to do**, work through that list.

If `wally.toml` lists packages of your own, `init` installs and types them too.

### See what it writes

Commit these:

| File | What it is |
|---|---|
| `craftsman.toml` | Settings, each commented out with its default, and places. See [craftsman.toml reference](/docs/project-setup/craftsman-toml). |
| `craftsman.lock` | The exact package versions. |
| `base.project.json` | The Rojo project you edit. |
| `src/Features/` | One folder per feature. |
| `src/Control/Definitions.luau` | The declaration, naming your universe. |
| `src/Control/Stores.luau` | Opens your entity stores. It must stay a ModuleScript. |
| `src/Start.server.luau` | Starts the runtime. |
| `src/Bootstrap.server.luau`, `src/Bootstrap.client.luau` | Start the kit, and load your `Handler`s and `Controller`s. |
| `.github/workflows/release.yml` | The release workflow. |

`CraftsmanPackages/`, `CraftsmanDevPackages/`, `default.project.json`, `release.project.json` and `sourcemap.json` are generated and gitignored. `init` adds any missing lines to `.gitignore` and keeps yours.

### Start working

1. Run `craftsman serve` and connect Studio's Rojo plugin. See [Feature folders](/docs/project-setup/feature-folders).
2. Add a feature with `craftsman feature Shop The in-game shop`.

Next: [Connect your universe](/docs/getting-started/connect-your-universe)

---

## Page: /docs/getting-started/connect-your-universe

**Connect your universe.** Add your Roblox universe to a project, store its game key, save an Open Cloud key, and pin the signing key.

Connect your game's universe from the console. Craftsman Staff register the project first. Changing keys needs `craftsman.settings.secret`.

### Add the universe

1. Open **Settings**, then **Universes**.
2. Press **Add universe**.
3. Enter the universe ID from the Creator Hub, not a place ID, and a name.
4. Press **Add**.

Every place group starts in the first universe you add. See [Use separate universes](/docs/project-settings/separate-universes).

### Store the game key

The game key is shown once. If you do not see one, open the universe with **Manage** and press **Create game key**. **Rotate game key** replaces it, and the old one stops working at once.

1. Copy the key.
2. In Studio, open **Game Settings**, then **Security**, then **Secrets**.
3. Add it as `CRAFTSMAN_CONTROL`.
4. Set its domain to the one the console shows. Any other domain fails silently.

Local playtests cannot read secrets. Test from Team Test or a live server, or add the key to Local Secrets.

### Save an Open Cloud key

1. Press **Manage** on the universe.
2. Open **Permissions this key needs** and make the key on the Creator Hub. List every universe of this project under each system.
3. Paste the key and press **Save & test**.

The test lists anything missing. **Copy missing** gives you the scopes to add. One key can serve several universes if each one is on its list.

### Map a place

Open **Settings**, then **Place groups**, and map a place to a group under **Map a place**. Until a group has a place, changes save but reach no game. See [Place groups](/docs/project-settings/place-groups).

### Pin the signing key

Signing stops anyone who can write to your universe's DataStores or MessagingService from pretending to be the console. Do this after your first release is out.

1. Open **Settings**, then **Signing**.
2. Press **Sign now** on **Sign what is published**, if it is offered.
3. Copy the `SigningKey` line into your [declaration](/docs/getting-started/the-declaration), beside `Universe`.
4. Release and publish.

> [!WARNING]
> Finish step 2 first. A server that pins the key refuses anything unsigned, and runs on its code values.

Each place group pins the key on its own, because each can run a different release. The page shows every group's state. The key does not rotate.

Next: [Describe your places](/docs/getting-started/describe-your-places)

---

## Page: /docs/getting-started/describe-your-places

**Describe your places.** Tell the CLI how to build each place, with a [places.<role>] table in craftsman.toml.

A release is your Studio place with your code laid over it. Each place a release builds is a `[places.<role>]` table in `craftsman.toml`. `craftsman init --place <placeId>` writes the first one.

### Write the minimum

With the layout `init` writes, a place needs two IDs:

```toml
[places.main]
universe = 10202097921
place = 123021532395166
```

The role, `main` here, matches this place to the place with the same role in each place group. One build is made per role and handed to every group's copy. A role is up to 32 lowercase letters, digits and hyphens, starting with a letter.

### Set other keys

| Key | Default | What it does |
|---|---|---|
| `universe`, `place` | Required | What this builds. The declaration must name the universe. |
| `source` | `"control"` | `"control"` uses the Studio place the console holds. `"studio-download"` uses a place file you commit. |
| `baseline` | None | For `"studio-download"`: the committed `.rbxl`, saved with **File → Download a Copy**. |
| `overlay` | `release.project.json` | The Rojo project that builds your code. |
| `definitions` | `src/Control/Definitions` | The declaration module's path, without `.luau`. |
| `owned` | None | Folders your code owns beyond what the overlay maps. |

Any other key is refused.

### List what a place owns

A build replaces what the place owns, and checks nothing outside it changed. With the generated overlay, the place owns everything `release.project.json` maps. List `owned` only for something it does not map.

With your own Rojo project as `overlay`, `owned` must list every folder:

```toml
[places.main]
universe = 10202097921
place = 123021532395166
source = "studio-download"
baseline = "places/main.rbxl"
overlay = "game.project.json"
owned = ["ReplicatedStorage/CraftsmanPackages", "ReplicatedStorage/CraftsmanDevPackages", "ReplicatedStorage/Craftsman", "ServerScriptService/Craftsman"]
```

> [!WARNING]
> With your own overlay, include `ServerScriptService/Craftsman`. The check that your `Stores` ModuleScript exists only runs when that folder is owned.

### Commit a place file

A `"studio-download"` baseline must be tracked by Git LFS, or the build stops. Add this to `.gitattributes`:

```
*.rbxl filter=lfs diff=lfs merge=lfs -text
```

### Build a place locally

`craftsman release <role>` builds on your machine and sends nothing. For a `"control"` place, pass the Studio place file:

```bash
craftsman release main --baseline main.rbxl
```

See [Publish and release commands](/docs/cli/publish-and-release).

### Move an older project

Older projects keep each place in `places/<role>.place.json`. `craftsman install` moves each one into `craftsman.toml` and deletes it. `craftsman doctor` fails while one is left.

Next: [Write the declaration](/docs/getting-started/the-declaration)

---

## Page: /docs/getting-started/the-declaration

**Write the declaration and start the runtime.** Write the module that tells Project Control what your game has, then start the runtime on servers and clients.

Your game has one declaration: a module that returns `Control.Declare({...})`. It lists everything the console can see and act on. [`craftsman init`](/docs/getting-started/initialize-a-project) writes it at `src/Control/Definitions.luau`, mapped to `ReplicatedStorage.Craftsman.Definitions`.

### Write the declaration

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Shop = require("../Features/Shop/Definitions")
local Player = require("../Features/Player/Definitions")
local Receipts = require("./Receipts")

return Control.Declare({
	Universe = 10202097921,
	EventCeiling = 500,
	SigningKey = "Da1xjlC8EDFNInfRMZGdbqZSfwzxq/mCtXRb8XoLJLM=",
	Receipts = Receipts,
	Includes = {
		Shop.Definition,
		Player.Definition,
	},
})
```

Each scope you declare has a `.Definition`. Add it to `Includes` and everything under that scope is declared. [`craftsman feature`](/docs/cli/setup-commands) adds the require and the entry for you.

| Field | Required | Meaning |
|---|---|---|
| `Universe` | Yes | The universe ID, never a place ID. Use a list, `{ a, b }`, when the code runs in several universes. |
| `EventCeiling` | Yes | The most status events one server may report in an hour. `500` is a good start. |
| `Includes` | No | The `.Definition` of each top-level scope. |
| `Receipts` | No | What each developer product's receipt grants. See [Sell passes, products and subscriptions](/docs/store-and-purchases/store-catalogue). |
| `SigningKey` | No | The public key from [Signing](/docs/getting-started/connect-your-universe). With it, a server refuses unsigned configuration. |
| `Entities`, `Operations` | No | Low-level descriptors. You rarely need these. |

The declaration errors at once on a mistake, naming the field. It also runs outside Roblox when you publish, so keep it to declarations.

### Start the server

The Script at `ServerScriptService.Craftsman.Start` starts reporting and the runtime once:

```lua
local HttpService = game:GetService("HttpService")

local Control = require("@game/ServerScriptService/Craftsman/Control/Stores")
local Definitions = require("@game/ReplicatedStorage/Craftsman/Definitions")

Control.Reporter.Start({
	declaration = Definitions,
	secret = HttpService:GetSecret("CRAFTSMAN_CONTROL"),
	endpoint = "https://operations.craftsman.systems/api",
	ceiling = Definitions.event_ceiling,
})

local Runtime: Control.Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
	Registry = true,
})
```

`CRAFTSMAN_CONTROL` is the game key, stored as a Roblox secret. It never goes in a file.

| Option | Meaning |
|---|---|
| `OnEvent` | Where status events go. Use `Control.Reporter.Emit`. |
| `FetchCommands` | How durable commands are collected when a player joins. Use `Control.Reporter.Commands`. |
| `Registry` | `true` lists this server on the **Servers** page, so server commands and overrides can reach it. |
| `Access` | `false` turns off in-game access and `Control.Run`. On by default. |
| `DocumentTimeout` | Seconds a feature with `WaitForDocument` waits for the console's values. Default 30. |

`Control.Start` errors on a client and in a universe the declaration does not name. A second call returns the first handle. The handle has `Revision()`, `IsReady()` and `OnReady(fn)`.

> [!WARNING]
> Register command handlers before `Control.Start`. A command that arrives with no handler is skipped, and the console reports it as failed.

### Start clients

Call `Control.StartClient()` once from a LocalScript, after requiring the declaration. Without it, no client feature runs, and nothing warns you.

```lua
local Client = Control.StartClient({ DocumentTimeout = 10 })

Client.OnReady(function()
	LoadingScreen:Hide()
end)
```

Next: [Publish your first manifest](/docs/getting-started/publish-your-first-manifest)

---

## Page: /docs/getting-started/publish-your-first-manifest

**Publish your first manifest.** Check your declaration with the console, then make it live in a place group.

The CLI turns your declaration into a manifest. You will check it, then make it live in one place group.

### Preview it

```bash
craftsman publish
```

This checks the manifest against your default place group and prints what would change. It also previews the release. Nothing changes.

### Make it live in one group

```bash
craftsman publish --group Development --activate --no-build --apply
```

- `--group` names the place group.
- `--activate` changes that group's live declarations.
- `--no-build` skips the release build.
- `--apply` does it. Without it, you get a dry run.

The terminal says `Activated in` and names the group. No other group changes.

If a group lives in another universe, add `--universe <id>`.

### Confirm a protected group

A protected group waits for a passkey. The terminal prints a link, so open it and confirm in your browser. The terminal carries on when you do. If you use the CLI only, send the link to someone who holds the permission.

Ctrl-C stops the terminal, not the request. It stays in **Approvals** until it expires or someone declines it.

Removals are also held until someone confirms them in the console.

### Check it

1. Open **Settings**, then **Manifest**. Each group that has been published has a row.
2. Open **Configuration** and pick the group at the top. Your features are listed.

If you see **No place mapped**, changes save but reach no game. Press **Map a place**.

### Build a release

```bash
craftsman publish --apply
```

This builds a release on GitHub for every place group, tests it in Roblox, and activates nothing. Its declarations go live in each group when you deploy it there.

Next: [Build a release](/docs/releases/build-a-release)

---

# Collection: Stop something fast

Turn off what is breaking your game, right now.

---

## Page: /docs/emergencies/turn-a-feature-off

**Turn a feature off.** Stop one feature on every server in a place group. No passkey.

Off needs no passkey. You need `craftsman.config.feature`.

### Turn it off

1. Open **Configuration**.
2. Pick the place group in the top bar.
3. Find the feature, and press **⋯** on its row.
4. Press the **Active** switch.
5. Press **Save changes**.

Servers pick it up within seconds. One that misses the message catches up within a minute.

Features under it stop too.

In Discord: `/feature key:<key> state:off place_group:Production`. See [Stop it from Discord](/docs/emergencies/stop-from-discord).

### Cannot switch it

The row says **locked in code** if the feature sets `CanOverride = false`. The console cannot change it. Ship a fix.

### Turn it back on

Same steps. On asks for a passkey. See [Turn a feature on](/docs/features/turn-a-feature-on).

---

## Page: /docs/emergencies/use-a-fallback

**Use a fallback.** Run code values instead of hosted values when a config change broke the game. No passkey.

A fallback runs your code values. Your hosted values are kept. It does not turn features off, and does not beat a server override. It never expires.

### Cover a whole place group

1. Open **Configuration** and pick the place group in the top bar.
2. Press **Use fallback**, or **Use fallback in** your group.
3. If a menu opens, pick the group, or **Everywhere** for all groups.

It applies at once. One group needs `craftsman.config.fallback`. **Everywhere** needs Superpower or Craftsman Staff.

### Cover one feature or config

1. Open its row. On a feature, press **⋯**.
2. Press **Use fallback**.
3. Press **Save changes**.

A feature's fallback covers every config below it. You need `craftsman.config.fallback`.

In Discord: `/fallback scope:group:Production`. See [Stop it from Discord](/docs/emergencies/stop-from-discord).

### Stop it

- On a group, press **Stop** on its banner.
- For the whole project, press **Stop fallback →** on the project banner.
- On a row, press **Stop fallback**, then **Save changes**.

It asks for a passkey, because it puts hosted values back.

---

## Page: /docs/emergencies/halt-operations

**Halt operations.** Refuse every game operation on a project at once. Configuration and reads keep working. No passkey.

A halt refuses every operation on your game: data changes, commands and bans. Configuration, reads and attaching or detaching accounts keep working. You need Superpower or Craftsman Staff. No passkey.

Requests waiting out a delay do not run during a halt. They can run once you resume.

### Halt

1. Open **Overview**.
2. Press **Halt all operations**.
3. Add a reason if you have one.
4. Press **Halt operations in** your project's name.

A banner appears above every tab, and it is announced in the project's Discord.

In Discord: `/halt-ops`.

### Resume

Press **Resume operations →** on the banner. You need `craftsman.operations.resume`, and it asks for a passkey. Discord cannot resume.

---

## Page: /docs/emergencies/freeze-someone

**Freeze someone.** Lock a person out right now, the CLI and running games included. No passkey.

Takes effect now, with no passkey. The CLI and running games are included. You need Superpower or Craftsman Staff.

- **Suspend** locks them out of this project. Other projects are unaffected.
- **Freeze** locks them out of every project. Only Craftsman Staff can freeze.

### Do it in the console

1. Open **Settings**, then **Members**.
2. Press the three dots on their row.
3. Press **Suspend on this project…** or **Freeze account…**.
4. Press **Suspend now** or **Freeze now**.

### Do it in Discord

```
/freeze user:@them
```

From an administrator, this suspends. From Craftsman Staff, this freezes. It also works on yourself.

### Lift it

Press the three dots on their row, then **Lift suspension** or **Unfreeze account**. It asks for a passkey.

Another administrator or Craftsman Staff lifts a suspension. Only Craftsman Staff lift a freeze. Nobody can lift their own.

---

## Page: /docs/emergencies/stop-from-discord

**Stop it from Discord.** The bot's brakes and what each option does. They apply at once, with no passkey.

Run these in your project's Discord server. Brakes apply at once, with no passkey.

> [!WARNING]
> Always pass `place_group`. Without it, the command acts on your project's first group, usually Development.

### Stop something

| Command | What it stops |
|---|---|
| `/feature key:<key> state:off place_group:Production` | One feature |
| `/fallback scope:<scope> place_group:Production` | Hosted values, back to code values |
| `/halt-ops` | Every operation on the project |
| `/freeze user:@them` | One person |

`/fallback`'s `scope` is a config or feature key, `group:<name>`, or `project`.

Each command also takes an optional `reason`. The audit channel gets the same answer you do.

### See what is off

| Command | What it shows |
|---|---|
| `/list-features place_group:Production` | Features off, fallbacks on, values held |
| `/list-users` | Who can touch this project, and who is frozen |

### Undo it

Discord cannot ask for a passkey, so undoing happens in the console.

- `/feature state:on` and `/fallback action:clear` answer with a console link and change nothing.
- `/feature state:default` answers with a link if it would switch the feature on. Otherwise it applies at once.
- A freeze and a halt are lifted only in the console.

### If the bot asks or refuses

- With several projects on one server, the bot asks which one, unless you are in that project's command channel.
- `place_group` offers only Development, Testing and Production. For a renamed group's fallback, use `scope:group:<name>`. For a renamed group's feature, use the console.
- You need the same permission as in the console.

---

# Collection: Features and configuration

Declare features and configs, then change them live.

---

## Page: /docs/features/features

**Declare a feature.** Make part of your game a feature, so you can switch it off from the console without a release.

Make part of your game a **feature**, and the console can switch it on and off. Its switch is `Active`.

### Declare it

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")

local Shop = Control.Scope("Shop"):Feature({
	Client = true,
	Description = "The in-game shop.",
})

return Shop
```

Or let the CLI write it. This adds `src/Features/Shop/Definitions/init.luau` and puts `Shop.Definition` in your declaration's `Includes`:

```bash
craftsman feature Shop The in-game shop. --client
```

See [Feature folders and routing](/docs/project-setup/feature-folders) for the rest of the folder.

| Option | Default | Meaning |
|---|---|---|
| `Active` | `true` | The code value. `false` ships the feature off. |
| `CanOverride` | `true` | `false` locks `Active` to the code value. The console and server overrides are ignored. |
| `Client` | `false` | The feature also runs on clients. |
| `WaitForDocument` | `false` | Hold the feature until the console's values load, or `DocumentTimeout` passes. |

`Scope("Shop")` has the key `shop`. Rename a scope and you get a new key. The old key's console settings do not follow.

> [!TIP]
> Do not declare an `Enabled` config. `Active` is the switch the console, the Discord bot and the attention list understand.

### Run code while it is active

```lua
Shop:OnActivate(function(keeper)
	local stand = keeper:Add(ShopStand:Clone())
	stand.Parent = workspace
	keeper:Connect(Players.PlayerAdded, GreetShopper)
end)

Shop:OnDeactivate(function()
	print("The shop closed")
end)
```

- `OnActivate` runs every time the feature switches on, and at once if it is already on.
- Create and connect through the `keeper`. It is cleaned when the feature switches off.

### Check it at request time

```lua
local function Buy(player, item)
	if not Shop.Active then
		return false, "The shop is closed"
	end
	-- ...
end

Shop:ObserveActive(function(active)
	ShopButton.Visible = active
end)
```

`ObserveActive` calls your function at once, then on every change.

Until the runtime starts, `Active` reads `false` and no handler runs. Call `Control.Start` on the server and `Control.StartClient` on each client.

### Use step events

```lua
Shop.Heartbeat:Connect(function(dt)
	SpinSign(dt)
end)

Shop.Heartbeat:Connect(TrackIdle, { Disconnect = false }) -- connected whatever the state
```

Step events connect only while the feature is active.

- Both sides: `PreSimulation`, `PostSimulation`, `Heartbeat`, `Stepped`.
- Clients only: `PreRender`, `PreAnimation`, `RenderStepped`. Reading one on the server errors.

### Add child features

Declare a child with `:Scopes`:

```lua
local Seasonal = Control.Scope("Seasonal"):Feature():Scopes({
	Banner = Control.Scope("Banner"):Feature({ Client = true }),
})

print(Seasonal.Banner.Active)
```

A child runs only while its parent runs. It keeps its own switch while its parent is off.

### Know when values have loaded

A server starts every feature on its code values. `Shop:IsReady()` and `Shop:OnReady(fn)` tell you when the console's values arrive.

Use `WaitForDocument` only when running on code values would be wrong. After `DocumentTimeout` seconds, 30 by default, the feature starts on its code values and warns.

Only `Client` features run on clients. They wait for the server's state, so a feature that is off never flickers on.

Next: [Add configs to a feature](/docs/features/configs)

---

## Page: /docs/features/configs

**Add configs to a feature.** Declare typed values on a feature, and change them in the console without a release.

Add a **config** to a feature: a price, a rate, a radius, a message. Give it a type and a code value, and you can change it live.

### Declare configs

Call `:Config` after `:Feature`. Every config belongs to a feature.

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

local Shop = Control.Scope("Shop"):Feature({ Client = true }):Config({
	DiscountPercent = Control.Config(gt.number({ integer = true, range = "[0, 75]" }), {
		Default = 0,
		Shared = true,
		Description = "Store item discounts.",
	}),
	Prices = Control.Config(gt.dictionary(gt.string(), gt.number({ integer = true })), {
		Default = { Sword = 100, Shield = 150 },
	}),
})
```

| Option | Meaning |
|---|---|
| `Default` | Required. The code value. It must fit the type. |
| `Shared` | `true` sends the value to clients. Unshared values never leave the server. |
| `Description` | Shown in the console. |

The key comes from the scope path: `Shop.DiscountPercent` is `shop.discount_percent`.

> [!NOTE]
> Players can read feature and config names, because the declaration sits in `ReplicatedStorage`. Never put a secret in a name.

### Pick a type

| GreenTea | In the console |
|---|---|
| `gt.boolean()` | On or off |
| `gt.number({ integer, range = "[0, 75]" })` | A number |
| `gt.string({ bytes = "[1, 80]", pattern })` | Text |
| A union of `gt.literal`s | One of the options |
| `gt.table({...})`, `gt.array(x)`, `gt.dictionary(gt.string(), x)` | An object or a list |
| `gt.Color3()`, `gt.Vector3()` | A colour, or three numbers |

- A config cannot be `gt.optional` as a whole. A field inside a table can.
- No table in it can be empty.
- `DateTime` and `EnumItem` are refused. Use Unix seconds or an ISO string, and a union of literals of the enum item's names.

### Read a value

```lua
local percent = Shop.DiscountPercent:Get()

local stop = Shop.DiscountPercent:Observe(function(value, source)
	SetDiscount(value)
end)
```

- `:Get()` returns the current value, typed from the GreenTea type.
- `:Observe(fn)` calls `fn` at once, then on every change of value or source. It returns a function that stops it.
- Table values are frozen.
- Configs stay readable while their feature is off.
- Before the runtime starts, both answer the code value.
- On a client, only `Shared` configs of `Client` features can be read. Anything else errors.

### See where a value comes from

A config takes the first of these that is set:

1. **A server override**, for [one server](/docs/features/override-one-server).
2. **[Use Fallback](/docs/emergencies/use-a-fallback)**, which serves the code value.
3. **The value set in the console.**
4. **The code value.**

`source` in `Observe` names which one: `"override"`, `"fallback"`, `"hosted"` or `"default"`.

A feature's `Active` has no fallback step: server override, then the console, then the code value. With `CanOverride = false`, the code value always wins.

Next: [Declare configs where they are used](/docs/features/declare-where-used)

---

## Page: /docs/features/declare-where-used

**Declare configs where they are used.** Declare a config or command in the file that uses it, and let the CLI copy it into the feature's definitions.

Declare a config or command in the file that uses it. The CLI copies the declaration into `Definitions/Generated.luau`, and only that file is published.

### Set up the feature

[`craftsman feature`](/docs/cli/setup-commands) writes a feature that adopts its generated file. For `craftsman feature Grass Procedural grass.`:

```lua
-- src/Features/Grass/Definitions/init.luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Generated = require("@self/Generated")

return Control.Scope("Grass"):Feature({ Description = "Procedural grass." }):Scopes(Generated.Scopes)
```

> [!WARNING]
> Write `@self/Generated`, never `./Generated`. From an `init.luau`, `./` points at the folder's siblings, so the generated scopes are never adopted.

### Declare in the file that uses it

Call `:Declare(name, spec)` at the top level of the file:

```lua
-- src/Features/Grass/Client/Bending.luau
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Grass = require("@game/ReplicatedStorage/Features/Grass/Definitions")
local gt = Control.GreenTea

local PUSH = Grass:Declare("Push", {
	Feature = { Description = "Grass bending away from characters." },
	Config = {
		Radius = Control.Config(gt.number({ range = "[0, 20]" }), { Default = 3.8 }),
	},
})

local radius = PUSH.Radius:Get()
```

- `spec.Feature`, `spec.Config` and `spec.Commands` take what `:Feature`, `:Config` and `:Commands` take.
- The CLI copies only the `Declare` call and what it names.
- Declare a child on its parent: `Grass.Push:Declare("Tips", {...})`.

Then run [`craftsman project`](/docs/cli/setup-commands), or keep `craftsman serve` running. Commit `Generated.luau` with your change.

### Attach a command's handler

A handler is never copied into the generated file. Attach it where its state lives:

```lua
local REGROW = Grass:Declare("Regrow", {
	Commands = {
		Now = Control.Command({ Radius = gt.number() }).NoImpact().Tier("routine"),
	},
})

REGROW.Now.Handles(function(directive)
	World.Regrow(directive.params.Radius)
	return true
end)
```

A second handler for one command errors.

### Keep the spec copyable

A spec may use only:

- literals, tables and function literals
- top-level locals bound to those, and `local function`s and `type` aliases the spec names
- `require`s of `CraftsmanPackages`, your own `Packages`, or modules in this feature's `Definitions/` folder
- Luau's globals, `Color3` and `Vector3`

The CLI names the file and line when it refuses a Roblox global such as `game` or `Enum`, or a local built from one. It also refuses a `Declare` inside a function or block or on another feature's scope, `.Handles` inside the spec, and two files declaring the same name.

Entities cannot be declared this way yet. Declare them in `Definitions`.

### Fix a stale file

`craftsman release` and [`craftsman publish`](/docs/cli/publish-and-release) refuse a `Generated.luau` that no longer matches your files, or one `Definitions` does not adopt. Run `craftsman project`.

### Name Roblox globals in declaration modules

Declaration modules also run outside Roblox when the CLI publishes. There, Roblox-only globals are stand-ins. A module may **name** them at its top level, but never **read** them.

```lua
local Players = game:GetService("Players") -- fine: named, not read

print(#Players:GetPlayers()) -- stops the publish: read at the top level
```

Handlers and reducers can use them freely. `const` is still refused at the top level of a declaration module.

> [!WARNING]
> A stand-in is always truthy. `if workspace:FindFirstChild("Map") then` at the top level takes its first branch when the CLI loads the module, so never branch on one there.

---

## Page: /docs/features/change-a-config

**Change a config.** Edit a config's value in the console, for every server in a place group, without a release.

Change a config's value for every server in a place group. You need `craftsman.config.write`. A value edit does not ask for a passkey.

### Edit the value

1. Open **Configuration**.
2. Pick the place group in the top bar.
3. Click a feature to unfold it, or type in **Filter**.
4. Click the config. Its editor opens under **Hosted value**.
5. Change the value. The form follows the config's type.
6. Press **Save changes** in the bar at the bottom.

Nothing changes until you save. See [Review and save your changes](/docs/features/publish-a-draft).

Running servers take the new value without a restart. A save pings them, and they also check every 30 to 60 seconds. After a large change in a busy game, servers spread their reads over up to 10 minutes.

A value the type refuses shows its problem beside the field. Fix it or undo it before you can save.

### Go back to the code value

1. Open the config.
2. Press **Use code value**.
3. Press **Save changes**.

This removes the hosted value. **Use code value** appears only once a hosted value exists.

### Read a config's row

The row shows the value servers run, and under it where that comes from:

| Label | Meaning |
|---|---|
| **set here** | The hosted value. |
| **from code** | No hosted value, so the code value. |
| **fallback** | [Use Fallback](/docs/emergencies/use-a-fallback) covers it. It may name what set it. Edits still save. |
| **held** | The stored value does not fit the type any more. |
| **unsaved** | You have changed it and have not saved. |

A **shared** tag means clients get the value. An **overridden** tag means some servers [run their own value](/docs/features/override-one-server).

### Fix a held value

A value is held when it no longer fits the config's type, usually after you change the declaration. A banner lists each one and why. Servers run the code value until you enter a new value and save.

### Save to a group with no place

If you see **No place mapped to** your group, changes save but reach no game. Press **Map a place** to fix it. See [Place groups](/docs/project-settings/place-groups).

Next: [Review and save your changes](/docs/features/publish-a-draft)

---

## Page: /docs/features/publish-a-draft

**Review and save your changes.** See every unsaved change on Configuration, undo what you do not want, and save them together.

Every change on **Configuration** is a draft until you save. Check them, then send them all at once.

### Review and save

1. Make your changes. A bar appears at the bottom: **1 unsaved change in** your place group.
2. Press **Review changes**. Each change shows its before and after.
3. Press **Undo** on any change you do not want.
4. Press **Save changes**.
5. Tap your passkey if the bar says **Asks for your passkey**.

All your drafts go in one save. One passkey covers them all.

### Know what asks for a passkey

| Change | Passkey |
|---|---|
| Edit a value, or use the code value | No |
| Switch a feature off | No |
| Use a fallback on a config or feature | No |
| [Switch a feature on](/docs/features/turn-a-feature-on) | Yes |
| Stop a fallback | Yes |

A tap covers the next 10 minutes in the same browser. See [The passkey window](/docs/concepts/passkey-window).

The fallback for a whole place group or the project does not wait for a save. It acts at once. See [Use a fallback](/docs/emergencies/use-a-fallback).

### Throw drafts away

Press **Discard** to drop every draft in this place group.

Drafts live in this browser tab. Each place group keeps its own, so you can switch groups and come back. Each saves or discards on its own. If you leave the page with drafts, your browser asks first.

### Handle a refused save

- **`N to fix first`** means a value does not fit its type. **Save changes** stays disabled until you fix or undo it.
- A refused change stays a draft. Its row opens and says why.
- **Changed by someone else** means another person saved that value first. The diff now compares against theirs. Check it and save again.
- **not pushed** in the message means the change is stored but did not reach servers. The message says why.

Every save shows in [Audit](/docs/monitoring/search-the-audit-log).

---

## Page: /docs/features/override-one-server

**Override one server.** Change a config or a feature on one running server, from the console or in game.

Change a config or switch a feature on one running server. You need `craftsman.config.override`. It does not ask for a passkey.

An override beats the hosted value and any fallback. It ends when you clear it or the server closes.

### Override from the console

The console reaches only a server started with `Registry = true`.

1. Open **Servers** and pick a server.
2. Press **Configure this server**.
3. Change a value, or flip a feature's switch.
4. Press **Save to this server**.

A refused change stays a draft and says why. The rest still go.

Within a minute, the row reads **overridden here**. The value shows as a dash, because a server does not report the values it runs.

Press **Remove** to work on the whole place group again.

### Clear an override

1. Open the row on the same server.
2. Press **Clear override**.
3. Press **Save to this server**.

Clearing a feature also clears every override under it.

### See overrides from the group view

With no server picked, a row with overrides has an **overridden** tag. A fallback does not reach those servers.

### Override in game

You need your Roblox account [attached to the project](/docs/team-and-access/attach-an-account). In the [Konsole](/docs/servers-and-commands/use-the-konsole), these need `craftsman.config.read`:

| Type | Does |
|---|---|
| `features` | Lists every feature, `on` or `off`, and why |
| `configs` | Lists every config, its value and source |
| `configs <key>` | Lists a feature's configs, or one config |

These need `craftsman.config.override`:

| Type | Does |
|---|---|
| `override <config> <value>` | Overrides one config |
| `override <feature> on` | Switches one feature, `on` or `off` |
| `clearoverride <key>` | Clears one override, and everything under a feature |
| `clearoverrides` | Clears every override on this server |

Or from your own code:

```lua
Control.Override(player, "shop.discount_percent", "25")
Control.Override(player, "shop", "off")
Control.ClearOverride(player, "shop.discount_percent")
Control.ClearOverride(player) -- everything
```

The value is text, read against the config's type:

- A number, or `true`, `false`, `on`, `off`.
- One of an enum's options.
- A string. One pair of surrounding quotes is removed.
- A colour as `#rrggbb` or `r, g, b`. A vector as `x, y, z`.
- Anything else as JSON.

These are refused, and nothing runs: text that cannot be read, a value that does not fit the type, and an override of a `CanOverride = false` feature.

Overrides show in [Audit](/docs/monitoring/search-the-audit-log).

---

## Page: /docs/features/turn-a-feature-on

**Turn a feature on.** Switch a feature back on from Configuration. It asks for your passkey.

Switch a feature on for every server in a place group. You need `craftsman.config.feature` and your passkey.

### Switch it on

1. Open **Configuration**.
2. Pick the place group in the top bar.
3. Press **⋯** on the feature's row.
4. Flip the switch under **Active**.
5. Press **Save changes**.
6. Tap your passkey.

The passkey is there because someone may have switched it off for a reason. A tap covers the next 10 minutes in the same browser. See [Tap a passkey once](/docs/concepts/passkey-window).

Switching off asks for nothing. See [Turn a feature off](/docs/emergencies/turn-a-feature-off).

### Go back to the code value

Press **Use code value** beside the switch, then save. It asks for a passkey only if that turns the feature on.

### Start from Discord

`/feature key:<key> state:on` changes nothing. Discord cannot ask for a passkey, so the bot replies with a link. The link opens this feature's row in the console. Finish the steps there.

### Check why it is still off

A feature's row shows **on** or **off**, and where that comes from:

| Label | Meaning |
|---|---|
| **set here** | Someone set it in the console. |
| **from code** | It runs the code value. |
| **locked in code** | It is declared `CanOverride = false`. There is no switch. Change it in code. |
| **off by** a key | A parent feature is off. Turn the parent on first. |

A child keeps its own switch while its parent is off. It comes back as it was.

You see **No permission to switch** without `craftsman.config.feature`.

---

# Collection: Players and data

Declare your data, then read and fix it from the console.

---

## Page: /docs/players-and-data/entities

**Declare an entity.** Declare saved data, like a player profile, so the console can read and revert it.

An **entity** is saved data the console reads by key: a player profile, an inventory, a world document. You declare its shape once, and it changes only through [reducers](/docs/players-and-data/reducers).

Data entities live in a DataStore, saved with Ledger. For sorted maps and queues, see [Declare a MemoryStore entity](/docs/players-and-data/memorystore-entities).

### Declare it

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

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
```

Then add reducers and return `Profile:Reducers({...})`. An entity needs at least one.

| Option | Meaning |
|---|---|
| `State` | Required. The whole shape, as `gt.build(gt.table({...}))`. |
| `Store` | Required. `{ Name = "..." }`, the DataStore's name. `Scope` may be left out, or be `"global"`. |
| `Default` | Required. The document a new key starts from. It must fit `State`. |
| `KeyedBy` | `"Player"` is the default. It keys by UserId, with a session per player. `"String"` takes any key, written by key. |
| `Label` | The name in the console. |
| `Shared` | Fields other players may receive. See [Replicate entities to clients](/docs/players-and-data/replicate-to-clients). |
| `Owner` | Fields the player receives about themselves. Without it, they get every field not starting with `_`. |
| `Migrations` | Save-data migrations, in order. |

> [!WARNING]
> `Store.Name` must be the store your game really writes. A different name lists an empty store in the console, with no error.

Mark a currency field with `gt.meta(type, { Currency = true })`, and the console shows it as one.

### Add a migration

A migration upgrades an old document to the current shape. List them oldest first:

```lua
Migrations = {
	{
		Run = function(state)
			local upgraded = table.clone(state)
			upgraded.Visits = upgraded.Visits or 0
			return upgraded
		end,
		Fields = { "Visits" },
	},
},
```

A step that adds fields names them in `Fields`. A step that removes or retypes a field says `IsBreaking = true` instead, because a server on the previous release can no longer read what it writes. Every migration says one or the other, never both.

Every field of `State` must be in `Default` or a migration's `Fields`, or the declaration errors.

Put migrations on the entity, never on `Control.Store`, so a release can check them before you deploy.

### What you get

- **A revert.** The console can put any field of `State` back to an earlier save's value, with no code from you. See [Revert data](/docs/players-and-data/revert-data). Do not write a generic "set field" reducer for admins.
- **A timeline.** Every saved version of a key, with the operations that landed around it.

### Follow the rules

- Declare entities in `Definitions` modules. `:Declare` does not take entities yet.
- Names starting `craftsman.` and `_` are reserved.
- Leave out `Authority`. A `Memory` table makes a MemoryStore entity, and anything else is a data entity. `Authority = "postgres"` is accepted, but nothing can read or write it.

Next: [Write reducers](/docs/players-and-data/reducers)

---

## Page: /docs/players-and-data/reducers

**Write reducers.** Declare the only ways an entity's data may change, and choose which ones the console can run.

A **reducer** is a named change to an entity: add coins, level up, grant an item. Every write goes through one.

### Write one

```lua
local Amount = gt.number({ integer = true, range = "[1, 100000]" })

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

- `Profile.Writes(fields)` takes GreenTea types for the reducer's inputs.
- `.Then(fn)` gets the current `state` and the checked `fields`. Return **a table of the fields that changed**, or **`nil` to refuse**.
- Names starting with `_` are reserved.

### Make it private or an operation

A bare `.Writes(...).Then(fn)`, like `RecordVisit`, is **private**. Only your game calls it.

Chain an impact or any builder below, and moderators can run it in the console as an **operation**. An operation must state its impact.

| Impact | Use it when |
|---|---|
| `.NoImpact()` | It changes nothing of value. |
| `.Grants(field)` | It gives value. `field` names the amount, which budgets measure. |
| `.Transfers(field)` | It moves value between players. |
| `.Prices(field?)` | It charges or sets a price. |

| Builder | Meaning |
|---|---|
| `.Label(text)` | The name in the console. |
| `.Named(key)` | The operation's key, by default like `player.profile.add_coins`. |
| `.Permission(key)` | The permission needed. Defaults to the operation's key. |
| `.Tier(tier)` | `"routine"`, `"valued"` or `"structural"`. |
| `.Irreversible()` | It cannot be undone. This makes it `structural`. |
| `.Factor("passkey")` | Ask for a passkey every time. |
| `.Approver(true)` | Ask for a second person to approve. |
| `.Constrain(types)` | Extra constraints, as a table of GreenTea types. |

An operation is `routine` by default, and `valued` when it grants or transfers. See [Read an operation's tier](/docs/concepts/tiers).

### Call it from your game

```lua
local ok, reason = Player.Profile.AddCoins(player, { Amount = 50 })
```

On a player-keyed entity, the target is the Player and the change goes to their session. On a string-keyed entity, the target is the key and the change is written straight through. [Open the store](/docs/players-and-data/open-stores) first.

### Never read a clock

> [!WARNING]
> A reducer must use only `state` and `fields`. Ledger runs it again whenever it judges a change on a newer saved state. So `os.time()`, `tick()` or `math.random()` inside `Then` gives a different answer each time, and nothing stops you.

Pass the time in as a field:

```lua
-- State has LastClaim = gt.number({ integer = true })
Reducers.Claim = Profile.Writes({ At = gt.number({ integer = true }) }).Then(function(state, fields)
	return { LastClaim = fields.At }
end)

Player.Profile.Claim(player, { At = os.time() })
```

Next: [Open stores and write data](/docs/players-and-data/open-stores)

---

## Page: /docs/players-and-data/open-stores

**Open stores and write data.** Open each entity's store in the Stores module, load player sessions, and read and write documents.

Open every entity's store in one ModuleScript: `ServerScriptService.Craftsman.Control.Stores`, which returns `Control`.

### Add a store

`craftsman init` writes the module as `src/Control/Stores.luau`, opening nothing. Add a `Control.Store` call per entity:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Ledger = require("@game/ReplicatedStorage/CraftsmanPackages/Ledger")
local Player = require("@game/ReplicatedStorage/Features/Player/Definitions")

local Profiles = Control.Store({
	Entity = Player.Profile,
	Ledger = Ledger,
})

return Control
```

Your start Script requires this module instead of the package.

> [!WARNING]
> Open stores in this ModuleScript, never in a Script. A store opened from a Script works in game, but the console says the place has no store. [`craftsman doctor`](/docs/cli/quality-commands#doctor) checks the module exists.

To give the console's Account & actions row a list of players, also call `Control.Metadata({ Ledger = Ledger })` here.

| Option | Meaning |
|---|---|
| `Entity` | Required. The entity, after `:Reducers`. |
| `Ledger` | Required for a data entity. The Ledger package. |
| `OnApplied` | `function(key, kind, fields, state, opId)`, called after the console's change lands. |

### Load player sessions

```lua
Players.PlayerAdded:Connect(function(player)
	Profiles.Load(player) -- yields
	if Profiles.Session(player) == nil then
		return -- the load failed
	end
	Player.Profile.RecordVisit(player, {})
end)

Players.PlayerRemoving:Connect(function(player)
	Profiles.Unload(player)
end)
```

### Read and write

| Call | Does |
|---|---|
| `Player.Profile.AddCoins(player, fields)` | Runs a reducer on the player's session. |
| `Profiles.Apply(player, "AddCoins", fields)` | The same, through the store. |
| `Profiles.Edit(key, "AddCoins", fields)` | Runs a reducer on any key, loaded or not. Saved once it returns `true`. |
| `Profiles.Read(key)` | Reads a document. |

Each write returns `ok, reason, detail`. `reason` is Ledger's word, like `Refused` or `Busy`.

> [!WARNING]
> `Unresolved` means the change may have landed. Do not retry a grant without checking, or the player may get it twice.

### See the console's changes

A server holding a player's session sees a console change at its next save or idle read, up to 120 seconds. To read it sooner, refresh the session when you hear a change landed, through `OnApplied` or a MessagingService message of your own:

```lua
local session = Profiles.Session(player)
if session ~= nil then
	session:Refresh()
end
```

### Open the rest here

- The book of passes and products: `Control.Commerce.Open({ Declaration = Definitions, Ledger = Ledger })`. See [the store catalogue](/docs/store-and-purchases/store-catalogue).
- [MemoryStore entities](/docs/players-and-data/memorystore-entities), which need no Ledger.
- [Replication to clients](/docs/players-and-data/replicate-to-clients), right after a store opens.

> [!NOTE]
> The console may keep a session open for minutes, so anything this module caches can reach it stale. Keep the module to opening stores.

---

## Page: /docs/players-and-data/replicate-to-clients

**Replicate entities to clients.** Stream a player's saved data to their client, and chosen fields to other players, with no remotes.

`Control.Replicate` streams an entity's documents to clients, and `Control.Replica` reads them there. You do not write a RemoteEvent.

This works for data entities only.

### Send it from the server

In the Stores module, right after the store opens:

```lua
local Profiles = Control.Store({ Entity = Player.Profile, Ledger = Ledger })

Control.Replicate(Profiles, {
	Level = function(subject: Player, viewer: Player)
		return subject.Team == viewer.Team
	end,
})
```

The second argument has one function per field in the entity's `Shared`, and exactly those. Each decides whether `viewer` may see that field of `subject`'s document. Pass `{}` when the entity shares nothing.

| Who | Receives |
|---|---|
| The player themselves | Their whole document, or only the `Owner` fields if the entity declares `Owner`. Fields starting with `_` are never sent. |
| Other players | Only `Shared` fields, and only where your function returns `true`. |

Pass `{ Owner = false }` as a third argument to stop sending players their own document.

A string-keyed store sends a key only after you call `Share(key)` on the handle `Replicate` returns. `Unshare(key)` stops it.

### Read it on the client

Require the same declaration module, then:

```lua
local Profile = Control.Replica(Player.Profile)

Profile.Observe().Subscribe(function(state)
	CoinsLabel.Text = `Coins: {state.Coins}`
end)

Profile.Select(function(state)
	return state.Coins
end).Subscribe(PlayCoinSound)

Profile.Of(other.UserId).Observe().Subscribe(function(shared)
	Nameplate.Text = `Lv {shared.Level}`
end)
```

| Call | Gives |
|---|---|
| `Get()` | The current document, or `nil` before it arrives. |
| `Observe()` | Every new document. |
| `Select(fn)` | Only changes to what `fn` picks out. |
| `Of(userId)` | Another player's shared fields, with the same calls. |

`Subscribe(fn)` returns a function that stops listening. Values are typed from the entity's `State` and `Shared`.

> [!NOTE]
> A console change reaches the client only once the server's session sees it. See [flushing a session](/docs/players-and-data/open-stores).

---

## Page: /docs/players-and-data/memorystore-entities

**Declare a MemoryStore entity.** Declare a sorted map or a queue, like a matchmaking pool or pending rewards.

A MemoryStore entity holds short-lived data in a Roblox sorted map or queue: a matchmaking pool, a leaderboard, pending rewards. Declare it with a `Memory` table instead of `Store`.

> [!WARNING]
> Reading a queue hides its items from your game for a while. So the console never reads a queue. It shows the queue's name and its operations, and nothing else.

### Declare a sorted map

```lua
local Region = gt.string({ bytes = "[1, 16]" })
local Skill = gt.number({ integer = true, range = "[0, 5000]" })
local QueuedAt = gt.number({ integer = true, range = "[0, 4102444800]" })

local Pool = Control.Scope("Pool"):Entity({
	Label = "Matchmaking pool",
	Memory = { Structure = "SortedMap", Name = "MatchmakingPool" },
	State = gt.build(gt.table({ Region = Region, Skill = Skill, QueuedAt = QueuedAt })),
	Default = { Region = "none", Skill = 0, QueuedAt = 0 },
})

local Enter = Pool.Writes({ Region = Region, Skill = Skill, QueuedAt = QueuedAt }).Then(function(_, fields)
	return { Region = fields.Region, Skill = fields.Skill, QueuedAt = fields.QueuedAt }
end)

local Rate = Pool.Writes({ Skill = Skill }).NoImpact().Label("Adjust matchmaking skill").Then(function(state, fields)
	if state.QueuedAt == 0 then
		return nil
	end
	return { Skill = fields.Skill }
end)

return Pool:Reducers({ Enter = Enter, Rate = Rate })
```

Open it in the Stores module. It needs no Ledger:

```lua
Control.Store({
	Entity = Pool,
	Expiry = 600,
	SortKey = function(state)
		return state.Skill
	end,
})
```

Write with a reducer and the item's key: `Pool.Enter(tostring(player.UserId), fields)`. Read with `store.Read(key)` and delete with `store.Remove(key)`.

### Declare a queue

```lua
local UserId = gt.number({ integer = true, range = "[1, 1e15]" })
local Reward = gt.string({ bytes = "[1, 64]" })
local Coins = gt.number({ integer = true, range = "[0, 1000000]" })

local Pending = Control.Scope("Pending"):Entity({
	Label = "Pending rewards",
	Memory = { Structure = "Queue", Name = "PendingRewards" },
	State = gt.build(gt.table({ UserId = UserId, Reward = Reward, Coins = Coins })),
	Default = { UserId = 1, Reward = "none", Coins = 0 },
})

local Grant = Pending.Writes({ UserId = UserId, Reward = Reward, Coins = Coins })
	.Grants("Coins")
	.Irreversible()
	.Label("Queue a reward")
	.Then(function(_, fields)
		return { UserId = fields.UserId, Reward = fields.Reward, Coins = fields.Coins }
	end)
```

A queue reducer takes no key. An operation on a queue must be `.Irreversible()`, because an added item has no key to take it back by.

Your game calls `store.Take(count, wait?)`, which returns the items and a read id. Call `store.Discard(readId)` once they are handled.

### Set store options

| Option | Meaning |
|---|---|
| `Entity` | Required. |
| `Expiry` | Required. Whole seconds an item lives after each write. |
| `SortKey` | Sorted map only. `function(state)` returning the item's sort key. |
| `Priority` | Queue only. `function(item)` returning its priority. |
| `Invisibility` | Queue only. Seconds a `Take` hides its items for. |

### See it in the console

A sorted map lists its items under **Records**, each with its sort key and expiry. Items expire on their own, so an empty map between matches is normal.

An item that is missing may have expired, or never been written. Its operations still run against that key. The game starts a missing item from `Default`, and your rules decide if that is allowed.

A queue row shows **Changes only through the operations below.** Each operation builds an item with your game's code and adds it.

### Know the limits

- **Only sorted maps and queues.** Open Cloud cannot read hash maps, so they are refused.
- **Writes go only through reducers.** The console shows items read-only.
- **No history.** There is no timeline, revert or `Migrations`.
- **A sorted-map key is at most 128 bytes.**
- **No replication** to clients.
- Structure names starting `Craftsman` are reserved.

A reducer with no impact or builder is private, and the console shows no operation for it. An entity with no `Default` has no operations at all, and is read-only in the console.

---

## Page: /docs/players-and-data/find-a-player

**Find a player.** Look up a player by UserId, profile link or username, then open their account and data.

Look someone up, then see who they are and what they hold.

### Look them up

1. Open **Players & Data**, and pick the place group in the top bar.
2. Under **Player**, pick **Account & actions**.
3. Type a UserId, a profile link such as `roblox.com/users/123/profile`, or a username.
4. Press **Open**.

Or press **Inspect** on a UserId in the **Players** list. **Filter prefix** and **List** narrow it down.

The list fills as players join, once your Stores module calls `Control.Metadata`. See [Open stores](/docs/players-and-data/open-stores). With no list, you will see **No roster until the game publishes again. A UserId still opens.**

### Read the account

| Line | Comes from |
|---|---|
| Name, **Account age**, **Created**, **User ID**, **Locale** | Roblox. An account under 30 days old is marked. |
| **ID verified**, **Premium** | Roblox. |
| **First seen**, **Last seen** | Your game, written once per join. "Last seen" means last arrived. |
| **Banned here** | Roblox's ban list. Shown only while a ban is in force. |

Each part answers on its own. A key missing a scope shows a sentence naming it, beside whatever did load. See [Missing key scope](/docs/troubleshooting/missing-key-scope).

**Find server** opens the Servers page on the server they are in.

### Open their data

Under **Player**, pick one of your player-keyed entities. The UserId stays open as you move between rows.

The document is read from the game when you open it, so it takes a few seconds. Nothing is copied into Project Control. **Refresh** reads it again.

### Open any other record

Entities not keyed by player sit below the **Player** group. Pick one, then type the exact key or press **Inspect** in **Records**.

If an entity is missing, it has not been published to this place group. Deploy a release there, or run:

```bash
craftsman publish --group <group> --activate --apply
```

Next: [Run an operation](/docs/players-and-data/run-an-operation)

---

## Page: /docs/players-and-data/run-an-operation

**Run an operation.** Run one of your game's operations against a document from the console.

Run a change your game declared, like adding coins, against one document.

### Run it

1. In **Players & Data**, [open the document](/docs/players-and-data/find-a-player).
2. In **Actions & operations**, press the operation.
3. Fill in its fields. A value out of range is flagged under the field.
4. Press **Run in** and the group name, or **Request in** for a `structural` operation.

Your game's own rules check every change. Each row shows its tier, and **passkey** if it asks for one. See [Read an operation's tier](/docs/concepts/tiers).

Operations come from your [reducers](/docs/players-and-data/reducers). Your game's [commands](/docs/servers-and-commands/commands) are on **Account & actions** instead, since they do not change a document. On a queue, there is no target: each operation adds an item.

A greyed-out row says why it cannot run.

### Read the result

| You see | It means |
|---|---|
| **Saved to the document.** | It landed. The new document is shown below. |
| **The game refused it:** and a reason | Your game's rules said no. Nothing changed. |
| **It could not be completed:** and a reason | Nobody knows if it applied. |
| A note about the delay | It is waiting. See below. |

See [Read an operation's status](/docs/concepts/refused-or-failed).

### Retry a failed one

Check the document's timeline first.

Pressing run again with the same values returns the same operation, and sends nothing. Only **Send as a new operation** sends it again. If the first one did apply, a new one applies twice.

### Wait for a structural one

A `structural` operation asks for your passkey and waits 15 minutes. If it asks for an approver, a second person must also approve it, unless nobody else on the project can. You cannot approve your own.

Approving early does not shorten the wait. Until it runs, it can be cancelled from **Approvals**. See [Approve a request](/docs/team-and-access/approve-a-request).

### See no operations

**No operations change** and the entity's key means your game declares none. Add an impact or builder to a reducer, then publish. A bare reducer stays private to your game.

---

## Page: /docs/players-and-data/edit-a-field

**Edit a field.** Change a saved field from the console, with a diff, a passkey and a second approver if there is one.

The console has no free-form box for typing a value. You change a field in one of two ways:

- **To a new value:** [run an operation](/docs/players-and-data/run-an-operation) your game declares.
- **Back to an earlier value:** pick a save that holds it, and request an edit.

### Request an edit

1. In **Players & Data**, [open the document](/docs/players-and-data/find-a-player).
2. Open the **Current (Live)** menu, and pick a save under **SAVES**.
3. Tick only the fields you want. Press **Deselect** first to clear them all.
4. Press **Revert selected**.
5. Read the panel, then press **Request this edit**.
6. Tap your passkey. A tap in the last ten minutes covers it. See [Tap a passkey once](/docs/concepts/passkey-window).

The panel shows two things:

- **Expect:** what each field must still hold when this runs.
- **Fields:** what this writes.

It also lists every field it is **not** changing.

### When it lands

An edit is `structural`, and has no delay. If someone else on the project can approve, it runs when they do, and you cannot approve your own. They see each field's before and after in **Approvals**. If nobody else can, it runs after your passkey.

The request is recorded in the audit log.

### Know when it is refused

Each field is pinned to its live value when you press **Revert selected**. If a field you are writing changed by the time the edit runs, the game refuses the whole edit. Nothing is written.

So a player earning coins before it runs refuses an edit to `Coins`. Request it again from the new value.

### Who can do it

You need `craftsman.data.edit`. It covers every data entity in the project.

You cannot edit a MemoryStore entity this way, or an entity that declares no fields.

Next: [Revert data](/docs/players-and-data/revert-data)

---

## Page: /docs/players-and-data/revert-data

**Revert data.** Undo one save, or restore a document to an earlier point, field by field.

Put a document back after a bad change. Pick a save, choose what it means, then request the edit.

### Pick a save

Open the document, then open the **Current (Live)** menu. Under **SAVES**:

| Badge | It means |
|---|---|
| **still open** | Saved this hour. A later write this hour replaces it. |
| **2 ops** | Operations from the console landed around this save. |
| **in-game save** | No console operation sits under it, so your game wrote it. |
| **+1 pending** | An operation is newer than the newest save. |

Roblox keeps **one save per key per hour**. Every write in that hour replaces it, so one save can cover many changes.

Roblox keeps saves for 30 days. Older operations show under **OLDER THAN 30 DAYS**, and cannot be compared.

If a still-open save gets replaced, reading it is refused. Pick **Current (Live)** in the menu and press **Refresh** to list what replaced it.

### Choose restore or undo

- **Restore to this point** offers every field that differs from now. This is the default.
- **Undo this save** offers only the fields that save changed, and writes back the save before it. It is off for the oldest save.

The diff marks the live value with `-` and the value you'd get with `+`. **Covered ops** names the operations around that save, with who ran them.

### Request it

1. Tick the fields you want. **Select all** and **Deselect** help.
2. Press **Revert selected** or **Undo selected**.
3. Read the panel, then press **Request this edit**.

Nothing changes until it lands. See [Edit a field](/docs/players-and-data/edit-a-field) for the passkey, the wait and the approval.

### Read the warnings

| Warning | What to do |
|---|---|
| **Later changes touched** these fields. **Reverting discards them.** | Check you mean to lose those changes. |
| These fields **moved while you were choosing.** | The edit is pinned to the newer values shown. |
| **Every chosen field already holds this value.** | Nothing's left to change. |
| **field created after this save, cannot be removed** | The field did not exist then, so it cannot be put back. |

Press **Switch to Live View** to leave without changing anything.

Next: [Investigate a change](/docs/players-and-data/investigate-a-change)

---

## Page: /docs/players-and-data/investigate-a-change

**Investigate a change.** See what changed in a document around a time.

Find out what happened to a document around a given time.

### Compare the saves

1. In **Players & Data**, [open the document](/docs/players-and-data/find-a-player).
2. Open **Investigate**, under the document.
3. Pick the time next to **Changes around**.
4. Press **Compare**.

This makes two requests into your game, out of the five a minute Roblox allows an experience. Nothing's read until you press the button.

### See the whole history

Press **Whole history** instead. It shows saves spread evenly across the life of the document.

**Pin** keeps a save in your next comparison. You can pin 3.

### Read the table

Each column is a save, named by its time: **Before** your time, **After** it, and **Now**, the live document. A blue line marks where your time falls.

Each row is a field that changed between them, numbers and anything else. A changed value is bold, with its change and its rate per hour under it. Press **Show N unchanged** to see the rest.

Roblox keeps one save an hour, so the table never shows the state at an exact minute. A rate is an average over a gap nobody can see inside. It can show something could not have been earned that fast. It cannot show how fast it was.

The console gives no verdict. You decide what the numbers mean.

### Check what you changed

**Also in this window** lists your own deploys, config edits and halts between the first and last column, and any operation run on this document. It reads the 100 newest audit entries, and says so if they do not reach back far enough.

### Know the limits

- Up to 1,024 saves are listed, which is more than Roblox keeps in 30 days.
- An old save is shown exactly as it was saved, so its shape can differ across a migration.
- MemoryStore entities keep no saves, so they have no **Investigate** card.

To hand this to an AI assistant, see [Copy for LLM](/docs/players-and-data/copy-for-llm).

Next: [Export the evidence](/docs/players-and-data/export-forensics)

---

## Page: /docs/players-and-data/export-forensics

**Export the evidence.** Download an investigation as one JSON file, recorded in the audit log with your name on it.

Take an investigation out of the console as one file.

### Export it

1. [Compare the saves](/docs/players-and-data/investigate-a-change) in **Investigate**.
2. Press **Export as JSON**.

The file is named `forensics-<entity>-<key>-<moment>Z.json`, and goes only to your browser.

### Know it is recorded

The export is recorded in the audit log first, as `forensics.exported`. If that fails, you will get no file:

**Nothing was exported, because the export could not be recorded in the audit**

### See what is in it

| Field | Holds |
|---|---|
| `moment` | The moment you looked around. |
| `samples` | Each column: its save, time and whole document. |
| `series` | Every declared number across them, with change and rate. |
| `versions` | The saves that were listed. |
| `operations` | What was run against this key through Project Control. |
| `events` | Your deploys, config edits and halts in the window. |
| `caveats` | What the numbers cannot tell you. Keep these with the file. |

A save with no operation beside it was written by your game.

The file holds player data. Store it like any other player data.

---

## Page: /docs/players-and-data/copy-for-llm

**Copy for LLM.** Hand the evidence on a document to an AI assistant, with players hidden.

**Copy for LLM** builds a brief you can paste into an AI assistant. There is no connection to any assistant. You copy the text, and you paste it.

### Copy it

1. Open **Investigate**, under the document.
2. Press **Copy for LLM**.
3. The console takes a capture, then compares the whole history if you have not compared yet.
4. Tap your passkey. The audit records it.
5. Read the text, then press **Copy**.

It needs `craftsman.data.share`. An administrator can turn it off in [General settings](/docs/project-settings/general-settings).

Use a private chat, with an assistant that cannot reach your computer.

### What is hidden

UserIds and names are hidden before the text is built. It goes by field names and long runs of digits, and hides too much on purpose.

### Answers

| Answer | Means | Do |
| --- | --- | --- |
| **could not be looked up** | The name of the player could not be read, so nothing was copied. | Try again. |
| **No capture could be taken** | What the save was in the middle of is not known. | Read the brief with that in mind. |
| **Unchanged fields were left out** | The text was too long. | Nothing. |
| **Your browser would not copy it** | The browser refused. | Select the text, and copy it. |

A capture needs a place built on Control 0.11.9 or later. An older place says so.

Next: [Investigate a change](/docs/players-and-data/investigate-a-change)

---

# Collection: Moderation

Ban, kick and look up players.

---

## Page: /docs/moderation/ban-a-player

**Ban or unban a player.** Ban a player from your game through Roblox, or lift a ban, from the console.

Ban a player through Roblox. If they are in game, they are removed. It also works when they are offline, and needs no game code.

### Ban them

1. In **Players & Data**, [open the player](/docs/players-and-data/find-a-player) on **Account & actions**.
2. In **Actions & operations**, press **Ban player**.
3. Fill in the form.
4. Press **Run in** and the group name, then tap your passkey. A tap in the last ten minutes covers it.

| Field | What to enter |
|---|---|
| **Reason shown to the player** | Required. Up to 400 bytes. |
| **Moderator note** | Never shown to the player. Up to 400 bytes. |
| **Duration in seconds** | Up to 10 years. Leave it empty for permanent. |
| **Leave this person's alt accounts unrestricted** | **Yes** means the ban does not reach their alts. |

It runs at once, with no wait and no second approver.

The ban covers the whole universe this place group lives in.

### Check it

The account card shows **Banned here**, with its length, time left and reasons. It shows bans placed outside Project Control too.

**Roblox has not lifted it yet** means the end time has passed, and Roblox has not caught up.

### Lift it

1. Open the player the same way.
2. Press **Lift ban**.
3. Add why, if you like. It is never shown to the player.
4. Press **Run in** and the group name, then tap your passkey.

The **Banned here** panel goes away. The lift stays in [the player's history](/docs/moderation/player-history).

**Inherited from a parent universe. Lift it there.** means this universe cannot lift it.

### Get access

- Banning and lifting share one permission: `craftsman.player.restrict`.
- Your Open Cloud key needs `universe.user-restriction:write` to ban, and `universe.user-restriction:read` to show a ban.

See [Grant a permission](/docs/team-and-access/grant-a-permission) and [Missing key scope](/docs/troubleshooting/missing-key-scope).

### See no ban option

**Ban and Lift ban arrive with the game's next publish.** Publish to this place group once, and they appear.

### Ban from inside the game

Moderators in your game use `craftsman.player.ban_in_game`, a separate permission with no passkey. There is no in-game lift. See [Run commands in game](/docs/servers-and-commands/run-in-game).

Next: [Kick a player](/docs/moderation/kick-a-player)

---

## Page: /docs/moderation/kick-a-player

**Kick a player.** Declare a kick command in your game, then run it on a live player from the console.

Project Control has no built-in kick for one player. Your game declares it as a command, because only a running server can remove someone.

### Declare it

```lua
local Reason = gt.string({ bytes = "[1, 200]" })

Kick.Now = Control.Command({ Reason = Reason })
	.Label("Kick Player")
	.NoImpact()
	.Tier("routine")
	.Handles(function(directive)
		local player = game:GetService("Players"):GetPlayerByUserId(tonumber(directive.target) or 0)
		if player == nil then
			return -- not on this server, so stay quiet
		end
		player:Kick(directive.params.Reason)
		return true
	end)
```

- Every server gets the command, so return nothing when the player is not there. A `false` reports a refusal, and the first report settles the result.
- `.Tier("routine")` lets it run at once. Without it, a kick waits 15 minutes.

Publish it. See [Declare a command](/docs/servers-and-commands/commands).

### Run it

1. In **Players & Data**, [open the player](/docs/players-and-data/find-a-player) on **Account & actions**.
2. In **Actions & operations**, press your kick.
3. Enter a reason.
4. Press **Run in** and the group name.

### Read the result

You will see **Waiting for a server to acknowledge it**, then **Applied.** It usually takes about two seconds.

If no server answers in two minutes, it shows **No server acknowledged this within 120 seconds.** Either the player was not in the game, or a server kicked them and could not tell us. Running it again is safe.

**Find server** on the account card shows which server they are on.

### Kick them when they next join

A kick lasts for this session only. To catch someone offline, declare a second command with `.Durable()`. See [Reach an offline player](/docs/servers-and-commands/reach-an-offline-player).

To keep them out, [ban them](/docs/moderation/ban-a-player).

### Kick from elsewhere

- In game, from the command bar: [Use the Konsole](/docs/servers-and-commands/use-the-konsole).
- Everyone on one server: [Shut down a server](/docs/servers-and-commands/shut-down-a-server).

Next: [See a player's history](/docs/moderation/player-history)

---

## Page: /docs/moderation/player-history

**See a player's history.** List what has been run against one player, with who ran it and how it ended.

See what has been done to a player before you act.

### Open it

1. In **Players & Data**, [open the player](/docs/players-and-data/find-a-player) on **Account & actions**.
2. Press **History**, near the bottom.

It shows up to the last 50 operations against this UserId. Press **Refresh** after running something new.

**Nothing run against this account.** means nobody has run anything against them through Project Control.

### Read a row

Each row shows the operation's key, its status, when, who ran it and who approved it. A refusal also shows the game's own words.

| Status | It means |
|---|---|
| `applied` | It landed. |
| `refused` | The game or Roblox said no. Nothing changed. |
| `failed` | Nobody knows if it applied. See [Read an operation's status](/docs/concepts/refused-or-failed). |
| `delayed` | It is waiting out its delay, or an approver. |
| `pending` | It was sent, and no answer has come back yet. |
| `waiting` | It is waiting for the player to join. |
| `cancelled` | Someone stopped it before it ran. |

Refused attempts stay on the list.

### Know what is left out

- Bans placed outside Project Control. A ban in force still shows on the account card.
- Anything your game does on its own.

For everything across a project, not just one player, see [Search the audit log](/docs/monitoring/search-the-audit-log).

---

# Collection: Servers and commands

See your live servers and act on them.

---

## Page: /docs/servers-and-commands/see-live-servers

**See your live servers.** Turn on the server registry and list every running server with its players and uptime.

Turn on the registry, then open **Servers**.

### Turn on the registry

Pass `Registry = true` when you start Control on the server:

```lua
local Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
	Registry = true,
})
```

Your Open Cloud key needs `memory-store.sorted-map:read` and `memory-store.sorted-map:write`.

A server writes to the registry every 45 seconds and drops off within two minutes of ending. A Studio session never appears.

### Read the page

1. Open **Servers**.
2. Check the three figures: **Live servers**, **Connected players** and **Behind deploy**.
3. Find a server in the list. Servers are grouped by place group, then place.

**Behind deploy** counts servers on an older version than your last deploy. They get the new version when they restart.

A count with `+` is a minimum, because the read stopped short.

The list does not refresh by itself. Press **Refresh** to read it again.

### Narrow the list

- Filter by JobId or place id.
- Filter by type: public, reserved or private.
- Filter by health: **Behind deploy**, **Stale** or **Reported a warning**.

### Read a row

| Badge | Means |
|---|---|
| `stale` | The server has not written for over 90 seconds. |
| `behind` | It runs an older version than your last deploy. |
| `warning` | It reported a problem. Select it to read what. |
| `overridden` | It runs at least one config or feature override. |

A section named `Unmapped` is a place in no place group. You cannot configure or command those servers until you [add the place to a group](/docs/project-settings/place-groups).

### Find a player's server

On **Players & Data**, open the player and press **Find server**. The page selects their server, or says they are on no listed server. They may be offline, or on a server whose build does not report its players.

Nothing listed: see [the server list is empty](/docs/troubleshooting/empty-server-list).

Next: [Inspect a server](/docs/servers-and-commands/inspect-a-server)

---

## Page: /docs/servers-and-commands/inspect-a-server

**Inspect a server.** Select a running server to read its health, version and players, then act on it.

Select a server on **Servers**. The panel beside the list shows what it last reported.

### Read its numbers

| Tile | Shows |
|---|---|
| **Physics FPS** | Its physics frame rate. |
| **Memory usage** | Its memory in MB. |
| **Connected players** | Players, against the cap. |
| **Total uptime** | How long it has been running. |
| **Commit** | The commit of its release. |
| **Framework version** | The Craftsman Control version it runs. |
| **Overrides** | How many configs or features are overridden on it. |

**Not stamped** under **Commit** means the place was published from Studio, or built without a commit.

### Know how fresh it is

The selected server refreshes every 30 seconds. The line above the tiles says what is happening:

- **Paused while the page was idle:** the tab was hidden, or nobody touched it for five minutes. Press **Resume**, or come back to the page.
- **This server has ended:** press **Refresh the list**.
- **Refreshing stopped:** a read failed. Press **Resume** to try again.

### Check its warnings

A warning box appears when a server reports one of these:

- **Running a manifest this console did not publish**
- **Fell back to in-code fallbacks**

For the second, see [a server fell back to defaults](/docs/troubleshooting/fell-back-to-defaults).

### See who is on it

**Players on this server** lists each player as last reported.

- **Account** opens the player on **Players & Data**.
- **Profile** opens their Roblox profile.

The list is capped, so a full server can say **Showing 200 of 250**. If it says the server's build does not report its players, ship a newer framework version.

### Act on it

- **Configure this server** opens **Configuration** for this server only. See [Override one server](/docs/features/override-one-server).
- **Commands for this server** opens **Commands** for this server only. See [Send commands to servers](/docs/servers-and-commands/commands-page).

If the panel says **Its place is in no place group**, [add the place to a group](/docs/project-settings/place-groups) first.

---

## Page: /docs/servers-and-commands/shut-down-a-server

**Shut down a server.** End one running server by moving its players elsewhere, or kick everyone on it.

You can move a server's players away and end it, or kick everyone at once. Both act on one server, never on every server.

### Shut it down

1. On **Servers**, select the server.
2. Press **Commands for this server**.
3. Open **Shut down this server**.
4. Fill in **What to tell players who cannot be moved**, up to 200 bytes.
5. Leave **Move players to another server first** empty or on **Yes**.
6. Press **Run in** and your group's name.
7. Read the count of players, then press **Shut down this server**.

Players move to another server of the same place. Anyone still there after 20 seconds is kicked with your message. If no other server can take them, everyone is kicked at once.

### Kick everyone

1. Open the server's commands, as above.
2. Open **Kick everyone on this server**.
3. Fill in **What to tell the players**.
4. Press **Run in** and your group's name, then confirm.

Setting **Move players to another server first** to **No** on a shutdown also just kicks everyone.

### What you need

Both need the `craftsman.server.end` permission. It is `valued`, with no passkey and no delay. See [Grant a permission](/docs/team-and-access/grant-a-permission).

The server must be started with `Registry = true`. Both commands arrive with your game's next publish.

You cannot undo either one.

---

## Page: /docs/servers-and-commands/message-a-server

**Message a server.** Show a message to everyone on one server, after your game draws it with Control.OnServerMessage.

Draw the message in your game once, then send it from the console.

### Draw the message

Project Control hands your game the text. You decide how it looks.

```lua
Control.OnServerMessage(function(text)
	Banner.Show(text)
end)
```

Without a handler, the server refuses the message and says why.

> [!NOTE]
> The text does not pass through Roblox's text filter.

### Send it

1. On **Servers**, select the server.
2. Press **Commands for this server**.
3. Open **Message this server**.
4. Type the text, up to 200 bytes.
5. Press **Run in** and your group's name.

It needs the `craftsman.server.message` permission. It is `routine`, so there is no passkey or delay.

The server must be started with `Registry = true`. The command arrives with your game's next publish.

### Send it everywhere

The same command goes to every server at once. See [Command every server](/docs/servers-and-commands/command-every-server).

---

## Page: /docs/servers-and-commands/commands-page

**Send commands to servers.** Use the Commands page to run a command on every server in a place group, or on one you pick.

**Commands** runs a command on running servers. With no server picked, it reaches every server in the current place group.

### Run a command

1. Open **Commands**. The place group in the top bar is where it goes.
2. To reach one server, press **Pick a server**, select it, then press **Commands for this server**.
3. Open a command and fill in its fields.
4. Press **Run in** and your group's name.

A tier decides what happens next. See [Tiers](/docs/concepts/tiers). A `structural` command's button says **Request** instead of **Run**.

The result stays on screen until the server answers. See [Refused or failed](/docs/concepts/refused-or-failed).

### Read the server banner

With a server picked, a banner names it and says **This server only**.

- **View server** goes back to it on **Servers**.
- **Remove** acts on the whole group again.
- **Ended** means nothing more can be sent. Remove it to act on the group.
- **Could not read it** means its entry could not be read. Hover for the reason. Commands can still be sent.

Configuration shows the same banner. See [Override one server](/docs/features/override-one-server).

### Know what is listed

| List | Shows |
|---|---|
| Every server | Commands declared `.PerServer()`, and **Message this server**. |
| One server | Those, plus `.SingleServer()` commands, **Shut down this server** and **Kick everyone on this server**. |

A greyed row says why it cannot run.

### Fix an empty list

**No commands for every server, or for this server:** your game declares no `.PerServer()` command yet. Craftsman's own arrive with your next publish.

**Nothing declared in a group:** a group's commands come from its active manifest. Deploy a release to the group, or activate from the terminal:

```bash
craftsman publish --group Production --activate --no-build --apply
```

### Find other actions

An action sits beside what it acts on.

- A command aimed at a player is on **Players & Data**, beside the person.
- A change to a document is beside that document.
- Configs and overrides are on **Configuration**.

Next: [Command every server](/docs/servers-and-commands/command-every-server)

---

## Page: /docs/servers-and-commands/command-every-server

**Command every server.** Run one command on every server in a place group at once, and read how many ran it.

Run a server command across a whole place group.

### Send it

1. Pick the place group in the top bar.
2. Open **Commands**. Make sure no server is picked.
3. Check the line under the title. It shows how many servers and players were listed when the page opened.
4. Open a command and fill in its fields.
5. Press **Run in** and your group's name.
6. Read the count, then type the group's name to confirm.
7. Press the button, such as **Message 12 servers**.

The count is not refreshed by itself. Reload the page for a new one.

If nobody's running, the page says **No servers are running in** your group, and nothing can be sent.

### Read the result

Each server answers as it runs the command. The result counts them for two minutes, such as **Ran on 11 of 12 servers. 1 refused it.**

- It is applied once any server runs it.
- It is refused only if every server that answered refused.

### Get permission

You need `craftsman.server.every` as well as the command's own permission. It adds the reach, and the command's tier does not change.

See [Grant a permission](/docs/team-and-access/grant-a-permission).

### Choose what can go everywhere

Every `.PerServer()` command can go to every server, and so can **Message this server**. To keep a command to one server, declare it `.SingleServer()`. See [Choose how a command is delivered](/docs/servers-and-commands/command-delivery).

**Shut down this server** and **Kick everyone on this server** never go everywhere.

Only servers started with `Registry = true` run these commands.

---

## Page: /docs/servers-and-commands/commands

**Declare a command.** Declare an action a moderator can run on a live player or server, like a kick or a message.

A command is an action on a running server: a kick, a teleport, a message. The console sends it, and your handler does it.

### Declare it

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local gt = Control.GreenTea

local Kick = {}

Kick.Now = Control.Command({ Reason = gt.string({ bytes = "[1, 200]" }) })
	.Label("Kick player")
	.NoImpact()
	.Tier("routine")
	.Handles(function(directive)
		local player = game:GetService("Players"):GetPlayerByUserId(tonumber(directive.target) or 0)
		if player == nil then
			return nil
		end
		player:Kick(directive.params.Reason)
		return true
	end)

return Control.Scope("Kick"):Commands(Kick)
```

Under `Control.Scope("Player"):Scopes({ Kick = Kick })`, its key is `player.kick.now`. You can also [declare it where it is used](/docs/features/declare-where-used).

Params are GreenTea types. A value that does not match never reaches your handler. Label one with `gt.meta(type, { Label = "Seconds" })`, and build one per command: `gt.meta` changes the type you hand it.

### State its impact

Pick one, or the declaration errors.

- `.NoImpact()`: hands out nothing of value.
- `.Grants(param)`: gives something. `param` names the amount.
- `.Transfers(param)`: moves value between players.
- `.Prices(param?)`: charges or sets a price.

### Say who may run it

| Builder | Does |
|---|---|
| `.Label(text)` | Names it in the console. |
| `.Permission(key)` | Sets the permission. Defaults to the command's key. |
| `.Tier(tier)` | `"routine"`, `"valued"` or `"structural"`. |
| `.Factor("passkey")` | Asks for a passkey every time. |
| `.Approver(true)` | Asks a second person to approve. |
| `.Constrain(types)` | Adds constraints, as GreenTea types. |

The default tier is `structural`: a passkey, a 15-minute wait, and never from inside the game. Use `.Tier("routine")` for anything a moderator should do quickly. See [Tiers](/docs/concepts/tiers).

### Write the handler

`.Handles(fn)` ends the chain. `fn` gets `directive.params`, `directive.id` and `directive.target`, a string: a UserId for a player command.

Return one of these:

- `true`, with an optional sentence: it ran.
- `false`, with a sentence: it refused, and the console shows why.
- `nil`: this server has nothing to say.

Every server in the group receives a player command. Return `nil` when the player is not here, or the wrong server settles it for everyone.

### Register it in time

Start in this order: `Control.Reporter.Start`, every handler, then `Control.Start`. A command with no handler is skipped, and after two minutes the console reports it failed.

To register a handler elsewhere, leave out `.Handles` and call `Control.OnCommand("player.kick.now", fn)`.

Keys starting `craftsman.` and names starting `_` are reserved.

Next: [Choose how a command is delivered](/docs/servers-and-commands/command-delivery)

---

## Page: /docs/servers-and-commands/command-delivery

**Choose how a command is delivered.** Decide whether a command runs now, waits for a player, or targets a server.

Add one builder to the command.

| You want | Chain |
|---|---|
| Act on a player who is playing now | nothing, live is the default |
| Act on a player whenever they next join | `.Durable()` |
| Act on one server, or every server | `.PerServer()` |
| Act on one server, never every server | `.SingleServer()` |

### Live

A live command runs in the player's current session or not at all. It is sent once, with no retry.

If no server answers within two minutes, the console reports **failed**. Nobody can tell whether it ran.

The whole command travels in one message of up to 900 bytes, so keep params small. A larger one is refused before it is sent.

### Durable

A durable command waits, with no expiry, until a server that has the player runs it. See [Reach an offline player](/docs/servers-and-commands/reach-an-offline-player).

### Per server

```lua
ServerCommands.Countdown = Control.Command({
	Seconds = gt.number({ integer = true, range = "[3, 60]" }),
})
	.Label("Start a countdown on this server")
	.NoImpact()
	.Tier("routine")
	.PerServer()
	.Handles(function(directive)
		StartCountdown(directive.params.Seconds)
		return true, "counting down"
	end)
```

A per-server command targets a server instead of a player. Run it from [Commands](/docs/servers-and-commands/commands-page). It reaches only a server started with `Registry = true`.

It cannot be durable, because it acts on a server that is running now.

### Single server

Use `.SingleServer()` instead of `.PerServer()` for anything harmful everywhere at once. It is still sent to one server, but it cannot [go to every server](/docs/servers-and-commands/command-every-server).

### Provided server commands

With `Registry = true`, every server also runs Project Control's own commands:

- [Message this server](/docs/servers-and-commands/message-a-server)
- [Shut down this server](/docs/servers-and-commands/shut-down-a-server)
- [Kick everyone on this server](/docs/servers-and-commands/shut-down-a-server)

Next: [Reach an offline player](/docs/servers-and-commands/reach-an-offline-player)

---

## Page: /docs/servers-and-commands/reach-an-offline-player

**Reach an offline player.** Declare a durable command that waits for a player and runs when they next join.

Declare the command with `.Durable()`, collect it on join, then send it from the console.

### Declare it

```lua
Kick.NextJoin = Control.Command({ Reason = Reason })
	.Label("Kick on next join")
	.NoImpact()
	.Tier("routine")
	.Durable()
	.Handles(function(directive)
		local player = Players:GetPlayerByUserId(tonumber(directive.target) or 0)
		if player == nil then
			return nil
		end
		player:Kick(directive.params.Reason)
		return true
	end)
```

Return `nil` when the player is not on this server. Every running server receives the command, and a `true` or `false` from any of them settles it, so it would never wait for the player.

### Collect it on join

Pass `FetchCommands` to `Control.Start`. Without it, a joining player's commands are never collected.

```lua
local Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
})
```

`Control.Reporter.Start` must run first.

### Send it

1. Open **Players & Data** and [find the player](/docs/players-and-data/find-a-player).
2. Open the command on their account.
3. Fill in its fields.
4. Press **Run in** and your group's name.

If they are playing now, a server runs it right away. If not, the console shows **Sent. If they are not playing now, it runs when they next join.**

### Follow it

The command stays **waiting** until a server runs it. That can take days, and it does not mean anything went wrong.

Only your handler's answer settles it. A server that collects it and then closes leaves it for the next join.

See every command sent to a player in [their history](/docs/moderation/player-history).

Durable delivery only works on player commands. A per-server command cannot be durable.

---

## Page: /docs/servers-and-commands/run-in-game

**Run commands in game.** Run a command for an attached admin from game code, check what they may do, and ban in game.

Admins whose Roblox account is [attached to your project](/docs/team-and-access/attach-an-account) can run commands in game. Their roles live in the console, so you keep no separate admin list.

`Control.Run` needs `Control.Reporter.Start`, and `Control.Start` with `Access` left on.

### Run a command

```lua
local result = Control.Run(admin, "player.kick.now", tostring(target.UserId), { Reason = "AFK" })

if result.Outcome ~= "applied" then
	warn(result.Reason)
end
```

`Control.Run(who, key, target?, params?)` takes a Player or a UserId.

| `Outcome` | Means |
|---|---|
| `applied` | It ran. |
| `refused` | Your handler returned `false` or nothing, or Roblox or the reducer refused. |
| `denied` | It was not allowed. `result.Code` is a word to branch on, such as `not_attached`, `not_held`, `structural` or `passkey`. |
| `failed` | Nobody can tell whether it ran. The handler may have errored, or be missing. |

- Your own commands run on this server, with no request and no yield.
- A reducer, a data edit or a ban goes to Project Control, and `Run` yields.
- A `structural` command never runs in game. Lower it with `.Tier("routine")` or `.Tier("valued")`.
- A permission asking for a passkey, or a grant limited by a budget, keeps a command console-only.
- Every in-game run reaches the audit log, within a minute.

### Check access

```lua
local ok, sentence = Control.Access.CanRun(player.UserId, "player.kick.now")
KickButton.Visible = ok
```

| `Control.Access` | Answers |
|---|---|
| `CanRun(userId, key)` | Whether they may run this here, now. Returns `ok`, a sentence and a reason word. |
| `HasPermission(userId, key)` | Whether they hold the permission at all. |
| `GetPermissions(userId)` | Every permission they hold here. |
| `GetMember(userId)` | Their roles, or `nil` when not attached. |
| `IsReady()` | Whether the list has been read. |
| `OnChanged(fn)` | Fires when the list changes. |

> [!WARNING]
> Do not show a button from `HasPermission`. It is true even when a passkey, a budget or a halt would stop the command. Use `CanRun`.

Until the list has been read, nobody holds anything. In Studio, it needs API access and an attached account.

### Ban in game

```lua
Control.Run(admin, "craftsman.player.ban_in_game", tostring(userId), {
	display_reason = "Exploiting", -- shown to the player
	private_reason = "Speed hack in match 7", -- optional, moderators only
	duration_seconds = 86400, -- leave out for a permanent ban
	exclude_alt_accounts = false,
})
```

It needs `craftsman.player.ban_in_game`. A grant on `craftsman.player.ban` does not cover it. [Lift a ban from the console](/docs/moderation/ban-a-player). There is no in-game unban.

Next: [Use the Konsole](/docs/servers-and-commands/use-the-konsole)

---

## Page: /docs/servers-and-commands/use-the-konsole

**Use the Konsole.** Add a typed command bar so attached admins can run your commands from inside the game.

Add the Konsole bar, and admins can type your commands in game. Each line runs through `Control.Run`, so access, tiers and the audit all apply.

### Attach it

Konsole comes with Craftsman's packages, so there is nothing to install. Do not add it to `wally.toml`.

On the server, after `Control.Reporter.Start`:

```lua
local Konsole = require("@game/ReplicatedStorage/CraftsmanPackages/Konsole")

Control.Konsole.Attach(Konsole, {
	Declaration = Definitions,
	Kick = "player.kick.now",
})
```

In a LocalScript. It yields until the server attaches, so spawn it if code follows:

```lua
local Konsole = require("@game/ReplicatedStorage/CraftsmanPackages/Konsole")

task.spawn(Control.Konsole.AttachClient, Konsole)
```

`Kick` names your own kick command. `Ban = false` and `Configs = false` leave those commands out.

The bar opens with `T` or `/konsole`, only for a player who holds a command in it. That includes the game's creator.

### Type a command

A command takes the last part of its key: `player.effects.sparkle` is `sparkle`. Clashing names take one more part, like `world_spin`.

A player command takes its targets, then its params. A server command takes no target and runs here. Quote text with spaces.

```
sparkle me
countdown "Doors open" 10
```

| Target | Means |
|---|---|
| `me` | You. |
| `all` or `*` | Everyone here, including you. |
| `others` | Everyone here but you. |
| A name or display name | That player. |
| The start of a name | Everyone it matches. |

A command with an array or table param is left out, with a warning.

### Kick and ban

- `kick <target> <reason>` runs your `Kick` command. It never kicks you.
- `ban <target> <reason> [seconds] [alts]` bans one player, by name or UserId, permanently without `seconds`. It refuses `all`, `others`, yourself, and a name matching several.

`kill`, `bring`, `tp` and `unban` are switched off. [Lift a ban from the console](/docs/moderation/ban-a-player).

### Read and override configs

`features` and `configs [key]` need `craftsman.config.read`. Only you see the listing.

`override <key> <value>`, `clearoverride <key>` and `clearoverrides` need `craftsman.config.override`. A feature's value is `on` or `off`. An override lasts until cleared or the server ends.

### Know what can stop a command

The bar shows every command to anyone who can open it. `Control.Run` can still refuse one they do not hold, a `structural` one, or any during a halt.

In Studio, you need API access and the attached Roblox account. A ban there answers `failed`.

---

# Collection: Store and purchases

Sell things, and look after what players bought.

---

## Page: /docs/store-and-purchases/store-catalogue

**Sell passes, products and subscriptions.** Make items with the CLI, declare what a product's receipt grants, and ask Commerce who owns what.

Make your items with the CLI, declare what they grant, and ask `Control.Commerce` who owns them.

> [!WARNING]
> Do not write your own `ProcessReceipt` or call `UserOwnsGamePassAsync`. Gifts, awards and revocations exist only in Commerce's record, so code asking Roblox never sees them.

### Make the items

```bash
craftsman store add --dry-run    # see what it would create
craftsman store add              # asks pass or product, name, price, icon...
```

It creates the item in every universe of your [places](/docs/getting-started/describe-your-places), and needs `craftsman.catalogue.apply`. **Save as draft**, the default, creates nothing until `craftsman store push`. A protected group's universe waits for a passkey in the browser. Roblox items are never deleted.

Make subscriptions in Creator Hub, then [import them](/docs/store-and-purchases/import-a-catalogue).

Commit `assets/index.toml` and `src/Control/Assets.luau`.

### Pick a key

Your game names an item by its key: `Commerce.Owns(player, "vip")`.

- Use letters, digits, dots and underscores, at most 59 bytes. Keys are case-sensitive.
- **Never rename a key.** Purchases are filed under it. `craftsman store edit <key>` changes what players see.
- A gift is a product keyed `<key>.gift`. Make one with `craftsman store gift <key>`.

### Declare the receipts

A product grants nothing until you declare its receipt in `Control.Declare`:

```lua
Receipts = {
	golden_pickaxe = {
		Grants = Control.Op(Player.Inventory, "GrantItem", { ItemId = "golden_pickaxe", Amount = 1 }),
		Reverse = Control.Op(Player.Inventory, "RemoveItem", { ItemId = "golden_pickaxe", Amount = 1 }),
	},
},
```

- `Grants` delivers it, on a ledger entity keyed by player.
- `Reverse` takes it back. Stamp each grant's `PurchaseId` on what you give, so `Reverse` removes that copy. See [Handle refunds](/docs/store-and-purchases/handle-refunds).
- `Once = true` lets a player buy it once.

A gift product declares no receipt.

### Wire it up

```lua
-- Stores module, beside the entity stores
Control.Commerce.Open({ Declaration = Definitions, Ledger = Ledger })

-- server Script, once
Control.Commerce.Start({ Declaration = Definitions, Ledger = Ledger })
```

Every entity a receipt names needs its store open, or its receipts wait.

At boot, the server log prints `[Commerce] Bound 2 products, 1 pass, 1 subscription ...`. `[Commerce] No catalogue ids` means nothing's for sale in this universe.

### Use it

| Call | Does |
|---|---|
| `Owns(player, "vip")` | Whether they hold the pass, however they got it. |
| `Subscribed(player, "premium")` | The same for a subscription. |
| `Prompt(player, key)` | Prompts a purchase, or refuses. |
| `PromptGift(giver, recipientUserId, key)` | Prompts a gift. Anything but `"prompted"` showed nothing. Use the item's own key. |
| `Changed` | Fires `(player, key, holds)`, including at join. |
| `Gifted` | Fires `(recipient, key, giverUserId, purchaseId)`. The toast is yours. |

All are on `Control.Commerce`. If a player's record fails to load, ownership falls back to Roblox.

### Ship it

IDs reach players only with a release. Until then, `Prompt` refuses the item. **Store** in the console lists pushed items, and needs `craftsman.store.read`.

Next: [Look up a player's purchases](/docs/store-and-purchases/look-up-purchases)

---

## Page: /docs/store-and-purchases/import-a-catalogue

**Import an existing catalogue.** Adopt the passes, products and subscriptions you already sell on Roblox with craftsman store import.

Bring items you already sell on Roblox into your repository. Nothing on Roblox changes.

### Run the import

```bash
craftsman store import --dry-run    # see what it would adopt
craftsman store import
```

1. It lists every pass and product Roblox has in your [places'](/docs/getting-started/describe-your-places) universes that the index does not hold yet.
2. Pick the ones to adopt.
3. Give each new item a key. Press Enter for the default.
4. Type any subscription IDs, as Creator Hub shows them, such as `EXP-...`, separated by spaces. Leave it blank for none.
5. Give each subscription a key.

Commit `assets/index.toml` and `src/Control/Assets.luau`. The IDs reach players with your next release.

You need `craftsman.store.read` and `craftsman.catalogue.apply`. In a universe with a protected place group, each adoption waits for your passkey in the browser.

### Choose keys carefully

Players' purchases are filed under the key, and you cannot rename it later.

The default key is the first of these that applies:

1. The key the console already holds for that ID. Any other key is refused before anything's sent.
2. For a product named `<Name> (Gift)`, the key of `<Name>` plus `.gift`. It becomes that item's gift product.
3. A key made from the name.

To adopt any other product as a gift, type `<key>.gift`.

### Import without a terminal

Give keys with `--key`, once per item, by name or ID. `--yes` takes the default for the rest.

```bash
craftsman store import --key "Golden Pickaxe=golden_pickaxe" --key VIP=vip --yes
```

### Know how items match

- Passes and products match by name, across universes. Items with the same name in two universes become one entry with two IDs.
- An item whose name is already in the index gets the missing universe's ID.
- Subscriptions match by ID, in whichever universes have that ID.

### Read what it skipped

The import needs `game-pass:read` and `developer-product:read` on each universe's Open Cloud key. A missing one is named, per universe. Add it in Creator Hub and import again.

- `skipped` on a universe means its list could not be read. The line gives the reason. The import then never says the index is complete.
- `skipped` on an item means its key is invalid or taken. Choose another with `--key`.
- `removed` means no universe adopted the item, so it was taken out of the index again.

| Exit code | Means |
|---|---|
| 0 | Everything it read was adopted, or already in the index. |
| 1 | A scope is missing, or an item was refused. |
| 3 | Roblox could not be read, so the result is not known. |

Next: [Sell passes, products and subscriptions](/docs/store-and-purchases/store-catalogue)

---

## Page: /docs/store-and-purchases/look-up-purchases

**Look up a player's purchases.** See what a player holds and how they got it, and find purchases that never reached them.

See what one player holds, or list purchases that went wrong across a universe. Both need `craftsman.store.read`.

### Look up one player

1. Open **Players & Data**, then **Account & actions**.
2. [Find the player](/docs/players-and-data/find-a-player).
3. Open the **Purchases** card.

The card reads from your game when you open it and when you press **Refresh**. Your Stores module must call `Control.Commerce.Open`, or it cannot read anything.

The table lists every pass and subscription you declare:

| Column | Shows |
|---|---|
| **Item** | The item, its key and its kind. |
| **State** | `Active`, `Revoked`, `Refunded`, `Lapsed`, `Expired` or *Not held*, with an end date if it has one. |
| **How** | *bought*, *awarded* or *gifted by* a UserId, and the reason if one was given. |
| **Roblox** | For a subscription, what Roblox reports, such as *Active, not renewing*. |

`Lapsed` means Roblox no longer reports a pass the player bought.

**History** lists everything that happened, newest first, including product purchases. Each row says where the claim came from, such as *bought*, *seen in game* or *refunded per Roblox*. An operation that was refused or failed keeps its own row with that status.

### Find purchases that went wrong

Open **Purchases** and pick a universe. Production is selected first.

**Unfulfilled** lists purchases from the last 45 days that granted nothing. The buyer paid and got nothing. Search, or filter by **Product** or **Reason**. Open a row for the buyer, price, reason and place.

The reason is what the game recorded. `unclaimed_gift` is a gift with no recipient on record. You can [assign it](/docs/store-and-purchases/award-an-item). To give any other buyer what they paid for, award them the item.

**Refunds and events** lists what Roblox's webhook sent. See [Handle refunds](/docs/store-and-purchases/handle-refunds).

Both lists load when you open the tab and when you press **Refresh**.

Next: [Award an item or assign a gift](/docs/store-and-purchases/award-an-item)

---

## Page: /docs/store-and-purchases/award-an-item

**Award an item or assign a gift.** Give a player a pass, subscription or product for free, or send an unclaimed gift to its recipient.

Give a player an item without a purchase, or send a paid gift to the person it was meant for.

### Award an item

You need `craftsman.store.award`.

1. Open **Players & Data**, then **Account & actions**, and [find the player](/docs/players-and-data/find-a-player).
2. Open the **Purchases** card and press **Award**.
3. Pick the item.
4. Set **Days**. A pass lasts forever if you leave it empty. A subscription needs a number. A product takes none.
5. Add a reason if you want one.
6. Press **Award**.

A product runs its `Grants`, the same as a purchase. A pass or subscription shows as `Active`, *awarded*. `Control.Commerce.Owns` and `Subscribed` count it, but it will not appear in the player's Roblox inventory.

The console tells the player's server right away. If that message is lost, they get it at their next save or join.

### Mind the limits

- The item's Robux price counts against your daily budget, if you have one. A subscription counts R$ 49 per 30 days.
- An item with no known price is refused.
- You cannot award a pass or subscription that is already `Active`.
- You cannot award a `.gift` product. Award the item it gives.

An award also restores a revoked item. Use the **Award** button on its row.

### Assign an unclaimed gift

A gift with no recipient on record is listed with the reason `unclaimed_gift`. You need `craftsman.store.gift.assign`.

1. Open **Purchases** and pick the universe it was bought in.
2. Under **Unfulfilled**, press **Assign** on its row.
3. Enter the recipient's user ID.
4. Press **Assign gift**.

A gift goes to one person, and never back to its giver. If the recipient already holds it, it is refused and stays unclaimed.

To let players gift each other in game, use `Control.Commerce.PromptGift`. See [Sell passes, products and subscriptions](/docs/store-and-purchases/store-catalogue).

Next: [Revoke an item](/docs/store-and-purchases/revoke-an-item)

---

## Page: /docs/store-and-purchases/revoke-an-item

**Revoke an item.** Take back an awarded, gifted or bought item in your game. Roblox refunds nothing.

Take an item back from a player. Every revoke needs a reason.

Open **Players & Data**, then **Account & actions**, [find the player](/docs/players-and-data/find-a-player), and open the **Purchases** card.

### Revoke an awarded or gifted pass or subscription

You need `craftsman.store.revoke`.

1. Press **Revoke** on the item's row.
2. Enter a reason.
3. Press **Revoke**.

### Revoke a bought pass or subscription

You need `craftsman.store.revoke_bought`, and a passkey every time.

1. Press **Revoke purchase** on the item's row.
2. Enter a reason.
3. Press **Revoke purchase**.

This applies in your game only. The item stays in their Roblox inventory, and nothing is refunded. `Control.Commerce.Owns` answers no, even though Roblox says they own it.

### Reverse a product

A product is reversed one purchase at a time, from **History**.

1. Find the purchase. An awarded product shows **Reverse**, which needs `craftsman.store.revoke`. A bought one shows **Revoke purchase**, which needs `craftsman.store.revoke_bought` and a passkey.
2. Enter a reason.
3. Confirm.

This runs the product's declared `Reverse`, on the copy carrying that `PurchaseId`. A purchase is reversed only once. If the product declares no `Reverse`, nothing is taken back.

### Undo a revoke

Press **Award** on the revoked item's row. See [Award an item or assign a gift](/docs/store-and-purchases/award-an-item).

Each item's price counts against your daily budget, if you have one. The console tells the player's server right away. If that message is lost, the change shows at their next save or join.

---

## Page: /docs/store-and-purchases/handle-refunds

**Handle refunds.** Choose what a product's refund does, and approve or decline the reversals that wait for a person.

Roblox tells you about refunds through its webhook. Choose what each product does, then review what is left.

### Connect the webhook

[Connect the webhook](/docs/privacy/connect-the-webhook) on your Production universe. Tick **Transaction Refunded** and **Commerce Product Order Refunded** in Creator Hub. Those two are the refund events that act.

The subscription events and **Commerce Product Order Paid** are only recorded.

### Choose what a product does

```bash
craftsman store edit coins_500 --on-refund reverse
```

- `propose` is the default. A person approves each reversal.
- `reverse` runs the product's `Reverse` by itself. Use it only on a product that declares `Reverse`.

### Know what happens to each item

- **Product:** it is matched to the purchase. It reverses by itself only if the match is exact and the product says `reverse`. Otherwise it waits for a person.
- **Pass:** Roblox takes it back. The player's record shows the change at their next join.
- **Subscription:** the event is recorded, and the game checks with Roblox at join.
- **Gift of a pass:** it is taken back from the recipient. An exact match does this by itself.
- **Gift of a product:** it follows the setting of the product it gives.

### Review the list

Open **Purchases** and look under **Refunds and events**. Open a row to see the player, the purchase and Roblox's raw body.

| Status | Means |
|---|---|
| **Proposal pending** | Waiting for a person. Press it to open **Approvals**. |
| **Running** | The reversal is in progress. |
| **Reversed** | The product was taken back. |
| **Not reversed** | Nothing was taken back, for example no `Reverse` is declared. |
| **Refused** | The game refused the reversal. |
| **Failed** | The result is not known. See [Refused or failed](/docs/concepts/refused-or-failed). |
| **Declined** | A person said no. |
| **Guess** | The event named no purchase, so one was picked by buyer, product and time. |
| **Info** | Nothing to act on, such as a pass or a subscription. |
| **Not ours** | The item is not in your catalogue. |
| **Unmatched** | The event could not be placed. The row's **Match** says why. |

### Approve a reversal

You need `craftsman.store.refund`.

1. Open **Approvals**.
2. Check the request. A guess always waits for a person.
3. Press **Approve** or **Decline**.

See [Approve a request](/docs/team-and-access/approve-a-request).

---

# Collection: Assets

Upload animations, sounds, images and video from code.

---

## Page: /docs/assets/assets

**Add animations, sounds, images and video.** Upload media with craftsman asset, keep every ID in your repository, and name them in code.

Upload your media with the CLI, then name each file in code instead of an ID.

### Add some

```bash
craftsman asset add ~/Downloads/Combat.rbxm ./sfx
```

Give it files or folders, or run it alone and drag them in. Rename what it found, then pick **Upload now**, **Save as draft** or **Cancel**. A draft uploads nothing until `craftsman asset push`, and it is the default.

| Kind | Files | Good to know |
|---|---|---|
| Animation | `.rbxm`, `.rbxmx` | A file holding several becomes one file each. |
| Audio | `.mp3`, `.ogg`, `.wav`, `.flac` | Counts against your monthly upload quota. |
| Image | `.png`, `.jpg`, `.jpeg`, `.bmp`, `.tga`, up to 8000×8000 | Needs the universe's [download key](/docs/project-settings/asset-and-download-keys). |
| Video | `.mp4`, `.mov` | Costs 2,000 Robux, so it waits for a passkey in the browser. |

Each file is at most 20 MB. Uploading needs `craftsman.assets.write`.

### Commit what it writes

Commit `assets/` and `src/Control/Assets.luau` with the change that uses them. A release refuses a module that is not what the index generates, and `craftsman project` writes it again. To move them, set `folder` and `module` under `[assets]` in [`craftsman.toml`](/docs/project-setup/craftsman-toml).

### Use them in code

```lua
local Assets = require("@game/ReplicatedStorage/Craftsman/Assets")

animation.AnimationId = Assets.Animations.Combat.Idle
sound.SoundId = Assets.Audio.Theme
icon.Image = Assets.Images.ui.Coin
```

Folders become the path. Each part is a Luau identifier, so `Probe Chime.wav` becomes `ProbeChime`. A draft is not in the module, so code naming one fails to type-check until it is uploaded.

The right ID is picked at runtime from `game.GameId`. An unpublished Studio file uses your Development universe's.

`craftsman check` warns on any `rbxassetid://` in your code.

### Know who owns the upload

Universes under the same group or user share one ID, so each file uploads once per owner.

### Check before you release

```bash
craftsman asset status
```

It exits 1 on a draft, a changed file, anything moderation has not approved, or a stale module. Use it in CI. A new asset reaches players only with a release.

### Browse what you have

Run `craftsman asset` alone to browse the index and copy an entry's `Assets.` path. **Assets** in the console lists every upload and its moderation status, and needs `craftsman.releases.read`.

Next: [Replace an asset](/docs/assets/replace-an-asset)

---

## Page: /docs/assets/replace-an-asset

**Replace an asset.** Swap an asset's file with craftsman asset replace. It always gets a new ID, and you can roll it back.

Give an existing entry a new file. Your code keeps the same name.

### Replace the file

```bash
craftsman asset replace Combat/Idle ~/Downloads/Idle.rbxm --dry-run
craftsman asset replace Combat/Idle ~/Downloads/Idle.rbxm
craftsman asset push
```

It shows each owner's current ID going to a new one, then asks **Upload now**, **Save as draft** or **Cancel**. If you upload now, you do not need `push`.

A replacement is always a new ID. Roblox would keep an animation's ID if you versioned it, but every universe under that owner shares the ID, Production included.

### Know when it goes live

The current ID stays live until the new file is uploaded for every owner. Then the old ID moves to the entry's history.

Until then, `craftsman asset list` and `status` say a replacement is waiting, and `status` exits 1.

The new ID reaches players with your next release. Commit `assets/` and `src/Control/Assets.luau` first.

### Change your mind

Replace it again with the file that is live now. The waiting replacement is withdrawn.

### Roll back

```bash
craftsman asset rollback Combat/Idle
```

Pick a version from the entry's history and confirm. It puts that version's IDs back and restores its file from Git.

- It refuses while the file has uncommitted changes. Commit or stash them first.
- It refuses if no commit holds that file.
- A rollback abandons a waiting replacement.

A rollback also reaches players with your next release.

Next: [Import assets you already have](/docs/assets/import-assets)

---

## Page: /docs/assets/import-assets

**Import assets you already have.** Record an asset that is already on Roblox with craftsman asset import, and download its file.

Bring an asset that is already on Roblox into your index. Nothing is uploaded.

### Import one

```bash
craftsman asset import 73231190282617 --name ProbeChime
```

It records the ID and downloads the file into `assets/`. Without `--name`, it asks and suggests the asset's display name. You need `craftsman.assets.write`.

Commit `assets/` and `src/Control/Assets.luau`. Then name it in code as `Assets.Audio.ProbeChime`. The ID reaches players with your next release.

### Know what you get back

Roblox re-encodes media, so the file may not match what was uploaded.

- **Audio is always `.ogg`**, whatever was uploaded.
- **An image comes back as `.png` or `.jpg`.**
- **A decal's ID is recorded as the image inside it**, because an `ImageLabel` needs the image. The CLI says so when it happens.

### Fix a refusal

- **It belongs to another owner.** The asset's owner must own one of the universes in your [`craftsman.toml`](/docs/project-setup/craftsman-toml) `[places]`.
- **It's already in the index.** Each ID is imported once.
- **It's not media.** Only animations, audio, images and video can be imported.
- **The name is taken.** Choose another with `--name`.

### Add a missing file

Downloading needs the universe's [download key](/docs/project-settings/asset-and-download-keys). Without one, the ID is recorded with no file, and the CLI says so.

An entry with no file cannot be uploaded to another owner. Give it one with `craftsman asset replace <name> <path>`. See [Replace an asset](/docs/assets/replace-an-asset).

---

# Collection: Releases

Build, test and deploy your game.

---

## Page: /docs/releases/release-workflow

**Set up the release workflow.** Get .github/workflows/release.yml ready, so GitHub can build release candidates.

GitHub builds your releases with `.github/workflows/release.yml`. `craftsman init` writes it. Without `init`, copy `templates/release.yml` from the CLI's repository, `averyark/craftsman-cli`.

### Get your repository ready

- **Name your repository in the console.** Go to **Settings** > **Releases** > **GitHub repository**. There is no secret to add: the run signs in with GitHub's OIDC token.
- **Add `rokit.toml`.** List `craftsman` and `rojo`, plus `wally` if you have a `wally.toml`.
- **Commit `craftsman.lock`.** The workflow installs exactly what it names.
- **Use Git LFS** for any committed `"studio-download"` place file.

### Know when it runs

It runs only when `craftsman publish --apply` starts it, never on a push. It builds each `[places.<role>]` in `craftsman.toml` from the commit you pushed, then hands the candidates to the console.

Nothing goes live until someone deploys. The workflow holds no Roblox key and cannot publish a place.

### Know what each job does

| Job | What it does | Holds a token |
|---|---|---|
| Prepare | Lists commits and changelogs, and downloads your packages and each place's Studio place. Uses only git, curl, jq and Python's standard library. | Yes |
| Build | Installs your tools with Rokit, runs `craftsman install --frozen`, runs `wally install` if you have a `wally.toml`, and runs `craftsman release` for each place. | No |
| Hand over | Checks both jobs' files against their hashes, then hands the candidates to the console. Uses only curl and jq. | Yes |

The build also compares save data against the last deployed commit. `craftsman summary` writes a summary to the run's page.

> [!WARNING]
> If you edit the workflow, never install or run a tool in Prepare or Hand over. Any step in a job that can mint the OIDC token can act as your repository.

### Point it at another console

It reaches the hosted console by default. To use another, set two repository variables:

- `CONTROL_RELEASE_ENDPOINT`: that console's release address.
- `CONTROL_RELEASE_AUDIENCE`: its origin.

### Upgrade the workflow

A workflow from CLI 0.10.0 or earlier cannot read today's `craftsman.lock`. When you move to a newer CLI, replace the file with the current template. Commit it with your new lock.

Next: [Build a release](/docs/releases/build-a-release)

---

## Page: /docs/releases/build-a-release

**Build a release.** Start a build with craftsman publish, and watch it become a release candidate.

`craftsman publish` builds your pushed commit into a release candidate.

### Build it

1. Commit and push your branch. Uncommitted changes are not built, and `origin` must be the repository you named in **Settings** > **Releases**.
2. Preview it. This changes nothing.

```bash
craftsman publish
```

3. Start the build.

```bash
craftsman publish --apply
```

It starts the workflow with the GitHub login you push with. The terminal draws the run, then tests the candidate in Roblox. The build is now under **Release candidates** in **Releases**.

A publish activates nothing. Your declarations go live in a place group when someone deploys there.

### Read the result

| Terminal says | What it means |
|---|---|
| `Release candidate` | It is ready to test and deploy. |
| `Needs a Studio place` | Roblox did not hand over a Studio place. [Upload one](/docs/releases/upload-a-studio-place). |
| `The build failed` | The GitHub run failed. The terminal prints its link. |
| `Built, and not handed over` | The run passed, but the console has no release from it. |

### Know what goes in

- **Your code,** from the commit you pushed.
- **The newest Studio save** of the default place group's place. The console marks that group **Studio source**.

**Changed in Studio** on the release lists paths that changed outside your code folders.

One build covers every place group. It carries every commit since the last deploy, listed on its **Changelog** tab.

### Change what it does

| Option | What it does |
|---|---|
| `--group <group>` | Builds for this group only. |
| `--no-watch` | Starts the build and returns. |
| `--no-test` | Skips the test in Roblox. |
| `--watch` | Starts nothing. Attaches to the newest build of your current commit. |

[Publish and release commands](/docs/cli/publish-and-release) has the full list.

Next: [Test a build](/docs/releases/test-a-build)

---

## Page: /docs/releases/test-a-build

**Test a build.** Run your Jest specs on a real Roblox server, from the console or the terminal.

Run your `verify/` specs on a real Roblox server. There are three ways.

### Test a release in the console

1. Open **Releases** and pick a build.
2. Choose **Tests**.
3. Press **Verify**, or **Verify again**.

It usually takes under a minute. It publishes nothing and asks for no passkey. It saves a place version nobody starts on, and runs your specs against it.

You need `craftsman.releases.build`.

### Test from a publish

`craftsman publish --apply` tests the candidate it builds. Each distinct file is tested once.

```bash
craftsman publish --apply            # build and test
craftsman publish --apply --no-test  # build only
craftsman publish --watch --test     # test the build you attach to
```

Exit 0 means passed, 1 means failed and 3 means no verdict. On a group that deploys itself, the test runs beside the deploy, not before it.

### Test your checkout

```bash
craftsman test-roblox
```

This needs no commit or release. With more than one place, name it: `craftsman test-roblox lobby`. Your code goes up as a private test model, and no place version is saved. It takes `craftsman.releases.test`, and your key needs `asset:read` and `asset:write`.

The result cannot unlock a deploy. Only a test of the release's own file does.

`--save-place-version` tests a saved place version instead. It works only on an open group that is not the default, and needs `craftsman.releases.deploy.open`.

### Read the verdict

- **Passed.** Every test passed.
- **A red failure.** A test failed. Expand a spec file to see which.
- **Not verified.** The tests could not run, so this says nothing about your build. Fix what it names and run it again.

A run with no tests is not a pass. A place without `ServerScriptService.Craftsman.Verify` has no tests to run.

A verdict belongs to the file. Every group's copy is the same file, so a pass on Development counts on Production. The newest verdict wins.

> [!WARNING]
> Tests run in your real universe, against your live data stores. Have specs use store names your game does not use.

Next: [Deploy a release](/docs/releases/deploy-a-release)

---

## Page: /docs/releases/deploy-a-release

**Deploy a release.** Send one build to one or several place groups, with one passkey.

Send a build to one or several place groups at once.

### Deploy it

1. Open **Releases** and pick a build under **Release candidates**.
2. Under **Can deploy to**, tick each group. The default group starts ticked.
3. Sort out anything listed under **Resolve or untick these to deploy.**
4. Press **Deploy to Production**, or **Deploy to 2 groups**.
5. Confirm with your [passkey](/docs/concepts/passkey-window) if any ticked group is protected. You are asked once.

The group moves under **Deployed to**, with its Roblox version. New servers start on it. Running ones keep theirs.

The release's features, configs, operations and permissions go live in that group too.

You need `craftsman.releases.deploy.protected` for a protected group and `craftsman.releases.deploy.open` for an open one. A batch needs each key it touches.

### Know how a batch runs

- **All or nothing to start.** One refused group stops the whole batch. Fix it or untick that group.
- **Default group first,** then the others in order.
- **It stops at the first failure.** Later groups show **Not deployed** and stay candidates. Earlier ones stay deployed.

### Look before you send

Click a group's line to see its release, its checks and its tests. This never ticks it.

A group with no copy says **Not in this build** or **No place mapped**.

### Finish a partial deploy

A release can show **Published as version 12, not finished**. The place went out, but its declarations did not activate.

Press **Finish deploying**. It does not upload the place again.

Next: [Read and answer deploy warnings](/docs/releases/deploy-warnings)

---

## Page: /docs/releases/protected-or-open

**Make a group protected or open.** Choose how much stands between a build and each place group.

Set how each place group deploys.

### Change a group

1. Go to **Settings** > **Releases**.
2. Under **Deployment**, find the group.
3. Choose **Protected** or **Open**.
4. On an open group, set **Auto deploy** and **Require tests**.
5. Press **Save changes**, and confirm with your passkey.

You need `craftsman.settings.write`.

### Compare the options

| | Protected | Open |
|---|---|---|
| Passkey to deploy | Always | Never |
| Passing test | Always | With **Require tests** |
| Deploys itself | Never | With **Auto deploy** |
| Permission to deploy | `craftsman.releases.deploy.protected` | `craftsman.releases.deploy.open` |

Every group takes builds from any branch. A protected group shows the branch on its line, such as `feature/x → Production`, so read it before you deploy.

Development starts open, with no required tests. Other groups start protected.

### Know what Auto deploy will not do

It deploys the newest candidate within a minute or so. It answers no questions for you, so it stops on:

- unread warnings
- a required test that has not passed
- a Studio publish it would overwrite
- a snapshot it cannot take, or one over an hour old

The release stays in the list with the reason. Deploy it by hand.

With **Require tests**, an untested release does not wait for a test that is still running. It stops with `Not deployed automatically`, and you deploy it by hand.

---

## Page: /docs/releases/deploy-warnings

**Read and answer deploy warnings.** Clear warnings and the live-version check, and see what you cannot override.

Some deploys stop and ask you first. Nothing goes out until you answer.

### Read the warnings

1. Find the line that says **Production has 2 warnings to read.**
2. Press **Read them**, then open the **Warnings** row on the release below.
3. Tick **I have read them**.
4. Press the deploy button.

More warnings can appear after you press it, such as a Studio save. Tick **I have read these warnings** and press again.

### Deploy over a Studio publish

The console checks which version is live. It stops if that is not the one it last deployed:

> Live version 131 was published from Studio after Project Control last published 130. Deploying erases its changes.

To go ahead:

1. Tick **Deploy over version 131**.
2. Press **Deploy to Production again**.

If another publish lands first, you are asked again about that version.

Your first deploy through the console asks the same thing. Afterwards the release notes which version it replaced.

If the console cannot read the live version, it refuses. Check that your key holds `asset:read`. See [Asset and download keys](/docs/project-settings/asset-and-download-keys).

### Know what you cannot override

These stop a deploy until the cause is gone:

- **A failing test,** where the group requires tests. Fix it and build again.
- **A build that hasn't passed,** where the group requires tests. [Test it](/docs/releases/test-a-build).
- **Save data older servers can't read.** The release says so under **Save data**.
- **Save data this release would damage.** The refusal names the rule it broke.
- **Someone in [Team Create](/docs/releases/team-create).**

A failed [snapshot](/docs/releases/save-data-snapshot) has its own answer.

---

## Page: /docs/releases/save-data-snapshot

**Snapshot save data before a deploy.** What the data store snapshot does, and how to answer when it fails or is old.

A release that changes save data takes a Roblox data store snapshot before it uploads. You do not start it. A first deploy through the console always takes one, unless the game declares no Craftsman entities.

Documents keep a pre-release version for 30 days. The release page notes the snapshot's time.

Your Open Cloud key needs `universe-datastores.control:snapshot`. Without it, the deploy stops and names the scope.

### Answer a failed snapshot

The deploy stops and nothing is published.

1. Read the reason on the release.
2. Fix it and deploy again.

Or press **Deploy without a snapshot** on the release. Only Roblox's hourly versions will predate the release, and the page says so.

That button only appears after a snapshot has failed.

### Accept an older snapshot

Roblox allows one snapshot per universe per UTC day. A later deploy gets that day's snapshot back. That is fine within the hour.

If it is over an hour old, the deploy stops and nothing is published. Documents written since then are covered only by Roblox's hourly versions. Choose one:

- Press **Deploy with the 00:05 UTC snapshot**. The button names the time.
- Wait for the next UTC day.

### Deploying by itself

A group with **Auto deploy** never answers these. The release stops with the reason. Deploy it from the console.

---

## Page: /docs/releases/upload-a-studio-place

**Upload a Studio place.** Rebuild a release on a place file you upload, including one marked Needs Studio place.

Upload a place file, and the release's commit is rebuilt on it.

### Fix Needs Studio place

A build shows **Needs Studio place** when Roblox did not hand over the newest Studio save. The release says why.

1. In Studio, choose **File** > **Download a Copy**.
2. In **Releases**, open the build.
3. Add a note under **What changed in Studio** if you want.
4. Press **Upload place file** and pick the `.rbxl` or `.rbxlx`.

GitHub rebuilds that commit on your file. The new release takes the old one's place when it arrives.

If you have fixed the cause, press **Try Asset Delivery again** instead. A build fetches Studio saves with a download key. See [Asset and download keys](/docs/project-settings/asset-and-download-keys).

### Rebuild on a different place

1. Open the release.
2. In the **Build** card, press **Rebuild on another place**.
3. Upload the file, as above.

The rebuild replaces the release, and its code does not change.

A copy in another group cannot be rebuilt. Rebuild the default group's release, or publish again.

### Check who may do it

You need `craftsman.releases.build`. On a group that deploys itself, you need `craftsman.releases.deploy.open`, because the rebuild deploys.

The **Build** card shows which Studio place a release used.

---

## Page: /docs/releases/team-create

**Clear Team Create before a deploy.** Why a deploy is refused while a place is open in Team Create, and what to do.

Roblox takes no place upload while anyone has the place open in Team Create. There is no override.

### Clear the place

1. Open the release. It names who is in:

   > builder7 has this place open in Team Create. A deploy now will be refused; ask them to close Studio.

2. Ask them to close Studio.
3. Press **Check again**. The line goes away when nobody's in.
4. Deploy.

A deploy pressed too early is refused by name, before your passkey.

### If it still says no

The lock can last a few seconds after the last person leaves. Wait, then deploy again. See [Server is busy](/docs/troubleshooting/server-is-busy).

### If the console cannot check

The release says **Could not check who has this place open in Team Create.** This does not block a deploy.

To see names, add `legacy-team-collaboration:manage` to your Open Cloud key.

**Finish deploying** is not affected. It uploads nothing.

---

## Page: /docs/releases/write-a-changelog

**Write a changelog.** Add a changelog to your commit, so it shows on the release.

Commit a changelog with your change. Whoever deploys reads it on the release's **Changelog** tab.

### Add the file

Create `changelogs/<yyyy-mm-dd>-<topic>/CHANGELOG.md`:

```markdown
## Players keep rewards when they leave a match

Leaving early used to cost players what they'd earned.

### Changed

- Players who leave mid-match keep their rewards.

### Before deploying

- Set `Match.RewardCap` first.
```

Commit it with your change, and it goes with the next build. It does not need its own commit.

If your repository has the `/changelog` skill, run it before you commit. It reads your change, writes this file and stages it. Running it again rewrites the same file.

### Follow the rules

The workflow skips a changelog that breaks one, with a warning on the run.

- **Topic:** lowercase letters and digits, joined by single hyphens.
- **Folder name:** at most 80 characters.
- **File size:** at most 64 KB.
- **Per build:** at most 20.

Only a newly added file is sent. Editing an old folder's changelog sends nothing, so make a new folder.

### Write what the console can show

It renders headings, lists, paragraphs, code, bold, italics and `https` links. Tables, images and HTML show as plain text.

Never quote secrets, keys or player data.

### Read the tab

- **No changelog in this build.** Nobody added one.
- **This build did not collect changelogs.** The run did not look.

Deploying never reads a changelog. It is for people.

---

## Page: /docs/releases/release-health

**Check a release's health.** Compare a deployed version's first hour with the version before it.

See how a deployed version compares with the one it replaced.

### Read it

1. Open **Releases** and pick a deployed release.
2. In the **Health** card, press **Read health**.
3. Press **Read again** later for more data.

It reads only when you press. Your Open Cloud key needs `universe.analytics:read`. If it cannot read, the card says why.

### Read the table

| Section | Compares | Metrics |
|---|---|---|
| **Players' clients, same hour** | Both versions, over the hour after the deploy | Client crash rate, Client frame rate, Client memory, Session length |
| **Servers, all versions** | The hour before against the hour after | Server crashes, Server frame rate, Server memory |

Server rows carry no version, so a change may just be the time of day.

The last column says:

- **Worse** or **Better.** The number moved at least 10%, in that direction.
- **About the same.** It moved less than 10%.
- **Too early to say.** One side has under ten minutes of data.
- **No comparison.** One side has no number.

**Worse** describes the number, not the cause. You decide whether the release did it.

A quiet place may never have enough data. **Not enough data on both versions to compare** is not a good result, just no result.

---

# Collection: Team and access

Choose who can do what.

---

## Page: /docs/team-and-access/add-a-member

**Add a member.** Put a person with a console account on your project, with or without roles.

Put somebody on your project from Members. Only Craftsman Staff see **Add member**, so ask them if you do not.

### Add them

1. Go to **Settings** > **Members**.
2. Press **Add member**.
3. Search by Discord username or ID, and pick the person.
4. Tick any **Roles** you want to give them. This is optional.
5. Press **Add** with their name, and confirm with your passkey.

They are on the project straight away. There is no invitation to accept.

### Know who you cannot add

- **No account yet:** they need a sign-up link first. See [Create your account](/docs/getting-started/create-your-account).
- **Frozen accounts** are not listed.

### Give them access

Being on a project gives no access. A person with no role shows **No access yet** in Members.

They see "You have no access here yet" on the Overview, with an **Ask for access** button. It notifies the people who can give them a role.

Next: [Create and assign roles](/docs/team-and-access/roles)

---

## Page: /docs/team-and-access/roles

**Create and assign roles.** Group permissions into a role, then give it to members.

Build a role from permission keys, then give it to people. You need `craftsman.roles.write` to edit roles and `craftsman.members.assign` to give them.

### Create a role

1. Go to **Settings** > **Roles**.
2. Press **+ Create**, and confirm with your passkey. You get a role called "New Role".
3. On **Display**, set the name and color.
4. On **Permissions**, press **Allow** or **Deny** on each key you want. **Inherit** clears it.
5. Press **Save changes** in the bar at the bottom, and confirm with your passkey.

One tap covers the next ten minutes. See [the passkey window](/docs/concepts/passkey-window).

### Choose keys

Keys form a tree. An allow on a branch covers everything below it. `*` covers every key, including ones declared later.

- The longest matching key decides, so you can allow `player` and deny `player.moderation.kick`.
- A deny only narrows the role it is in. Another role's allow still counts.
- You can only put keys you hold into a role, and that is checked for the whole role.
- **Staff only** keys cannot be held by any role.

### Give a role

From the role:

1. Open **Manage Members**.
2. Press **+ Add Members**.
3. Tick the people, then press **Add**.

From Members:

1. Press **+** in the person's **Roles** cell.
2. Pick the role, then press **Assign role**.

Both ask for your passkey. Only people on the project can get a role, and you cannot give one to yourself unless you hold Superpower.

### Change or delete a role

A saved change applies to everyone holding the role.

Drag roles to reorder them. The order only decides which role's color a member's name shows. Color and order are saved in your browser, not for your team.

To delete a role, remove everyone from it first. Then press **Delete Role** on **Display**.

Next: [Remove a member](/docs/team-and-access/remove-a-member)

---

## Page: /docs/team-and-access/grant-a-permission

**Grant a permission to one person.** Give one person one key using a role, and withdraw a direct grant they already hold.

The console has no form for issuing a direct grant. Use a one-key role instead.

### Give one key with a role

1. Go to **Settings** > **Roles**.
2. Press **+ Create** and name the role after the job.
3. On **Permissions**, press **Allow** on the key.
4. Press **Save changes**, and confirm with your passkey.
5. Open **Manage Members**, press **+ Add Members**, tick the person, and press **Add**.

To take it back, press **Remove** beside their name, then **Remove** again. It is immediate.

A role has no end date, so remove it when the job's done.

### See a direct grant

A direct grant is one allow on one key, for one person, on one project. It always has a reason and an expiry, and it ends by itself.

1. Go to **Settings** > **Members**.
2. Press the three dots on the person's row.
3. Look under **DIRECT GRANTS**.

### Withdraw one

You need `craftsman.members.assign`.

1. Press **Withdraw** beside the key.
2. Confirm with your passkey.

It stays active for 15 minutes, and the row shows the time left. To stop someone now, [suspend them](/docs/team-and-access/suspend-someone).

Next: [Create and assign roles](/docs/team-and-access/roles)

---

## Page: /docs/team-and-access/remove-a-member

**Remove a member.** Take someone off your project, or take away one role, and see what is immediate.

Dismiss someone to take them off your project. You need `craftsman.members.assign`.

### Dismiss them

1. Go to **Settings** > **Members**.
2. Press the dismiss icon on their row, or the three dots and then **Dismiss member…**.
3. Read what they will lose, then press **Dismiss**.
4. Confirm with your passkey.

They lose their roles and direct grants now. Accounts attached to this project are detached too. They can be added again later.

Craftsman Staff cannot be dismissed. Someone with Superpower cannot be dismissed until Craftsman Staff remove it.

### Take away one role

1. Press the **✕** on the role in their row.
2. Press **Remove role**, and confirm with your passkey.

You can also do it from the role's **Manage Members** tab.

### Know what is immediate

| Action | Takes effect |
|---|---|
| Dismiss a member | Now |
| Remove one role | Now |
| Detach an account | Now |
| Withdraw an unanswered invitation | Now |
| Withdraw a direct grant | After 15 minutes |
| Remove Superpower, Craftsman Staff only | After 15 minutes |

A project's last Superpower cannot be removed.

Their in-game access goes with their roles.

Next: [Suspend or freeze someone](/docs/team-and-access/suspend-someone)

---

## Page: /docs/team-and-access/suspend-someone

**Suspend or freeze someone.** Lock a person out now, on one project or everywhere, and lift it again later.

Lock someone out without removing them. Neither brake asks for a passkey.

| | Suspend | Freeze |
|---|---|---|
| **Locks them out of** | This project | Every project |
| **Who can do it** | Superpower or Craftsman Staff | Craftsman Staff |
| **Who can lift it** | Another administrator or Craftsman Staff | Craftsman Staff |

Both cover the console, the CLI and running games. Both take effect now and are announced in your audit channel.

### Suspend someone

1. Go to **Settings** > **Members**.
2. Press the three dots on their row.
3. Press **Suspend on this project…**.
4. Add a reason if you want.
5. Press **Suspend now**.

Their row shows **Suspended**. Their other projects are not affected.

### Freeze an account

Craftsman Staff use **Freeze account…** and **Freeze now** in the same menu. The row shows **Frozen**.

### Lift it

1. Press the three dots on their row.
2. Press **Lift suspension** or **Unfreeze account**.
3. Confirm with your passkey.

### Know the limits

- You cannot suspend yourself from Members, or lift your own suspension.
- Only Craftsman Staff can suspend Craftsman Staff.

### Use Discord in an emergency

`/freeze` works from Discord, and it takes a Discord user. See [Freeze someone](/docs/emergencies/freeze-someone).

---

## Page: /docs/team-and-access/approve-a-request

**Approve a request.** Review what is waiting on you in Approvals, then approve, decline or cancel it.

Approve or decline what is waiting from the **Approvals** tab. The bell links here too.

### Approve it

1. Open **Approvals**.
2. Press the request to open it. **Approve** stays disabled until you do.
3. Read what it changes.
4. Press **Approve** or **Confirm**, and confirm with your passkey.

The page tells you what happened. If you cannot approve a request, its row says why.

It does not update by itself. Press **Refresh** to read it again.

### Know the groups

| Group | What it is | Who can act |
|---|---|---|
| **Confirmations** | Asked for from a terminal | The person who asked may confirm it from their browser |
| **Account attachments** | A linked account acting on this project | An administrator. See [Attach an account](/docs/team-and-access/attach-an-account) |
| **Operations awaiting a second approver** | Structural operations a game declared | Somebody other than the requester |
| **Unreadable requests** | Rows whose rule cannot be read | Nobody can approve them. Decline them and ask again |

A data edit shows each field's value when requested and what it becomes. Only those fields are written, and it is refused if any has changed since.

### Watch the clock

- Approving does not skip a delay. The row says when it can run.
- A request with an expiry says so. It lapses if nobody acts.

### Decline or withdraw

1. Press **Decline**, or **Withdraw** on your own request.
2. Press **Decline it** or **Withdraw it**.

It will not run, and the audit records it.

### Cancel something scheduled

**Scheduled** lists operations waiting only on their delay. There is nothing to approve.

Press **Cancel**, then **Cancel it**, to stop one.

Next: [What refused and failed mean](/docs/concepts/refused-or-failed)

---

## Page: /docs/team-and-access/link-accounts

**Link your Roblox account.** Prove a Roblox account is yours from your Account page.

Link an account to prove it is yours. Linking gives no access by itself.

### Link one

1. Open the account menu and press **Your account**.
2. Press **Link a Roblox account**.
3. Sign in at Roblox and allow it.
4. Back on the page, press **Confirm with a passkey**.

Finish in the same browser within ten minutes. **Not now** drops the attempt.

You can link several accounts. Each account can belong to only one person.

### Use it on a project

Linking proves who you are. Attaching lets the account act for you on one project.

A Roblox account attached there lets you run commands in its games.

Each account on the page lists where it is **Attached**, where it is **Waiting**, and **Not attached** when it is neither.

### Unlink one

1. Press **Unlink** on the account.
2. Press **Unlink with a passkey**.

It is detached from every project too.

Next: [Attach an account to a project](/docs/team-and-access/attach-an-account)

---

## Page: /docs/team-and-access/attach-an-account

**Attach an account to a project.** Let a linked Roblox account act for you on one project, with an administrator's approval.

Attach a linked account so it can act for you on a project. You need to be on the project, and the account needs to be [linked](/docs/team-and-access/link-accounts).

Attaching gives no permission. Roles do.

### Request your own

From your account page:

1. Under the account, choose a project.
2. Press **Request attachment**, and confirm with your passkey.

Or from the project:

1. Go to **Settings** > **Members**.
2. Press **+** in the **Accounts** cell on your row.
3. Pick the account, press **Request attachment**, and confirm with your passkey.

The account shows as waiting until someone approves it. The request lapses after 7 days.

### Request one for a member

You need `craftsman.members.attach`. You cannot see what a member has linked, so ask them for the account.

1. Press **+** in the **Accounts** cell on their row.
2. Type the Roblox UserId. A username is not accepted.
3. Press **Request attachment**, and confirm with your passkey.

The account must already be linked by that member. Another administrator approves it.

### Approve it

An administrator is anyone holding `craftsman.members.attach`.

- Another administrator approves it in [Approvals](/docs/team-and-access/approve-a-request).
- A sole administrator can approve their own.
- Anyone else cannot approve their own.

It attaches as soon as it is approved.

### Detach it

The owner or an administrator can do this.

1. Press the account in Members.
2. Press **Detach…**, then **Detach now**.
3. Confirm with your passkey.

It stops acting for them on that project at once.

Next: [Give someone access in game](/docs/team-and-access/in-game-access)

---

## Page: /docs/team-and-access/add-a-passkey

**Add a passkey.** Save a second passkey on another device, and remove one you no longer use.

Keep a second passkey on another device. If you lose your only one, you cannot sign in.

### Add one

1. Open the account menu and press **Your account**.
2. Under **Your passkeys**, press **Add another passkey**.
3. Choose where to save it, and follow your browser's prompt.

If you have not signed in within the last 5 minutes, you confirm with an existing passkey first.

The device you are on may already hold one. If it does, you will see "That device already has a passkey here." Choose a phone or a security key instead.

You can hold up to 5.

### Read the list

Each passkey shows how it was saved and when it was last used.

- **Follows an account:** it syncs through a cloud account.
- **One device:** it lives only on that device.

### Remove one

1. Press **Remove** on the passkey.
2. Press **Remove with a passkey**.

Other browsers on your account are signed out. You cannot remove your only passkey, so add another first.

Next: [How long a passkey tap lasts](/docs/concepts/passkey-window)

---

## Page: /docs/team-and-access/cli-sessions

**See and revoke CLI sessions.** Check where the CLI is signed in as you, and sign a machine out.

See every session the CLI holds as you, and end any of them.

### See your sessions

Open the account menu, press **Your account**, and find **CLI sessions**.

Each row shows the CLI version, when it was last used and when it ends. It does not name the machine.

A session ends after 30 days unused, or 90 days after it signed in, whichever comes first. The row shows when.

Only `craftsman login` starts a session, and it asks for your passkey. It works on the projects you are on.

### Sign a machine out

1. Press **Revoke** on the row.
2. Press **Revoke** again to confirm.

Press **Revoke all** to sign out every machine. It appears when you have more than one.

No passkey is needed. That machine needs `craftsman login` again.

### Check a session you did not start

A new session sends you a notification. Its link opens the row ready to revoke.

If you do not recognise one, revoke it.

### Sign out from the terminal

```bash
craftsman logout
```

Next: [Sign in from the terminal](/docs/getting-started/sign-in-from-the-terminal)

---

## Page: /docs/team-and-access/in-game-access

**Give someone access in game.** Let a team member run commands inside your game, from their attached Roblox account and role.

Let a team member run commands from inside your game. Their role in the console decides what they can run, so there is no admin list to keep.

### Set them up

1. They [link their Roblox account](/docs/team-and-access/link-accounts).
2. They [attach it to your project](/docs/team-and-access/attach-an-account).
3. You [give them a role](/docs/team-and-access/roles) that allows the command's permission.

The change reaches running servers within seconds.

### Allow the command in game

Commands are `structural` by default, and a structural command never runs in game. Lower its tier to allow it:

```lua
PlayerCommands.Now = Control.Command({ Reason = Reason })
	.Label("Kick Player")
	.NoImpact()
	.Tier("routine")
	.Handles(function(directive)
		-- kick the player
		return true
	end)
```

`.Tier("routine")` or `.Tier("valued")` is the only switch. These stay console-only:

- A command whose permission asks for a passkey.
- A command limited by a budget or per-target cap on someone's grant.

### Check it in code

```lua
local ok, sentence, reason = Control.Access.CanRun(player.UserId, "player.kick.now")
```

`reason` is a stable word:

| Word | Means |
|---|---|
| `not_attached` | No Roblox account attached |
| `not_held` | They do not hold the key here |
| `structural` | The command's tier is too high |
| `passkey` | The key asks for a passkey |
| `frozen` | They are locked out |
| `halted` | Operations are halted |
| `unmapped` | This place is not in a place group |

Use `CanRun` for buttons, not `HasPermission`.

Your game must call `Control.Reporter.Start` and `Control.Start`, with `Access` left on.

Until a server has read the access list, nobody holds anything. If a later read fails, it keeps the last good list.

Next: [Run commands in game and check access](/docs/servers-and-commands/run-in-game)

---

# Collection: Monitoring

See how your game and your project are doing.

---

## Page: /docs/monitoring/read-the-overview

**Read the Overview.** See what needs you, how your project is laid out, who is playing and what changed.

Open **Overview** to see what needs you, how your project is doing and what changed lately.

### Check what needs attention

**Needs attention** appears only when something's wrong. Each row has a button to the fix. Red rows are critical.

| Row | What it means |
|---|---|
| **Operations are halted** | Someone [halted operations](/docs/emergencies/halt-operations). |
| **Fallback on** | Configs are [running code values](/docs/emergencies/use-a-fallback). |
| **held values in** a group | A stored value no longer fits its type. [Re-enter it](/docs/features/change-a-config). |
| **A server reported a manifest mismatch** | A server runs a declaration that was not published. |
| **Running manifest matches no published one** | Publish what those servers run, or deploy a release that carries it. |
| **A server fell back to in-code fallbacks** | [The game could not reach its config](/docs/troubleshooting/fell-back-to-defaults). |
| **waiting for your approval** | Open [Approvals](/docs/team-and-access/approve-a-request). |
| **right-to-erasure request** | One is [waiting or failed](/docs/privacy/erasure-requests). |
| **signed documents** | They expire soon, or have. New servers use code values once they have. |
| **Something else is writing** | Something outside Project Control writes `CraftsmanProjectControl`. Find it. |
| **Open Cloud key**, **No universe yet** | See [Add a universe](/docs/project-settings/add-a-universe). |
| **No place is mapped to** a group | See [Set up place groups](/docs/project-settings/place-groups). |
| **Scheduled work is not running** | Tell Craftsman Staff. Delayed operations and auto-deploys wait until it is fixed. |

**Some checks could not run** means a read failed or you lack access. Do not take that as healthy.

### Read the project map

The canvas draws your universes, place groups and places. **Live** means nothing to report. Other group states:

| Group state | Meaning |
|---|---|
| **Brakes on** | A feature is off, or a fallback is on. |
| **Not mapped** | No place is mapped, so nothing reaches a game. |
| **Not published** | No manifest is active here. |
| **Updating** | Some servers are on an older place version. |
| **Drifted** | Its servers run a manifest that is not the active one. |
| **Not checked** | Something could not be read. |

### Read the numbers

- **Servers** counts every server type, from servers that write to the registry.
- **Public servers** appears instead when the registry lists nothing. It counts public servers only.
- A `+` after a number means it is a minimum.

See [Empty server list](/docs/troubleshooting/empty-server-list) if the count looks wrong.

### Read recent activity

**Recent activity** lists the last six actions. For the rest, see [Search the audit log](/docs/monitoring/search-the-audit-log).

Anything you lack permission to read shows as a gap, not an error.

---

## Page: /docs/monitoring/check-health

**Check key permissions and rate limits.** See what your Open Cloud key can do and how much of each rate limit is left.

Open **Health** to see what Roblox lets your key do and how close you are to its limits. You need `craftsman.settings.read`.

### Check your key

1. Open **Health**.
2. If your project has several universes, pick one. Each has its own key.
3. Press **Check now**.
4. Press **Show all N permissions** to see each one.

With no key, press **Add a key** instead. See [Add a universe](/docs/project-settings/add-a-universe).

**Check now** never probes Luau Execution, restriction writes or place publishing, because the only call that proves them acts. Their status comes from real use.

### Read the permissions

| Status | Meaning |
|---|---|
| **Proven** | A call or a check succeeded. |
| **Missing** | Roblox refused the scope. |
| **Key rejected** | Roblox refused the key itself, so every call with it fails. |
| **No longer needed** | The key can drop this scope. |
| **Not proven** | The last check could not prove it. |
| **Not checked** | Nothing has used or checked it. |

The newer result wins. A fixed scope reads **Proven** as soon as anything proves it.

### Fix a refusal

**Needs attention** appears only when Roblox refused something. Each row names what stops working and the scopes it needs. Red means the feature stopped. The others still work in a lesser form.

1. Add the scope to your key on the Creator Hub.
2. Press **Open Universes** on the row, and [replace the key](/docs/project-settings/rotate-keys) if you made a new one.
3. Press **Check now**.

See [Missing key scope](/docs/troubleshooting/missing-key-scope).

### Read the rate limits

**Rate limits** has a card for each scope Roblox has reported on. It shows calls left out of what the tightest window grants. **None reported yet** means no call has returned a limit.

---

## Page: /docs/monitoring/search-the-audit-log

**Search the audit log.** Find who changed what, when and from where, across the console, Discord and your games.

Open **Audit** to find who changed what. It records every change from the console, Discord and your games. You need `craftsman.audit.read`.

### Narrow the list

Use the filters above the list. They apply together.

| Filter | Choose |
|---|---|
| **Who** | One person, or **Anyone**. |
| **What** | One kind of action, or **Anything**. |
| **From** | **The dashboard**, **Discord**, **The game**, **GitHub** or **Not recorded**. |
| **Back to** | A date. The list starts at the end of that day and goes back. |

Press **Clear** to remove every filter.

**Who** and **What** offer only what the log holds. If you see **Options come from recent activity only**, an older person or action may not be listed.

### Read an entry

Entries are grouped by day, newest first. Each one says who did what, to what, and from where.

Click an entry to open it. You see the exact action key, the person's Discord UID or GitHub ID, and whatever else was recorded.

### See older entries

Press **Show older** at the bottom. The count beside it shows how many are loaded.

New entries appear by themselves when you come back to the tab, and every 30 seconds while you are on it.

**Nothing matches** means your filters found nothing. Widen or clear them.

---

## Page: /docs/monitoring/notifications

**Read your notifications.** Use the bell to see what is waiting on you and what is happened, across all your projects.

Press the bell in the top bar. It is on every console page, and covers all your projects.

### Read the panel

**Waiting on you** comes first. It lists requests you can approve or confirm now. Press one to open it in [Approvals](/docs/team-and-access/approve-a-request).

**Notifications** follow, newest first. You will see:

- a request of yours that was approved or settled
- an operation of yours that settled
- an invitation to a project
- a member with no role asking for one
- a new CLI session on your account
- a linked Roblox account removed by an erasure request

Press one to go where it is dealt with. Opening it marks it read. **Mark all read** does them all.

**Dealt with** means someone else already handled it.

### Read the badge

| Badge | Meaning |
|---|---|
| Nothing | Nothing new. |
| `3` | Unread notifications plus requests waiting on you. |
| `99+` | More than 99. |
| `3+` | At least 3. Part of it could not be read. |
| `!` | It could not be read, so nothing was counted. |

A request waiting on you counts once. It stops counting when someone else deals with it, even if you never opened it.

### Check it is up to date

The panel's footer says how updates reach you:

- **Live**: they arrive as they happen.
- **Updates when you come back to this tab**: the live connection is down.
- **Connecting…**: it is still starting.

Next: [Approve a request](/docs/team-and-access/approve-a-request)

---

# Collection: Privacy

Handle right-to-erasure requests.

---

## Page: /docs/privacy/connect-the-webhook

**Connect the Roblox webhook.** Point Roblox's webhook at your project so erasure requests and refunds reach the console.

You will give Creator Hub a URL and a secret, then send a test. You need `craftsman.erasure.manage`. On a group game, only the group's owner can change webhooks in Creator Hub.

### Connect it

1. Open **Settings** > **Roblox webhook**.
2. Press **Generate secret** and confirm with your passkey.
3. Copy the secret. It is shown once.
4. Follow **Open webhooks for** your Production universe. Creator Hub opens on the right page.
5. Paste the **Webhook URL** and the secret. Turn on **Include Secret**.
6. Tick the events highlighted on the **Events** row: **Right to Erasure Request**, **Transaction Refunded** and **Commerce Product Order Refunded**.
7. Send a test from Creator Hub.

The badge turns **Connected** without a refresh. Press **Done** to clear the secret from the screen.

No link in step 4 means the project has no universe yet. [Add one](/docs/project-settings/add-a-universe) first.

### Read the badge

| Badge | Meaning |
| --- | --- |
| **Not connected** | No secret saved. Deliveries are refused. |
| **Waiting for Roblox** | Secret saved, but nothing has arrived signed with it. |
| **Replacing** | A new secret is waiting for Roblox. The old one still works. |
| **Connected** | A delivery arrived signed with the current secret. |

A saved secret is not enough. Only a real delivery makes it **Connected**.

### Replace the secret

1. Press **Replace secret**.
2. Tick **It may have leaked. Stop accepting it now.** if that is why you are replacing it.
3. Press **Generate new secret** and confirm with your passkey.
4. Paste it into Creator Hub and send a test.

The old secret works until Roblox uses the new one, or for 7 days. If you ticked the box, deliveries are refused until Creator Hub has the new one.

### Disconnect

Press **Disconnect**, then **Confirm disconnect**, and confirm with your passkey. Deliveries are refused and Roblox does not resend them. Erasure requests stop arriving.

Next: [Track an erasure request](/docs/privacy/erasure-requests)

---

## Page: /docs/privacy/erasure-requests

**Track an erasure request.** See what a right-to-erasure request will delete, when it runs, and what it did.

When Roblox sends a right-to-erasure request, it appears on **Privacy**. It runs by itself after 7 days unless someone cancels it. You need `craftsman.erasure.read` to see the page.

### Read a request

Press a request for its details. Failed and waiting ones sort first, and also show on **Overview**.

| Status | Meaning |
| --- | --- |
| **Waiting** | It will run at the time shown. |
| **Running** | It is deleting now. |
| **Deleted once** | It deletes again a day later, in case a server wrote the key back. It still holds the UserId. |
| **Erased** | Every store is clear. It no longer names the player. |
| **Failed** | Something was not cleared. It will not retry by itself. |
| **Cancelled** | It will not run. |

### Know what is deleted

It deletes the player's key from every data store a manifest declares as keyed by player, in every place group and universe. That includes stores a manifest stopped declaring. **What an erasure clears** lists them.

A store no manifest declares is not covered. After a run, **Not covered in** names them, so check those by hand. Roblox keeps earlier versions of a deleted key for 30 days.

The Open Cloud key needs `universe-datastores.objects:delete`. Listing undeclared stores needs `universe-datastores.control:list`.

### Know what is kept

Project Control's own records stay, with the UserId replaced. **Renamed to** shows the new name. A run also calls off the player's waiting commands, and unlinks that Roblox account from any member.

### Read each store's outcome

| Outcome | Meaning |
| --- | --- |
| **Deleted** | Roblox removed the key. |
| **Held nothing** | The store had no key for that player. |
| **Refused** | Roblox said no, usually over the delete scope. |
| **Not known** | The call failed. |

One **Refused** or **Not known** makes the request **Failed**. Your records stay unchanged, and it keeps the UserId.

### Check a player

Enter their UserId under **Was a player erased here?** and press **Check**.

### Open a request yourself

1. Enter the UserId under **Open a request by hand**.
2. Press **Open request** and confirm with your passkey.

It waits 7 days too. If that player already has one open, you get that one.

Next: [Cancel or retry an erasure](/docs/privacy/cancel-or-retry-erasure)

---

## Page: /docs/privacy/cancel-or-retry-erasure

**Cancel or retry an erasure.** Stop a waiting erasure, run it early, or run a failed one again.

You will do all of this from **Privacy**. Each action needs `craftsman.erasure.manage` and your passkey.

### Cancel a waiting request

1. Press **Cancel** on the request.
2. Type a reason of at least 4 characters. It is recorded in the audit under your name.
3. Press **Cancel this request** and confirm with your passkey.

**Keep it** closes the box without cancelling. A cancelled request never runs. If the player's request still stands, [open a new one](/docs/privacy/erasure-requests#open-a-request-yourself). Only a waiting request can be cancelled. A running, failed or finished one cannot.

### Run a waiting request now

Press **Erase now** and confirm with your passkey. The deletion starts right away.

### Run a failed request again

A failed request never retries by itself. Its sentence names what went wrong.

1. Fix the cause. Usually that is a key without `universe-datastores.objects:delete`, or a universe with no Open Cloud key. See [Rotate keys and secrets](/docs/project-settings/rotate-keys).
2. Press **Run again** and confirm with your passkey.

It is safe to repeat. A store that is already clear answers **Held nothing**.

---

# Collection: Project settings

Universes, place groups and keys.

---

## Page: /docs/project-settings/add-a-universe

**Add a universe.** Add a Roblox universe to your project, store its game key, and give it an Open Cloud key.

You will add the universe, store its game key in Studio, then paste an Open Cloud key. You need `craftsman.settings.secret`.

### Add it

1. Open **Settings** > **Universes** and press **Add universe**.
2. Enter the universe ID from Creator Hub. A place ID will not work.
3. Enter a name, such as `Live`.
4. Press **Add** and confirm with your passkey.

The first universe you add holds every place group. A universe can belong to one project only.

### Store the game key

The game key appears once, right after you add the universe.

1. In Studio, open **Game Settings** > **Security** > **Secrets**.
2. Add the key as `CRAFTSMAN_CONTROL`.
3. Set its domain to `operations.craftsman.systems`, as the console shows. Any other domain fails silently.
4. Press **Done** in the console.

Local playtests cannot read secrets. Test from Team Test or a live server, or add it to Local Secrets.

### Paste the Open Cloud key

1. Under the universe's **Manage**, open **Permissions this key needs**.
2. Create the key on Creator Hub with each **Required** permission. Put every universe of this project under each system.
3. Add any **Optional** permissions for the features you will use.
4. Paste the key under **Open Cloud key**.
5. Press **Save & test** and confirm with your passkey.

These are required:

- `universe-datastores.control:create`
- `universe-datastores.objects:create`
- `universe-datastores.objects:update`
- `universe-datastores.objects:read`
- `universe-messaging-service:publish`
- `universe.place.luau-execution-session:write`
- `asset:read`

**Also save to** is ticked by default. It puts the same key on your other universes that have none, and checks each one on its own.

The test covers data store entries and messaging. It does not check the other permissions. [Check Health](/docs/monitoring/check-health) shows those.

If something's missing, the report lists it. **Copy missing** copies the list and **Edit the key** opens Creator Hub. See [Fix a missing key scope](/docs/troubleshooting/missing-key-scope).

### Read the badge

| Badge | Meaning |
| --- | --- |
| **No key** | No Open Cloud key saved. |
| **Untested** | Key saved, not tested yet. |
| **Connected** | The last test passed. |
| **Scope issue** | The last test failed. Hover for the reason. |

### Rename or remove it

Change **Name** under **Manage**, then press **Save changes**.

To remove a universe, press **Remove** under **Manage**. This deletes its keys. You cannot remove one while a place group lives in it.

Next: [Manage place groups](/docs/project-settings/place-groups)

---

## Page: /docs/project-settings/separate-universes

**Separate Production, Testing and Development.** Move a place group into a universe of its own.

Move a place group into a universe of its own. You need Superpower.

Production, Testing and Development are built-in place groups. They can share one universe or have three.

### Move a group

1. [Add the new universe](/docs/project-settings/add-a-universe) with its own game key and Open Cloud key.
2. Under **Settings** > **Place groups**, press **Unmap** on each of the group's places, then **Confirm unmap**.
3. Under **Settings** > **Universes**, find the **Place groups** list. It appears once you have two universes.
4. Choose the new universe in the group's dropdown.
5. Press **Save changes** and confirm with your passkey.
6. [Map the new universe's places](/docs/project-settings/place-groups#map-a-place) to the group.

Each universe's badge shows the groups' roles it holds, for example **Production**.

### Declare every universe

List each universe in your declaration, so the same code runs in all of them:

```lua
return Control.Declare({
	Universe = { 10202097921, 10202097922 },
})
```

A publish uses the first universe in the list. Pass `--universe` to use another:

```bash
craftsman publish --universe 10202097922 --apply
```

A publish builds nothing for a group in another universe. The terminal says so.

### What follows the Production group

- The project's **Universe ID** across the console.
- The [Roblox webhook](/docs/privacy/connect-the-webhook), which you set on the Production universe.
- **Health**.

If you move the Production group, set the webhook up on its new universe.

### Share one key

One Open Cloud key can serve several universes. Each universe must be on the key's experience list in Creator Hub, under every system. **Test every key** checks each universe on its own.

Each universe always has its own game key.

---

## Page: /docs/project-settings/place-groups

**Manage place groups.** Create place groups and map your Roblox places to them.

A place group is a deployment environment. You will create groups and map places to them under **Settings** > **Place groups**. You need Superpower on the project.

Configuration reaches no server until a place is mapped.

### Map a place

1. Under **Map a place**, paste the **Place ID or URL**.
2. Enter a **Name**, such as `Lobby`.
3. Enter a **Role**, or leave it empty for `main`.
4. Choose the **Place group**.
5. Press **Map place** and confirm with your passkey.

The place must be in the group's universe, or the console refuses it. A place can belong to one project only.

### Use roles

A role says which place this is across groups, such as `main` or `lobby`. It matches `[places.<role>]` in [`craftsman.toml`](/docs/project-setup/craftsman-toml).

- A role is up to 32 lowercase letters, digits and hyphens, starting with a letter.
- A group holds one place per role.
- Press a role in the table to change it.

The default group is marked **Default**, and its places are marked **Studio source**. Every group's copy of a role is built from that place's Studio save.

The card warns you about two gaps:

- A group has no place for a role, so builds skip it.
- A group maps a role the default group does not have, so there is nothing to build it from.

### Unmap a place

Press **Unmap**, then **Confirm unmap**. The place's DataStore documents stay where they are. Remove them by hand if the place should not read them.

### Create a group

1. Under **Create place group**, enter a name of up to 50 characters.
2. Press **Add Group** and confirm with your passkey.

A new group starts in the Development universe, protected, and declares nothing. Activate something in it:

```bash
craftsman publish --group Staging --activate --apply
```

Set how it deploys under **Settings** > **Releases**. See [Choose protected or open](/docs/releases/protected-or-open).

### Delete a group

Press the trash icon, then **Confirm delete**. Unmap its places first. Production, Testing and Development show **Built-in** and cannot be deleted or renamed.

Next: [Separate Production, Testing and Development](/docs/project-settings/separate-universes)

---

## Page: /docs/project-settings/rotate-keys

**Rotate keys and secrets.** Replace a universe's game key or Open Cloud key, and the webhook secret.

Each universe holds its own keys, under **Settings** > **Universes** > **Manage**. You need `craftsman.settings.secret`, and each change asks for your passkey.

### Rotate the game key

The old key stops working at once. Your servers fail verification until you have updated the secret.

1. Press **Rotate game key**, then **Rotate**. If the universe has none, the button says **Create game key**.
2. Confirm with your passkey.
3. Copy the key. It is shown once.
4. In Studio, replace `CRAFTSMAN_CONTROL` under **Game Settings** > **Security** > **Secrets**.
5. Press **Done**.

### Replace the Open Cloud key

1. Create a new key on Creator Hub. **Permissions this key needs** lists what it takes.
2. Paste it under **Open Cloud key**. The field says **Paste to replace**.
3. Press **Save & test** and confirm with your passkey.

The key is saved first and tested second. A key that fails its test still replaces the old one, so do not delete the old one on Creator Hub until the test passes.

To check a saved key, press **Test**. **Test every key** checks each universe on its own.

### Replace other keys

- Download key and asset key: see [Add an asset or download key](/docs/project-settings/asset-and-download-keys).
- Webhook secret: see [Connect the Roblox webhook](/docs/privacy/connect-the-webhook#replace-the-secret). It needs `craftsman.erasure.manage`.

### Pin the signing key

You cannot rotate the signing key. Servers that pin it would refuse a new one. It is one key for the whole project, under **Settings** > **Signing**.

1. Press **Sign now** if step 1 offers it. Finish this before you pin.
2. Copy the line under **Pin the key in your declaration**.
3. Add it beside `Universe` in your declaration.
4. Release and publish.

A pinned server refuses anything this key did not sign. Each group pins it on its own, and the page shows each group's state.

---

## Page: /docs/project-settings/asset-and-download-keys

**Add an asset or download key.** Give a universe separate keys for reading assets and for downloading its Studio place.

Both keys are optional, and both live under **Settings** > **Universes** > **Manage**. Roblox cannot narrow these permissions to one experience, so they can live on keys of their own. You need `craftsman.settings.secret`.

### Add a download key

Every build takes the newest Studio save of your place. The download key is how it fetches that save.

1. Create a key on Creator Hub with only `legacy-asset:manage`.
2. Paste it under **Download key**.
3. Press **Save** and confirm with your passkey.

Roblox grants this permission over every asset the key's owner holds. Keep it off your Open Cloud key.

Without a download key, a build can show **Needs Studio place**. [Upload a Studio place](/docs/releases/upload-a-studio-place) to carry on. Saving a key does not test it.

### Add an asset key

Add one for a group-owned game. Roblox grants `asset:read` and `asset:write` only on a group, so the grant reaches every asset the group holds.

1. Create a key on Creator Hub with `asset:read` and `asset:write`, granted on the group.
2. Paste it under **Asset key**.
3. Press **Save** and confirm with your passkey.

With no asset key, the Open Cloud key is used. On a user-owned game, that key works if it is the owner's own.

The console uses the asset key to check a place's live version, to run deploy's Studio check, to upload `craftsman asset` media and the test model, and to find the newest Studio save.

The universe still needs its Open Cloud key. Saving does not test an asset key. [Check Health](/docs/monitoring/check-health) shows how `asset:read` and `asset:write` last did.

### Remove a key

Press **Remove** beside the key and confirm with your passkey. Without a download key, its places need an uploaded place file to build from. Without an asset key, the Open Cloud key takes over.

---

## Page: /docs/project-settings/general-settings

**Change general settings.** Rename your project and connect it to your Discord server and channels.

You will edit the fields under **Settings** > **General**, then save. You need `craftsman.settings.write`.

### Change a setting

1. Edit the fields you want.
2. Press **Save changes** in the bar at the bottom. **Review changes** lists them first, and **Discard** drops them.
3. Confirm with your passkey.

### The fields

| Field | What it does |
| --- | --- |
| **Project Name** | The name shown across the dashboard. Required. |
| **Project ID** | Your project's ID in the API and URLs. Read-only. Press **Copy** to copy it. |
| **Discord Server ID** | Your team's Discord server. Required. |
| **Discord Audit Channel ID** | Mirrors the audit log to this channel. Leave it blank for no mirror. |
| **Discord Command Channel ID** | Bot commands typed here go to this project. Leave it blank and the bot asks when it is not sure. |

The universe is not here. See [Add a universe](/docs/project-settings/add-a-universe).

### Find a Discord ID

1. In Discord, turn on Developer Mode under **User Settings** > **Advanced**.
2. Right-click the server's icon and choose **Copy Server ID**, or right-click a channel and choose **Copy Channel ID**.

IDs are numbers of 17 to 20 digits. A `#name` or an invite link will not save.

### Why a Discord save gets refused

- The Craftsman bot is not in that server, or cannot see that channel. Add it, then save again.
- You do not have Manage Server or Administrator in that server. Ask someone who does.
- The channel is not in that server.
- The channel is already another project's command channel. A channel routes to one project.
- You have no Discord account on Project Control. Binding a server needs one.

Next: [Stop things from Discord](/docs/emergencies/stop-from-discord)

---

# Collection: Concepts

The ideas behind Project Control.

---

## Page: /docs/concepts/tiers

**Read an operation's tier.** See what routine, valued and structural operations ask of you before they run.

Every operation has a tier. It is shown on the operation's row, and it decides what happens when you press the button.

| Tier | Button | What it asks |
|---|---|---|
| `routine` | **Run in** *group* | One click. A passkey too, if its permission asks for one. |
| `valued` | **Run in** *group* | The same, and what it moves counts against your daily allowance. |
| `structural` | **Request in** *group* | A passkey and a 15-minute delay. A second approver too, if the operation asks for one. |

A passkey tap covers ten minutes of your browser session. See [Tap a passkey once](/docs/concepts/passkey-window).

### Valued

Your allowance comes from your role. It covers what an operation grants or transfers.

- Go over it and the operation is refused. The message says how much you have left.
- The operation that takes you past half of it asks for a passkey.
- An operation that moves no value is never charged.

### Structural

A structural operation does not run when you press the button.

1. Press **Request in** *group* and confirm with your passkey.
2. It appears on **Approvals**.
3. If it needs an approver, someone else with `craftsman.operations.approve` presses **Approve** from their own sign-in, and confirms with their passkey.
4. It runs once the 15 minutes are up. Approving does not skip them.

If nobody else on the project can approve, it runs alone after the 15 minutes.

Anyone with `craftsman.operations.cancel` can stop it before it runs. That takes no passkey.

### Set the tier in code

```lua
Kick.NextJoin = Control.Command({ Reason = Reason })
	.Label("Kick on next join")
	.NoImpact()
	.Tier("routine")
	.Factor("passkey")
	.Handles(KickOnJoin)
```

Here is what you get if you do not set one:

- A reducer is `routine`.
- A reducer that grants or transfers value is `valued`.
- A reducer marked `.Irreversible()` is always `structural`.
- A command is `structural`.

`.Factor("passkey")` adds a passkey at any tier. `.Approver(true)` asks a structural operation for a second approver.

Bans are `routine` with a passkey. Field edits and reverts are `structural`, with no delay.

A `structural` command never runs from inside the game.

Next: [Read an operation's status](/docs/concepts/refused-or-failed)

---

## Page: /docs/concepts/passkey-window

**Tap a passkey once.** One passkey tap covers ten minutes of your browser session. See the time left, or lock early.

One passkey tap covers your next actions for ten minutes, in the same browser session. Signing in counts as a tap.

### See the time left

While the window is open, the top bar shows a lock with the minutes left, such as **8 min**. When it is closed, nothing shows.

The ten minutes run from the tap. Using the console does not extend them.

### Lock it early

Press **Lock** in the top bar. Your next action asks for a passkey again.

Do this before you step away from a shared computer.

### What still asks every time

- Adding or removing a passkey
- Linking an account
- Accepting an invitation

Another browser or device has its own window, and starts closed.

### What the window does not change

It covers the tap and nothing else. You still need the permission. A structural operation still waits out its delay and its approver.

The audit record notes when an action was covered by an earlier tap.

Next: [Add a passkey](/docs/team-and-access/add-a-passkey)

---

## Page: /docs/concepts/refused-or-failed

**Read an operation's status.** Refused means it did not happen. Failed means nobody knows. Here is every status and what to do next.

The console tells you what happened after you press **Run**. **Refused** and **failed** mean very different things.

| Status | What it means | What to do |
|---|---|---|
| `applied` | It happened. | Nothing. |
| `refused` | It did not happen. Someone decided no. | Read the reason. |
| `failed` | We do not know if it happened. | Check first. See below. |
| `queued` or `pending` | Sent to servers, with no answer yet. | Wait up to two minutes. |
| `waiting` | Held for a player who is not in a server. | Nothing. It does not expire. |
| `delayed` | Held by the clock, an approver, or both. | See [Approvals](/docs/team-and-access/approve-a-request). |
| `cancelled` | Stopped before it ran. | Nothing. |

### Refused

Your game or the console said no, and gave a reason. Nothing changed. Fix the reason and run it again.

### Failed

Your request went out and no clear answer came back. It may have landed.

1. Check the document's timeline, or the server.
2. If it did not land, press **Send as a new operation**.

Pressing **Run** again with the same values sends nothing. It shows you the same operation. **Send as a new operation** really does send again, so if the first one landed, it applies twice.

### Queued

A live command stays queued until a server answers, for up to two minutes. No answer ends as `failed`, not `refused`, because a server may have run it and been unable to tell us. See [Find a command that never arrived](/docs/troubleshooting/command-never-arrived).

### Every server

A command sent to every server is `applied` once the first server runs it. The console keeps counting, for example **Ran on 37 of 40 servers. 1 refused it.**

It is `refused` only if every server that answered refused. If none answered, it is `failed`.

### Waiting

`waiting` is not `pending`. A durable command waits for the player to join, which can take days. See [Choose how a command is delivered](/docs/servers-and-commands/command-delivery).

---

## Page: /docs/concepts/security

**Understand what keeps your game safe.** Passkeys, roles, delays on risky actions, an audit log nobody can rewrite, and signed commands and config.

You can hand Project Control to your whole team. Here is what protects your game.

### Signing in and acting

| Protection | What it means |
|---|---|
| **Passkeys, no passwords** | You sign in with a passkey, and important actions ask for it again. There is no password to phish or leak. |
| **Only what you grant** | Roles decide who can change what, and how far. A new member starts with no access. |
| **Risky actions wait** | Structural actions need a passkey and a delay, and an approver where they ask for one. Routine ones take a click. |
| **Fast activity freezes** | An account acting unusually fast is frozen automatically. |
| **A record nobody can rewrite** | Every change is kept with who made it and which passkey confirmed it. Edits are refused. The one exception: a right-to-erasure run replaces the player's UserId. You can mirror it to a Discord channel. |

### Keys and signatures

| Protection | What it means |
|---|---|
| **Signed, then checked** | A release build carries GitHub's signed token, a webhook carries Roblox's signature, and an important action carries your passkey's. Each is checked before anything happens. |
| **A key no script can read** | Your servers reach us with a key Roblox's secret store adds as the request leaves. No script in your game holds it. |
| **Keys stay on our servers** | Once you save your Roblox keys, they are used only on our servers, never in a browser or your repository. |
| **Commands that can't be spoofed** | Pin your signing key and servers refuse anything we did not sign. A forged or expired message is refused, and so is config for a place it does not list. |

**Settings** > **Signing** gives you the key to pin in your declaration. Sign first, as the page shows, or pinned servers refuse what they read. Until you pin, your servers do not check signatures.

Next: [Read an operation's tier](/docs/concepts/tiers)

---

## Page: /docs/concepts/how-its-built

**Understand how it is built.** Declared in your code, with your data kept in Roblox, servers that run without us, and Production kept separate.

Project Control stays out of your way. Four things make that true.

### Declared in your code

You write what the console can see and change in Luau, in your game's repository. `craftsman publish` sends it to the console. See [The declaration](/docs/getting-started/the-declaration).

### Your data stays in Roblox

Player saves stay in your universe. When you open one in the console, it is read from Roblox at that moment. No copy is kept.

### Your game runs without us

Your game's config lives in your own DataStore. If we are down, your servers keep running. If they cannot read their config, they use the values in your code.

A quiet server sends us nothing.

### Production stays separate

Development, Testing and Production each keep their own definitions. Trying something in Development does not touch your live servers.

Next: [Understand what keeps your game safe](/docs/concepts/security)

---

# Collection: Troubleshooting

When something does not work.

---

## Page: /docs/troubleshooting/empty-server-list

**Fix an empty server list.** The Servers page lists nothing. Find out whether no servers are running or the game is not reporting.

The **Servers** page lists nothing. An empty list has more than one cause, and the page says which it can tell.

### This game is not writing to the server registry

Roblox lists public servers, but none reports to Project Control.

Turn the registry on, then deploy a release:

```lua
Control.Start(declaration, { Registry = true })
```

Servers already running keep their old build. They will not appear until they are replaced.

The Open Cloud key also needs `memory-store.sorted-map:read` and `memory-store.sorted-map:write`. See [Add a missing key scope](/docs/troubleshooting/missing-key-scope).

### No servers are running

Roblox's public list agrees there are none. Join the game and look again.

That list misses reserved and private servers. If you know one is running, treat this as the case above.

### The server registry is empty

The page cannot read Roblox's public count, so it cannot tell the two apart. Check both.

Next: [See live servers](/docs/servers-and-commands/see-live-servers)

---

## Page: /docs/troubleshooting/empty-store

**Fix an empty store.** Players & Data lists no keys for an entity. Check the store exists and has the name your game writes.

An entity in **Players & Data** lists no keys, or says `No data store named "..." exists in this universe`.

### Nothing has written to it yet

Roblox creates a data store on its first write. A place nobody has played has none, and a mock store in a test does not create one.

Join the game on a live server and write something. Then look again.

### The store name does not match

The console lists the store named in your declaration. If your game writes somewhere else, the console shows an empty store with no error.

```lua
Store = { Name = "PlayerData" },
```

Set `Name` to the store your game really writes, then publish again. Leave `Scope` out, or set it to `"global"`.

### You are looking at another place group

Each place group has its own declaration, and may live in its own universe. Pick the group whose servers you played on.

### You left a filter on

Clear **Filter prefix**.

See [Declare an entity](/docs/players-and-data/entities). If the console says the place has no store, see [Open stores and write data](/docs/players-and-data/open-stores).

---

## Page: /docs/troubleshooting/command-never-arrived

**Find a command that never arrived.** A command ended failed, or stayed waiting, and nothing happened in the game. Here is where to look.

You ran a command and nothing happened in the game. Look at its status first.

### It failed straight away

The command could not be sent. The key probably lacks `universe-messaging-service:publish`. See [Add a missing key scope](/docs/troubleshooting/missing-key-scope).

### It failed after two minutes

No server answered. Check these in order:

- **The player wasn't in the game.** A live command runs in the current session or not at all. To reach them later, make it [durable](/docs/servers-and-commands/command-delivery).
- **The running build has no handler.** A server ignores a command it has no handler for, and says nothing. Deploy a release with the handler.
- **The server ran it and couldn't tell us.** The `CRAFTSMAN_CONTROL` secret's domain must be the console's domain. With any other domain, every report is lost. See [Connect your universe](/docs/getting-started/connect-your-universe).

Check the game before you send it again. It may have run.

### It was refused

Read the reason. A refusal means nothing was sent, or your game said no.

- **Too large:** keep params to about 1 KB.
- **Commands already awaiting a server:** 32 are outstanding, which usually means no server is reading them. Queued commands expire after two minutes.

### It is still waiting

That is a durable command, and the player has not joined yet. If they have joined, check `Control.Start` has `FetchCommands = Control.Reporter.Commands`. Without it, a joining player's commands are never collected.

### A server command reaches nothing

Per-server commands reach only servers started with `Registry = true`. See [Fix an empty server list](/docs/troubleshooting/empty-server-list).

See also: [Read an operation's status](/docs/concepts/refused-or-failed)

---

## Page: /docs/troubleshooting/missing-key-scope

**Add a missing key scope.** A feature stopped because the Open Cloud key lacks a permission. Find it, add it and check it.

A feature fails with a permission error, or **Health** shows a permission as **Missing**. The universe's Open Cloud key does not have that scope.

### Add it

1. Open **Health**. **Needs attention** lists each affected feature, what it cannot do, and the scopes it needs.
2. If your project has several universes, pick the one you are fixing.
3. In the [Creator Hub](https://create.roblox.com/dashboard/credentials?activeTab=ApiKeysTab), edit that key and add the scope.
4. Check the universe is on the experience list for that scope's system.
5. Back in **Health**, press **Check now**.

Each system keeps its own experience list. Adding the universe under Data Stores does not add it under Messaging Service.

### Read the status

| Status | Meaning |
|---|---|
| **Proven** | It worked. |
| **Missing** | Roblox refused it. Add the scope. |
| **Key rejected** | Roblox refused the whole key. Replace it. See [Rotate keys](/docs/project-settings/rotate-keys). |
| **Not checked** | Nothing has tried it yet. This is not a problem by itself. |

**Check now** cannot test some scopes without acting. These stay unproven until a feature really uses them, such as a ban, a deploy or a Luau session.

### Group-owned games

Roblox grants `asset:read` and `asset:write` only on a group. A user-owned game's own key reaches them with no grant. On a group-owned game, grant them on the group, or use a separate key. See [Asset and download keys](/docs/project-settings/asset-and-download-keys).

Next: [Check health](/docs/monitoring/check-health)

---

## Page: /docs/troubleshooting/server-is-busy

**Get past Server is busy.** A deploy is refused because someone has the place open in Team Create. Clear the session and deploy again.

A deploy says someone has the place open in Team Create, or Roblox answers "Server is busy".

Roblox takes no place upload while anyone is in a Team Create session. You cannot override this.

### Fix it

1. Read the release page. It names who is in the session.
2. Ask them to close Studio. That includes you.
3. Wait a few seconds. The lock outlasts the last person leaving.
4. Press **Deploy** again.

### "Could not check who has this place open"

The key lacks `legacy-team-collaboration:manage`, so the console cannot see the session. Deploy still goes ahead, and Roblox refuses if someone's in. Add the scope to get names. See [Add a missing key scope](/docs/troubleshooting/missing-key-scope).

Next: [Team Create](/docs/releases/team-create)

---

## Page: /docs/troubleshooting/fell-back-to-defaults

**Stop running on code values.** Servers ignore your hosted values. Find out whether a fallback is on or config cannot be read.

Your servers run the values in your code and ignore what you set in the console. The **Overview** usually says why, under its attention rows.

### Fallback on across the project

Someone turned on **Use Fallback**. It never expires, and your hosted values are kept. The banner names who set it and when.

Press **Stop fallback** and confirm with your passkey. For one group, feature or config, see [Use a fallback](/docs/emergencies/use-a-fallback).

### A server fell back to in-code fallbacks

A server could not read its config.

1. Press **Open Health**.
2. Fix anything missing under **Configuration**. See [Add a missing key scope](/docs/troubleshooting/missing-key-scope).
3. Check the place is mapped under **Settings** > **Place groups**. A group with no place publishes nothing.

### Signed documents have expired

New servers in that group refuse the expired config and use code values.

1. Press **Open Signing**. It goes to **Settings** > **Signing**.
2. Read the renewal problem, if it shows one. A bad key is the usual cause.
3. Press **Sign now**.

### One config runs its code value

A hosted value that does not fit the config's type on this build falls back on its own. Other configs are fine. Set a value that fits, or deploy the build that expects it. See [Change a config](/docs/features/change-a-config).

### A feature started before config arrived

A feature using `WaitForDocument` starts on its code values once `DocumentTimeout` passes, and warns naming itself. It takes hosted state if it arrives later.

---

## Page: /docs/troubleshooting/locked-out

**Locked out of the console.** What each screen that stops you means, and what to do.

A card over the page means the console is stopping you. Your games keep running.

| Screen | Means | Do |
| --- | --- | --- |
| **You are suspended on this project** | Your access here is paused. | Ask an administrator or Craftsman Staff. |
| **Your access has been revoked** | Your account is frozen. | Ask Craftsman Staff. |
| **You have no access here yet** | You are on the project and hold no role. | Press **Ask for access**. |
| **That is not a project** | The address is wrong, or you are not on it. | Press **Back to your projects**. |
| **Back soon** | The console is down for maintenance. | Press **Try again** later. |
| **We updated our Terms** | You must agree to carry on. | Read them, then press **Agree**. |
| **Sign in to carry on** | Your session ended. | Sign in with your passkey. The page stays behind the card. |

### Your session

A console session lasts 8 hours. Ten minutes before the end, **Stay signed in** appears.

### Lost your passkey

A lost passkey cannot be recovered. Keep a second one. See [Add a passkey](/docs/team-and-access/add-a-passkey).

---

# Collection: Project setup

The rest of a repository's setup: craftsman.toml, packages and upgrades.

---

## Page: /docs/project-setup/craftsman-toml

**craftsman.toml reference.** Every section of craftsman.toml, what it controls and what it defaults to.

`craftsman.toml` sits at the root of your project and holds its settings. The CLI finds your project by looking for this file, from the current folder upward. Commit it.

### Example

```toml
[craftsman]
version = "0.11.0"

[console]
## endpoint = "https://operations.craftsman.systems/api"

[features]
## folder = "src/Features"

[kit.interface]
mode = "Height"
## reference = [1920, 1080]

[lifecycle]
## hang_warning_seconds = 10

[places.main]
universe = 10463540692
place = 137372027987942
```

### Sections

| Section | What it holds | Default |
|---|---|---|
| `[craftsman]` | `version`: the Craftsman version the project is on. Written by `init` and `update`; don't edit it. | Written for you |
| `[console]` | `endpoint`: the console the CLI talks to. `CONTROL_ENDPOINT` in your shell or `.env` overrides it. | The hosted console |
| `[features]` | `folder`: where your feature folders live. | `src/Features` |
| `[[features.routes]]` | Where each part of a feature folder goes in the DataModel. Replaces the default routes entirely. | The default routes |
| `[kit.*]`, `[lifecycle]` | Settings for Craftsman's packages. | Each setting's default |
| `[places.<role>]` | A place a release builds. | None |
| `[assets]` | `folder`: where the asset index and its files live. `module`: the generated `Assets` module. | `assets`, `src/Control/Assets.luau` |

Each section has its own article:

- [Feature folders and routing](/docs/project-setup/feature-folders) for `[features]` and `[[features.routes]]`.
- [Package settings](/docs/project-setup/package-settings) for `[kit.*]` and `[lifecycle]`.
- [Describe your places](/docs/project-setup/describe-your-places) for `[places.<role>]`.
- [Add animations, sounds, images and video](/docs/project-setup/assets) for `[assets]`.

### Commented lines

When `install`, `init` or `update` finds a setting the file lacks, it adds it as a commented line with its description and default, at the end of its section:

```toml
[kit.interface]
## How the interface scales: "Fit", "Height", "Width" or "Diagonal".
## mode = "Fit"
```

To change a setting, remove the `#` and set the value. A commented line counts as present, so it is never added twice.

### How the CLI edits it

The CLI edits `craftsman.toml` as text. It keeps your comments, order and line endings, and changes only `version` and the lines it adds. If an edit would change what the file means, it writes nothing and lists the settings to add by hand.

### Mistakes it catches

- **A value a setting can't take** stops the command, naming the file, the line and what the setting takes.
- **A key no package knows** is a warning with the line, and a suggestion when a known name is close. It is left out of `Settings.luau`.
- **An unknown key in `[places.<role>]`, `[assets]` or a route** is refused.

`craftsman doctor` also checks that `Settings.luau` matches this file.

### Older files this replaces

`install` moves these into `craftsman.toml` and deletes them:

- `features.json`, into `[features]`.
- `places/<role>.place.json`, into `[places.<role>]`.

`update` moves `src/CraftsmanConfig.luau` into the `[kit.*]` sections when it moves a game off Wally.

---

## Page: /docs/project-setup/packages-and-lock

**Craftsman's packages and craftsman.lock.** What Craftsman installs into your project, how craftsman.lock pins it, and how to install it again.

Craftsman is four packages, each with its own version. The CLI downloads them from the console into two folders in your project and pins them in `craftsman.lock`.

### The packages

| Package | Installs as | Version |
|---|---|---|
| Control | `CraftsmanPackages/CraftsmanControl` | Always the CLI's own |
| Craftsman Kit | `CraftsmanPackages/Craftsman` | Its own |
| Lifecycle | `CraftsmanPackages/Lifecycle` | Its own |
| deps | The third-party code the three share, such as Keeper, Promise, Signal, ByteNet, Ledger and Konsole. Jest goes in `CraftsmanDevPackages`. | Its own |

Both folders are gitignored and mapped into `ReplicatedStorage`. Require Craftsman from there:

```lua
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
local Ledger = require("@game/ReplicatedStorage/CraftsmanPackages/Ledger")
```

Wally never installs any of them. If your game keeps Wally for packages of its own, they stay in `Packages/` and you run `wally install` for them as usual. Don't add a package Craftsman already carries to `wally.toml`: two copies of Ledger can open the same key twice, and `craftsman doctor` fails on it.

### craftsman.lock

The lock names each package's version and hashes. Commit it, and never edit it by hand.

```toml
## Written by craftsman install. Commit it, and never edit it by hand.
format = 2
shippable = true

[control]
version = "0.11.0"
sha256 = "..."
tree = "..."
```

It always has all four sections: `control`, `deps`, `kit` and `lifecycle`.

### Install what the lock names

After you clone the project, or pull a change to `craftsman.lock`:

```bash
craftsman install
```

It installs exactly what the lock names, or the newest set for this CLI when there is no lock. Every file is checked against the lock before anything is written, and a package already downloaded is reused without asking the console.

`install` also:

- Writes `CraftsmanPackages/Settings.luau` from `craftsman.toml`.
- Adds any setting `craftsman.toml` lacks, as a commented line.
- Moves an old `features.json` or `places/<role>.place.json` into `craftsman.toml`.
- Routes your features and writes `sourcemap.json`.

> [!WARNING]
> `install` replaces both package folders wholesale. An edit you made inside `CraftsmanPackages/` is discarded. The CLI names the first one it discards, but doesn't keep it.

### In CI: --frozen

```bash
craftsman install --frozen
```

`--frozen` refuses to run without a lock, refuses any set the lock doesn't name, and writes nothing but the packages. The [release workflow](/docs/project-setup/release-workflow) uses it.

### Who can download

Downloading needs `craftsman login`, and you must be on the project. In GitHub Actions, it needs the run's OIDC token (`permissions: id-token: write`) from the repository your project names for releases.

### Checking what is installed

- `craftsman --version` prints the CLI's version and the Control it finds.
- `craftsman doctor` checks that what is installed is what the lock names, and that Control is the CLI's version.

To move to newer packages, see [Update packages and move off Wally](/docs/project-setup/update-packages).

---

## Page: /docs/project-setup/update-packages

**Update packages and move off Wally.** Move Craftsman's packages to newer versions with craftsman update, and move a game that installs Craftsman with Wally onto them.

`craftsman update` moves your project's packages and rewrites `craftsman.lock`. It is also how a game that still installs Craftsman with Wally moves onto Craftsman's packages.

### Update the packages

| Command | What moves |
|---|---|
| `craftsman update` | Every package, to the newest set that fits together. |
| `craftsman update kit` | The kit, to its newest. The others stay where they are while they still fit. |
| `craftsman update kit@0.11.0` | The kit, to exactly that version, even an older one. |

You can name `kit`, `lifecycle` or `deps`. `update` prints each package that moved, for example `moved    kit 0.11.0 -> 0.12.0`. Commit `craftsman.toml` and `craftsman.lock` afterwards.

Updating changes what your game runs, so test before you release.

### Update Control

Control always matches the CLI's version, so you can't name it. It moves with the CLI:

1. Change the `craftsman` pin in `rokit.toml`, for example to `averyark/craftsman-cli@0.11.7`.
2. Run `rokit install`.
3. Run `craftsman update`.

When a newer Control exists, `update` prints the `rokit.toml` line that would run it. It never edits `rokit.toml` itself.

### From CLI 0.9 or 0.10.0

To move from any older version in one pass, follow [Upgrade from an older version](/docs/project-setup/upgrade-from-older-versions).

A `craftsman.lock` written by CLI 0.10.0 or earlier is in an older format, and `craftsman install` refuses it.

1. Move the `rokit.toml` pin to `0.11.7` and run `rokit install`.
2. Run `craftsman update`.
3. Replace `.github/workflows/release.yml` with the current [release workflow](/docs/project-setup/release-workflow). The old one can't read the new lock.
4. Commit `craftsman.lock`, `craftsman.toml`, `rokit.toml` and the workflow.

### Move a game off Wally

If your `wally.toml` names `averyark/craftsman-control`, `averyark/craftsman-kit` or `averyark/craftsman-lifecycle`, `init`, `install`, `publish` and most other commands refuse the project until it moves. Run:

```bash
craftsman update
```

The move is all or nothing. It plans every change first, and if anything stops it, nothing is written. It:

1. Removes Craftsman's packages, and the third-party ones Craftsman now carries, from `wally.toml`. Your other packages stay.
2. Rewrites requires of `ReplicatedStorage/Packages/<name>` to `ReplicatedStorage/CraftsmanPackages/<name>`.
3. Maps `CraftsmanPackages` and `CraftsmanDevPackages` in `base.project.json`.
4. Moves `features.json` and `src/CraftsmanConfig.luau` into `craftsman.toml`.
5. Installs the packages and regenerates your project files.

It ends with a summary and a **Left for you** list.

> [!WARNING]
> `update` only rewrites string requires. A require that finds a package by instance, such as `ReplicatedStorage.Packages.Ledger` or `:WaitForChild("Packages")`, is listed but not changed, and so is a `.luaurc` alias into a Wally folder. Fix each one it lists, or that code keeps loading the old copy.

Then:

- Commit `craftsman.toml` and `craftsman.lock`.
- Replace a release workflow that installs Craftsman with `wally install`.
- Remove Wally from `rokit.toml` if nothing else uses it.

### Options

| Option | What it does |
|---|---|
| `<package>[@<version>]...` | `kit`, `lifecycle` or `deps`, optionally pinned. Default: every package. |
| `--from <dir>` | Move to a package set on disk instead of the console's. For Craftsman development only. |

---

## Page: /docs/project-setup/feature-folders

**Feature folders and routing.** Keep each feature in one folder and let the CLI write your Rojo project with craftsman serve and craftsman project.

Keep each feature in one folder, `src/Features/<Feature>/`. The CLI routes each part of the folder to its place in the DataModel and writes your Rojo project for you.

### The project files

| File | Who writes it | Commit it? |
|---|---|---|
| `base.project.json` | You. Everything that isn't a feature: services, packages, the `Craftsman` folders. | Yes |
| `default.project.json` | The CLI: `base.project.json` plus your routed features. What `rojo serve` serves. | No, gitignored |
| `release.project.json` | The CLI: `default.project.json` without what only the Studio place owns, such as `Workspace` and `Lighting`. What a release builds. | No, gitignored |

`craftsman install`, `serve` and `project` write the two generated files, and `craftsman release` writes them again before every build, so a build never leaves a feature out.

### Default routes

| In `src/Features/<Feature>/` | Becomes |
|---|---|
| `Shared/` | `ReplicatedStorage/Features/<Feature>` |
| `Server/` | `ServerScriptService/Features/<Feature>` |
| `Client/` | `ReplicatedStorage/Features/Client/<Feature>` |
| `Controller.luau` or `Controller/` | `StarterPlayer/StarterPlayerScripts/Controllers/<Feature>` |
| `Handler.luau` or `Handler/` | `ServerScriptService/Handlers/<Feature>` |
| `Network`, `Definitions`, `Types`, `Lookup` | Inside the feature's `Shared`, or a Folder in its place |

Names match exactly, case included. A file or folder no route takes is a warning, naming the route it differs from only by case. Every route's container exists even while no feature uses it.

### Add a feature

```bash
craftsman feature Shop The in-game shop
```

This writes `src/Features/Shop/Definitions/init.luau`, adds the feature to your declaration's `Includes`, and routes it. Add `--client` for a feature that also runs on clients. See [Features](/docs/declaring-in-code/features) for what goes inside.

### Work in Studio

```bash
craftsman serve
```

`serve` writes the project files and runs `rojo serve`. Connect Studio's Rojo plugin as usual.

- Adding, removing or renaming a feature or a routed file restarts `rojo serve`. Reconnect the plugin when it does.
- Edits inside a feature are Rojo's own and restart nothing.
- Every 3 seconds, `serve` checks that Rojo is serving what is on disk. When Rojo has missed a change, `serve` names the files and restarts it.

`--port <n>` and `--address <host>` pass through to `rojo serve`.

### Regenerate without serving

```bash
craftsman project          # write the project files
craftsman project --check  # write nothing; exit 1 if they're out of date
```

When a feature's files call `:Declare(`, `project` also writes that feature's `Definitions/Generated.luau`. Commit it with the change: `publish` and `release` refuse a stale one.

### Change the routes

`[features]` in `craftsman.toml` changes the folder or the routes:

```toml
[features]
folder = "src/Features"

[[features.routes]]
name = "Server"
to = "ServerScriptService/Features"

[[features.routes]]
name = "Types"
into = "Server"
```

Each route has a `name` and either `to` (a DataModel path written with `/`) or `into` (the route it nests inside). Routes you write replace the default table entirely, so list every route you use.

---

## Page: /docs/project-setup/package-settings

**Package settings.** Tune Craftsman's packages from craftsman.toml, and read the result from the generated Settings.luau.

Craftsman's packages read their settings from one generated file, `CraftsmanPackages/Settings.luau`. You set them in `craftsman.toml`, and the CLI writes the file.

### Change a setting

Each package's settings have their own section: `[kit.<section>]` for the kit and `[lifecycle]` for Lifecycle. Control has none. Every setting is already in your `craftsman.toml` as a commented line with its description and default:

```toml
[kit.interface]
## How the interface scales: "Fit", "Height", "Width" or "Diagonal".
## mode = "Fit"
```

1. Remove the `#` and set the value:

   ```toml
   [kit.interface]
   # How the interface scales: "Fit", "Height", "Width" or "Diagonal".
   mode = "Height"
   ```

2. Run any command that reads the project, such as `craftsman project`. If `craftsman serve` is running, it rewrites the file as soon as you save, and Rojo picks it up.

### Value types

| Type | In `craftsman.toml` | In `Settings.luau` |
|---|---|---|
| Boolean | `true` | `true` |
| Number | `1.25` | `1.25` |
| String | `"rbxassetid://1"` | `"rbxassetid://1"` |
| Choice | `"Fit"`, one of the listed values | `"Fit"` |
| Vector2 | `[1920, 1080]` | `Vector2.new(1920, 1080)` |
| Color3 | `[200, 200, 200]`, whole numbers 0 to 255 | `Color3.fromRGB(200, 200, 200)` |
| List of strings | `["a", "b"]` | `table.freeze({ "a", "b" })` |

A value a setting can't take stops the command, naming the line and what the setting takes.

### The generated file

`Settings.luau` is one frozen table per package, with names upper-cased:

```lua
return table.freeze({
	KIT = table.freeze({
		INTERFACE = table.freeze({
			MODE = "Height",
			REFERENCE = Vector2.new(1920, 1080),
		}),
	}),
	LIFECYCLE = table.freeze({
		HANG_WARNING_SECONDS = 10,
		TOP_LEVEL_MODULES_ONLY = true,
	}),
})
```

It lives inside the gitignored `CraftsmanPackages/`, so don't commit it and don't edit it: the next command overwrites it. `craftsman doctor` fails when it no longer matches `craftsman.toml`.

You can only set what a package declares. A key no package knows is a warning, and is left out of the file.

> [!WARNING]
> `Settings.luau` sits in `ReplicatedStorage` and ships to every client. Never put a secret in `craftsman.toml`: any player can read it.

---

## Page: /docs/project-setup/upgrade-from-older-versions

**Upgrade from an older version.** Move a game from any earlier Craftsman version to the current one, step by step, with a check after each step.

This guide moves a game from any earlier Craftsman to the current one. Find your starting point, do your steps in order on one branch, and release once at the end.

### What you're moving to

| Part | Version |
|---|---|
| `craftsman` CLI | 0.11.7 |
| Control | 0.11.7 (always the CLI's version) |
| Craftsman Kit | 0.10.1 |
| Lifecycle | 2.0.0 |
| deps | 1.0.0 |

The kit's numbers restarted when it left Wally: registry kit 0.10.1 is newer than Wally kit 0.11.0.

### Find your starting point

| Your repository has | You're on | Do steps |
|---|---|---|
| `CONTROL_SIGNING_SECRET` in `.env`, or `rokit.toml` pins `craftsman-cli@0.1` or `@0.2` | CLI 0.1 to 0.2 | All |
| `wally.toml` names `averyark/craftsman-control@0.1.x`, and code uses `Control.Flag` or `FlagService` | Control 0.1 | All |
| `wally.toml` names `averyark/craftsman-control@0.2.0` or `@0.3.0` | Control 0.2 to 0.3, CLI 0.7 to 0.8 | All but 6 |
| `craftsman.lock` with `version = "0.9.0"` and no `format` line | CLI 0.9 | All but 6 and 8 |
| `craftsman.lock` with `version = "0.10.0"` and no `format` line | CLI 0.10.0 | All but 4, 6 and 8 |
| `craftsman.lock` with `format = 2` | CLI 0.10.1 or 0.10.2 | 1, 2, 3, 7, 9, 11, 12 |

> [!CAUTION]
> **If your declaration has `Store =`, or your code uses `Control.Catalogue`, do this now.** Servers on Control 0.10.2 or older no longer get pass and product IDs from Project Control. A running server keeps the IDs it has. A server that starts now sells nothing and grants no product until you release on 0.11.0. Step 9 is the fix.

### Before you start

- Work on one branch, and don't deploy until step 12. Between steps the game may not build.
- Hosted flag values, kill switches and schedules are already gone. There is nothing to export first.
- `craftsman update` and `craftsman project` download Luau's parser from GitHub the first time. Offline, set `CRAFTSMAN_LUAU_AST` to a `luau-ast` you already have.

### 1. Pin CLI 0.11.7 {#step-1}

Control moves with the CLI, so the pin decides both.

```toml
## rokit.toml
[tools]
craftsman = "averyark/craftsman-cli@0.11.7"
rojo = "rojo-rbx/rojo@7.7.1"
```

```bash
rokit install
```

You no longer need `lune` for Craftsman. Keep `wally` and `wally-package-types` only if your game installs packages of its own with Wally.

**Check:** `craftsman --version` prints `craftsman 0.11.7`. Until step 3, its `framework` line says why your project's Control can't load. That's expected.

### 2. Sign in with your passkey {#step-2}

Only CLI 0.11.7 or newer can sign in, which is why the pin comes first. The game key no longer works from a terminal.

1. You need a console account with a passkey, and to be on the project. Craftsman staff add people to a project.
2. Delete `CONTROL_SIGNING_SECRET` from `.env`. Delete `CONTROL_ENDPOINT` too, unless it names the hosted console or this machine.
3. Sign in, and confirm with your passkey in the browser that opens:

   ```bash
   craftsman login
   craftsman whoami
   ```

**Check:** `whoami` lists your project. See [Sign in from the terminal](/docs/getting-started/sign-in-from-the-terminal).

### 3. Move onto Craftsman's packages {#step-3}

Craftsman now installs from Project Control's registry into `CraftsmanPackages/`, pinned by `craftsman.lock`. One command moves you from Wally, from a bundle lock, or from an older set:

```bash
craftsman update
```

From Wally, it is all or nothing. It plans every change first, and writes nothing if any of them can't be made. It:

- Removes Craftsman's packages, and the ones Craftsman now carries (Keeper, Promise, Signal, ByteNet, Ledger, Konsole), from `wally.toml`.
- Rewrites string requires from `ReplicatedStorage/Packages/<name>` to `ReplicatedStorage/CraftsmanPackages/<name>`.
- Maps `CraftsmanPackages` and `CraftsmanDevPackages` in `base.project.json`, and adds both to `.gitignore`.
- Moves `features.json` and `src/CraftsmanConfig.luau` into `craftsman.toml`, and `places/*.place.json` into `[places]`.

```lua
-- Before
local Control = require("@game/ReplicatedStorage/Packages/CraftsmanControl")
-- After
local Control = require("@game/ReplicatedStorage/CraftsmanPackages/CraftsmanControl")
```

```toml
## craftsman.lock, before (CLI 0.9 to 0.10.0)
version = "0.10.0"
sha256 = "..."
tree = "..."

## After
format = 2
shippable = true

[control]
version = "0.11.7"
sha256 = "..."
tree = "..."
## then [deps], [kit] and [lifecycle]
```

Then work through its **Left for you** list:

- Fix each require it listed but didn't change: an instance path such as `ReplicatedStorage.Packages.Ledger` or `:WaitForChild("Packages")`, or a `.luaurc` alias into a Wally folder.
- If `wally.toml` still lists packages of your own, run `wally install` after every `craftsman install`. If it was removed, remove Wally from `rokit.toml`.
- Commit `craftsman.toml`, `craftsman.lock` and everything `update` changed.

**Check:**

- `craftsman --version` ends with `framework control 0.11.7 at CraftsmanPackages/CraftsmanControl`.
- `craftsman doctor --offline` passes **Framework**, **Packages** and **Shared packages**.
- Nothing in `src` requires Craftsman, Ledger, Keeper or Konsole from `Packages`.

More: [Update packages and move off Wally](/docs/project-setup/update-packages).

### 4. Check your places, and stop committing generated projects {#step-4}

Places live in `craftsman.toml` now, and `default.project.json` and `release.project.json` are written by the CLI.

1. Check that `craftsman.toml` has a `[places.<role>]` for each place and `places/` is gone. `craftsman install` moves any file left.

   ```toml
   [places.main]
   universe = 10463540692
   place = 137372027987942
   ```

2. Check that the declaration's `Universe` names every universe in `[places]`.
3. If you have a `base.project.json`, add these to `.gitignore` and untrack them:

   ```text
   /default.project.json
   /release.project.json
   ```

   ```bash
   git rm --cached default.project.json release.project.json
   ```

4. In the console, open **Settings → Place groups → Place mappings**. Each place's role must match a `[places.<role>]`. Places mapped before roles existed were given `main`, or `place-<placeId>` when their group already had one.

**Check:** `craftsman doctor` passes **Places**, and `git ls-files default.project.json release.project.json` prints nothing. See [Describe your places](/docs/project-setup/describe-your-places).

### 5. Replace the release workflow {#step-5}

Older workflows can't read today's `craftsman.lock` or `[places]`.

1. Replace `.github/workflows/release.yml` with `templates/release.yml` from [averyark/craftsman-cli](https://github.com/averyark/craftsman-cli), whole.
2. Check that `rokit.toml` lists `craftsman` and `rojo`, and `wally` only if you have a `wally.toml`.
3. Check that **Settings → Releases** in the console names this repository.

**Check:** step 12's `publish --apply` builds and hands the candidate over. See [The release workflow](/docs/project-setup/release-workflow).

### 6. Replace flags with features and configs {#step-6}

Control 0.2.0 removed flags with no alias or shim. Rewrite each call site.

| Old | New |
|---|---|
| A `bool` flag meaning "this part runs" (`Enabled`) | The feature's switch: `Control.Scope("Shop"):Feature()`, read as `Shop.Active` |
| Any other flag | `Control.Config(gtType, { Default = ... })` inside `:Config({...})` on a feature |
| `Type = "int"`, `Min`, `Max` | `gt.number({ integer = true, range = "[Min, Max]" })` |
| `Type = "enum"`, `Options` | `gt.union(gt.literal("a"), gt.literal("b"))` |
| `KilledValue` | Gone. Switch the feature off, or use Use Fallback in the console |
| `Schedulable` | Gone. Declare the times as configs and compare them in your code |
| `Shop.X.Get()`, `Shop.X.Observe(fn)` | `Shop.X:Get()`, `Shop.X:Observe(fn)`, with a colon |
| `Flags:Get("shop.x")` | The handle from the declaration module. There is no read by key |
| `IsActive()` | `Shop.Active` |
| `Control.Feature()` grouper | `Control.Scope("Name")` |
| `Flags:GetRevision()` | `Runtime.Revision()` |
| Lowercase spec keys (`default`, `permission_key`, `universe`) | PascalCase (`Default`, `PermissionKey`, `Universe`) |
| `/kill <flag>` in Discord | `/feature <key> off` |

```lua
-- Before
local Shop = Control.Scope("Shop"):Flags({
	Enabled = Control.Flag({ Type = "bool", Default = true, KilledValue = false }),
	DiscountPercent = Control.Flag({ Type = "int", Default = 0, Min = 0, Max = 75 }),
})

-- After
local Shop = Control.Scope("Shop"):Feature({ Client = true }):Config({
	DiscountPercent = Control.Config(gt.number({ integer = true, range = "[0, 75]" }), {
		Default = 0,
		Shared = true,
	}),
})
```

Move code that ran when a flag was on into `OnActivate`, and connect through its Keeper so switching off cleans up:

```lua
Shop:OnActivate(function(keeper)
	keeper:Connect(Players.PlayerAdded, GreetShopper)
end)
```

Start the runtime with `Control.Start`, without `SnapshotUrl`:

```lua
-- Before
local Flags = Control.FlagService()
Flags:Start(Definitions, { SnapshotUrl = SNAPSHOT_URL, OnEvent = Control.Reporter.Emit })

-- After
local Runtime: Control.Runtime = Control.Start(Definitions, {
	OnEvent = Control.Reporter.Emit,
	FetchCommands = Control.Reporter.Commands,
})
```

- If any feature is `Client = true`, call `Control.StartClient()` once from a LocalScript, or it never runs on clients.
- A config a client reads must be `Shared = true` on a `Client = true` feature.
- Set any hosted value that mattered again in the console's **Configuration** tab, or make it the `Default`.

**Check:** a search for `Control.Flag`, `:Flags(`, `FlagService`, `KilledValue`, `Schedulable`, `SnapshotUrl`, `Control.Feature(`, `GetWithSource` and `IsActive` finds nothing. See [Declare a feature](/docs/declaring-in-code/features) and [Add configs to a feature](/docs/declaring-in-code/configs).

### 7. Regenerate and check your declarations {#step-7}

The CLI loads declaration modules outside Roblox, and writes `Generated.luau` and the `Assets` module from your files.

1. Keep declaration modules loadable:
   - Name `game`, `workspace` or `task` at the top if you like, but never read them there, and never branch on them (`if workspace:FindFirstChild(...)` is always true under the CLI).
   - No `const` anywhere in a declaration module. Files that only call `:Declare` may use it.
   - Nothing may be named `Declare`: it is reserved on every scope.
2. If a feature's files call `:Declare`, its `Definitions` is a folder whose `init.luau` adopts `require("@self/Generated")`, never `./Generated`. Attach command handlers with `.Handles(fn)`.
3. Run:

   ```bash
   craftsman project
   ```

4. Commit every `Definitions/Generated.luau` it wrote.

**Check:** `craftsman project --check` exits 0, and `craftsman doctor` passes **Generated declarations** and **Stores**. See [Declare configs where they are used](/docs/declaring-in-code/declare-where-used).

### 8. Catch up with the kit and Lifecycle {#step-8}

Only for games that installed the kit and Lifecycle with Wally.

- **`Craftsman.Component` is gone.** Require Lifecycle:

  ```lua
  -- Before
  Craftsman.Component:LoadModulesAsync(ServerScriptService.Services):await()

  -- After
  local Lifecycle = require("@game/ReplicatedStorage/CraftsmanPackages/Lifecycle")
  Lifecycle:LoadModulesAsync(ServerScriptService.Services):await()
  ```

- **`MODULE_LOAD_ORDER` is gone.** Declare `Dependencies` on each module instead.
- **`CraftsmanConfig` is no longer read.** Step 3 moved its values into `[kit.*]` in `craftsman.toml`. If the kit warns that it found one, delete it from the place. `Config.Configure` still works.
- **Lifecycle 2.0.0 doesn't load ModuleScripts inside ModuleScripts.** Move a nested singleton into a Folder, or keep the old behaviour:

  ```toml
  [lifecycle]
  top_level_modules_only = false
  ```

**Check:** play in Studio. Every singleton you expect starts, and nothing warns about `CraftsmanConfig`. See [Package settings](/docs/project-setup/package-settings).

### 9. Move a declared store into the asset index {#step-9}

Skip this if your declaration has no `Store`. On Control 0.11.0 and newer a declaration with `Store` doesn't load: passes, products and subscriptions live in `assets/index.toml` now.

1. Adopt what each universe in `[places]` already sells. Passes and products match by name, subscriptions by their `EXP-` ID:

   ```bash
   craftsman store import --dry-run
   craftsman store import
   ```

2. Check every key matches the old one. Purchases are filed under the key, and a key can never be renamed.
3. Move each product's `Grants`, `Reverse` and `Once` into `Receipts` on `Control.Declare`, and delete `Store`:

   ```lua
   -- Before
   Store = {
   	Products = {
   		golden_pickaxe = Catalogue.Product({
   			Name = "Golden Pickaxe",
   			Price = 25,
   			Grants = Catalogue.Op(Player.Inventory, "GrantItem", { ItemId = "golden_pickaxe", Amount = 1 }),
   			OnRefund = "Propose",
   		}),
   	},
   },

   -- After
   Receipts = {
   	golden_pickaxe = {
   		Grants = Control.Op(Player.Inventory, "GrantItem", { ItemId = "golden_pickaxe", Amount = 1 }),
   	},
   },
   ```

4. Names, prices and icons are in the index now. Set a product's refund policy there: `craftsman store edit golden_pickaxe --on-refund reverse`.
5. `Giftable` is gone. An item is giftable when its `<key>.gift` product exists, made with `craftsman store gift <key>`. `Control.Commerce.PromptGift` is unchanged.
6. With your own Rojo project as `overlay`, map `ReplicatedStorage.Craftsman.Assets` to `src/Control/Assets.luau`. Generated projects map it for you.
7. Run `craftsman project`, then commit `assets/`, `src/Control/Assets.luau` and the declaration.

**Check:** `craftsman store status` exits 0. After you deploy, restart servers in the Creator Hub: a new server prints `[Commerce] Bound` with `ReplicatedStorage.Craftsman.Assets`. See [Sell passes, products and subscriptions](/docs/declaring-in-code/store-catalogue).

### 10. Update scripts, CI and tasks {#step-10}

Commands and exit codes changed under scripts that call the CLI.

| Old | New |
|---|---|
| `publish --group G --apply` to activate | `publish --group G --activate --apply` |
| `publish --group G --no-build --apply` | `publish --group G --activate --no-build --apply` |
| `test-roblox --place` | `test-roblox --save-place-version` |
| `release places/main.place.json` | `release main` |
| `craftsman packages` | `craftsman install`, then `wally install` for your own packages |
| `store plan`, the old `store import` | `store status`, `store import` |
| `tools/build-project.luau`, `serve.ps1`, `build.ps1` | `craftsman serve`, `craftsman project` |
| `tools/packages.luau` | `craftsman install` |
| `scripts/analyze.ps1` | `craftsman check` |

Exit codes are `0` done, `1` a verdict against you, `2` a mistake in the command or its configuration, and `3` couldn't tell. A script that treated `0` as "nothing went wrong" for a watch or a test needs updating.

**Check:** each script runs without `has no option`. See [Exit codes and environment variables](/docs/cli/exit-codes-and-environment).

### 11. Check the console's keys {#step-11}

Newer features need permissions an older key lacks. Open **Settings → Universes** for each universe.

| Key | Add | For |
|---|---|---|
| Open Cloud key | `universe-datastores.control:snapshot` | The data snapshot before a save-data deploy. Without it, that deploy stops. |
| Open Cloud key | `game-pass:read`, `game-pass:write`, `developer-product:read`, `developer-product:write` | `craftsman store`. |
| Asset key | `asset:read`, `asset:write`, on the owning group | A group-owned universe's deploy checks, player data reads, `craftsman test-roblox` and `craftsman asset`. A user-owned universe needs none: its owner's Open Cloud key works. |
| Download key | `legacy-asset:manage` only | Uploading images, and building from your newest Studio save. Without it, each build waits under Releases for you to upload the place. |

**Permissions this key needs** on each universe lists the rest. Press **Save & test** after editing a key.

Optional: copy the public key from **Settings → Signing** into `SigningKey` on `Control.Declare`, so servers refuse anything Project Control didn't sign.

**Check:** **Save & test** reports every scope you need as verified.

### 12. Verify and release {#step-12}

```bash
craftsman doctor
craftsman check
craftsman project --check
craftsman publish
```

The preview lists your features and configs, and prints no `configs were dropped` line. Then push your branch and build:

```bash
git push
craftsman publish --apply
```

Deploy the candidate from the console. A protected place group needs a passing test first.

**Check:** the build is handed over, the deploy succeeds, and `craftsman status` shows the release in each place group with the active manifest matching your declaration.

### Troubleshooting

| You see | Do |
|---|---|
| This console no longer accepts the game key from a terminal; update the CLI and run `craftsman login`. | [Step 1](#step-1) and [step 2](#step-2). |
| `This project's .env sets CONTROL_ENDPOINT to ..., which is neither Project Control (https://operations.craftsman.systems/api) nor this machine.` | Remove it from `.env`, or set it in your own shell. |
| `... installs Craftsman with Wally: its wally.toml names averyark/craftsman-control, and there is no craftsman.toml.` | [Step 3](#step-3). |
| `craftsman.lock names the bundle 0.10.0 (...), and Craftsman now installs each package on its own.` | `craftsman update`, then [step 5](#step-5). |
| `This project's control is 0.10.2, and this CLI is 0.11.7. Control works only with the CLI of its own version.` | `craftsman update`. |
| `[Declarations] The declaration has Store, which is not a key it declares.` | [Step 9](#step-9). |
| `... has "KilledValue", which is not a key it declares`, or `attempt to call a nil value` on `Control.Flag` | [Step 6](#step-6). |
| `... has "default", which is not a key it declares` | Spell the key in PascalCase ([step 6](#step-6)). |
| `[Config] Config "<key>" is not Shared, so its value never reaches a client` | Declare it `Shared = true` on a `Client = true` feature. |
| `... is not what the Declare calls in src/Features generate, so this build would ship an old declaration.` | `craftsman project`, and commit the result. |
| `.../Definitions does not adopt "<name>" from <file>:<line>.` | Adopt `Generated.Scopes` with `@self/Generated` ([step 7](#step-7)). |
| `[Placeholders] workspace.Gravity was used in arithmetic while Lune loaded a declaration module.` | Read the value inside a function, not at the top of a declaration module. |
| `release names a place by its role in craftsman.toml's [places], e.g. craftsman release main, not by a file` | Pass the role ([step 10](#step-10)). |
| `test-roblox has no option --place` | Use `--save-place-version`. |
| **Shared packages** fails in `doctor` | Remove the package from `wally.toml`. Two copies of Ledger can open the same key twice. |
| `Universe ... has no download key, ... so nothing was uploaded. Add one under Settings → Universes.` | [Step 11](#step-11). |
| `Taking a snapshot needs universe-datastores.control:snapshot on this universe, under Data Stores.` | [Step 11](#step-11), then deploy again. |
| `There is no such API route` | The CLI is newer than the console it reaches. Check `CONTROL_ENDPOINT`. |

---

# Collection: CLI reference

Every craftsman command, its options, its exit codes and the environment variables it reads.

---

## Page: /docs/cli/setup-commands

**Setup commands.** Reference for init, install, update, feature, serve and project.

These commands set a project up and keep its packages and Rojo project current. Every command also takes `--help` (or `-h`).

### init

Installs Craftsman's packages and writes what a feature-routed game needs. Never overwrites a file. See [Set up a project with craftsman init](/docs/project-setup/initialize-a-project).

```bash
craftsman init [--place <placeId>] [--universe <id>] [--role <role>] [--from <dir>]
```

| Option | What it does |
|---|---|
| `--place <placeId>` | The start place. Adds `[places.<role>]` to `craftsman.toml` and looks up its universe. |
| `--universe <id>` | The universe the declaration names. Default: looked up from `--place`. |
| `--role <role>` | The place's role. Default `main`. |
| `--from <dir>` | Install a package set from a folder instead of the console. For Craftsman development. |

Needs `craftsman login`, and you must be on a project.

### install

Installs the packages `craftsman.lock` names into `CraftsmanPackages/` and `CraftsmanDevPackages/`, and writes `Settings.luau`. See [Craftsman's packages and craftsman.lock](/docs/project-setup/packages-and-lock).

```bash
craftsman install [--frozen] [--from <dir>]
```

| Option | What it does |
|---|---|
| `--frozen` | Install only what `craftsman.lock` names, and write nothing but the packages. For CI. |
| `--from <dir>` | Install a package set from a folder instead of the console. For Craftsman development. |

Without a lock, it installs the newest set for this CLI. A lock written by CLI 0.10.0 or earlier is refused: run `update`.

### update

Moves the packages, one or all, and moves a game off Wally. See [Update packages and move off Wally](/docs/project-setup/update-packages).

```bash
craftsman update [<package>[@<version>]...] [--from <dir>]
```

| Argument or option | What it does |
|---|---|
| `<package>[@<version>]` | `kit`, `lifecycle` or `deps`, optionally pinned, such as `kit@0.11.0`. Default: every package. |
| `--from <dir>` | Move to a package set from a folder. For Craftsman development. |

Control moves with the CLI and can't be named. A folder with no `craftsman.toml` exits 2 and names `init`.

### feature

Adds a feature: its folder and `Definitions`, its entry in the declaration's `Includes`, and `sourcemap.json`.

```bash
craftsman feature <Name> <description> [--client]
```

| Argument or option | What it does |
|---|---|
| `<Name>` | The feature's name, in PascalCase, such as `Shop`. |
| `<description>` | What it is, as the console shows it. The rest of the line, so it needs no quotes. |
| `--client` | The feature also runs on clients. |

It refuses a name that isn't PascalCase, the name `Craftsman`, a feature that already has `Definitions`, and a name that differs from an existing feature folder only by case. If it can't edit your declaration, it leaves it alone and prints the exact line to add.

### serve

Routes `src/Features` into `default.project.json` and runs `rojo serve`, restarting it when the routing changes or when Rojo misses a change. See [Feature folders and routing](/docs/project-setup/feature-folders).

```bash
craftsman serve [--port <n>] [--address <host>]
```

| Option | What it does |
|---|---|
| `--port <n>` | The port `rojo serve` listens on. Default Rojo's, `34872`. |
| `--address <host>` | The address `rojo serve` listens on. Default `localhost`. |

### project

Routes `src/Features` into `default.project.json` and `release.project.json`, and writes each feature's `Generated.luau` and the `Assets` module from `assets/index.toml`.

```bash
craftsman project [--check]
```

| Option | What it does |
|---|---|
| `--check` | Write nothing. Exit 1 when a file isn't what the features route to, or the `Assets` module isn't what the index generates. For CI. |

---

## Page: /docs/cli/publish-and-release

**Publish and release commands.** Reference for publish, status, release, test-roblox and summary.

These commands check your declaration with the console, build releases, and test them in Roblox.

### publish

Checks the declaration's manifest with the console, and builds a release.

```bash
craftsman publish [--apply] [--group <group> [--activate]] [--universe <id>] [--no-build] [--no-watch] [--watch] [--test | --no-test] [--plain | --live]
```

| Option | What it does |
|---|---|
| `--apply` | Act: start the build, and activate with `--activate`. Without it, nothing is started or changed. |
| `--group <group>` | Build for this place group only, and check the manifest against it. Activates nothing by itself. |
| `--activate` | With `--group` and `--apply`: change that group's live features, configs, operations and permissions now, without a release. |
| `--universe <id>` | Publish for this universe, one the declaration names. Default: its first. |
| `--no-build` | The manifest only. Start no build. |
| `--no-watch` | Start the build and return without watching it. |
| `--watch` | Start nothing. Attach to the newest publish build of your current commit. |
| `--test` | With `--watch`: run the in-Roblox test on the build you attach to. |
| `--no-test` | Don't run the in-Roblox test on the candidate. |
| `--plain`, `--live` | Plain progress lines, or the moving line even where the CLI can't tell it's in a terminal. |

#### Common uses

```bash
craftsman publish                                            # preview: check the manifest, show what a release would carry
craftsman publish --apply                                    # build for every place group, watch it, test it
craftsman publish --group Production --apply                 # build for Production only; activates nothing
craftsman publish --group Production --activate --no-build --apply   # change Production's declarations now, no release
```

#### What to know

- **A publish activates nothing.** A release's declarations go live in a place group when someone deploys it there from the console. Only `--activate` changes a group's live declarations from the terminal.
- **A preview exits 0**, even when it lists problems a real run would stop on. Read it.
- **`--apply` builds what is pushed.** Your branch must be checked out and pushed, and `origin` must be the GitHub repository your project names for releases. Uncommitted changes aren't built.
- **Starting the build uses your GitHub login.** The CLI takes `GH_TOKEN` or `GITHUB_TOKEN` if set, then `gh auth token`, then the login Git uses when you push.
- **A protected place group waits.** `--activate` on one prints a link, and waits while someone confirms with a passkey in the console. Ctrl-C stops the waiting, not the request.
- **A stale asset module is refused.** `publish` and `release` stop when `src/Control/Assets.luau` isn't what `assets/index.toml` generates, or an entry has IDs for only some universes. Run `craftsman project`, `craftsman asset push` or `craftsman store push`, as it says.
- `--activate` without `--group`, and `--group G --no-build --apply` without `--activate`, are refused with exit 2.

### status

Says what is live in each place group, beside this checkout. One read; changes nothing.

```bash
craftsman status [--universe <id>] [--json]
```

| Option | What it does |
|---|---|
| `--universe <id>` | Ask as this universe, one the project holds. Default: the declaration's. |
| `--json` | Print the answer as JSON, for agents and CI. |

For each place group it shows its places, whether its active manifest is your local declaration's, the release last deployed there and whether your checkout is that commit, ahead of it or behind it, and any features switched off, on Use Fallback or with held values.

### release

Builds a release candidate place file and its report on your machine. Sends nothing.

```bash
craftsman release [<role>] [options]
```

| Argument or option | What it does |
|---|---|
| `<role>` | The place to build, a `[places.<role>]`. Default: the only one. |
| `--baseline <place.rbxl>` | Build on this Studio place file. Required for a `"control"` place unless `--data-only`. |
| `--baseline-version <n>` | The Roblox place version `--baseline` was saved as. |
| `--previous <baseline.rbxl>` | The previous Studio place, to report what changed outside your code. |
| `--previous-release <release.json>` | The previous release's report, to compare save data against. |
| `--commit <sha>` | The commit the build is stamped with. |
| `--data-only` | Report save-data changes only. Build no place file. |
| `--out <dir>` | Where to write the files. Default `dist`. |

It writes `<role>.rbxl`, `<role>.release.json` and `<role>.manifest.json`. The release workflow runs it on GitHub.

### test-roblox

Builds this checkout and runs the Jest specs in `verify/` against it on a real Roblox server. No commit, run, release or deploy.

```bash
craftsman test-roblox [<role>] [--save-place-version] [--baseline <place.rbxl> --baseline-version <n>] [--plain | --live]
```

| Argument or option | What it does |
|---|---|
| `<role>` | The place to test. Default: the only one. |
| `--save-place-version` | Save the whole build as a place version and test that, instead of a private test model. |
| `--baseline`, `--baseline-version` | As for `release`. |
| `--plain`, `--live` | As for `publish`. |

By default your code is uploaded as a private test model and no place version is saved. `--save-place-version` works only on a place in an open place group that isn't the default group, and needs the `craftsman.releases.deploy.open` permission. Exit 0 means passed, 1 failed or refused, 3 no verdict.

### summary

Prints the release reports in a folder as Markdown.

```bash
craftsman summary [<dir>]
```

`<dir>` defaults to `dist`. The release workflow uses it for the run's summary page.

---

## Page: /docs/cli/asset-and-store-commands

**Asset and store commands.** Reference for asset and store, which upload media and manage passes, products and subscriptions in the asset index.

These commands write your repository's asset index (`assets/index.toml`) and the module generated from it (`src/Control/Assets.luau`). See [Add animations, sounds, images and video](/docs/project-setup/assets) and [Sell passes, products and subscriptions](/docs/declaring-in-code/store-catalogue).

Both take the same three options:

| Option | What it does |
|---|---|
| `--yes` | Take every question's default, and say yes to every confirmation. The last question's default is **Save as draft**. |
| `--dry-run` | Show what would happen. Write, upload or create nothing. |
| `--plain` | No moving line. Already the case in CI and wherever output isn't a terminal. |

Without a terminal, a command that would ask a question refuses, naming the flag that answers it, unless you pass `--yes`. Run either command with no action, in a terminal, to browse the index by kind and entry.

> [!WARNING]
> Preview with `--dry-run` first. Uploads spend your quota, a video costs Robux, and passes and products are never deleted.

### asset

Adds, uploads and tracks animations, audio, images and videos.

```bash
craftsman asset <add|replace|import|rollback|push|list|status> [<arg>...] [--name <name>] [--yes] [--dry-run] [--plain]
```

| Action | What it does |
|---|---|
| `add [<paths>...]` | Copy files into `assets/`, let you rename them, then upload or save them as drafts. Without paths, it asks for them. |
| `replace <name> [<paths>...]` | Replace an entry's file. Always a new ID; the current one stays live until `push` has uploaded the new file for every owner. |
| `import <id>` | Record an asset Roblox already has, and download its file. `--name` names the entry. Default: its display name. |
| `rollback <name>` | Put a version from the entry's history back. |
| `push [<names>...]` | Upload drafts, every draft by default, and apply renames made in the index. |
| `list` | List the index. |
| `status` | Refresh moderation, and exit 1 on a draft, a changed or missing file, anything not approved, or a stale module. For CI. |

#### Examples

```bash
craftsman asset add ~/Downloads/Combat.rbxm ./sfx
craftsman asset replace Combat/Idle ~/Downloads/Idle.rbxm
craftsman asset import 73231190282617 --name ProbeChime
craftsman asset push --yes                    # upload every draft
craftsman asset status                        # the CI gate
```

### store

Creates, changes and adopts game passes, developer products and subscriptions, in every universe your `[places]` name.

```bash
craftsman store <add|edit|gift|import|rollback|push|list|status> [<key>] [<field flags>] [--yes] [--dry-run] [--plain]
```

| Action | What it does |
|---|---|
| `add` | Make a pass or product, asked field by field. |
| `edit <key>` | Change an item's fields. Only what changed is sent. |
| `gift <key>` | Make `<key>.gift`, the product that gifts `<key>`. |
| `import` | Adopt passes and products Roblox already lists, matched by name, and subscriptions by their ID. |
| `rollback <key>` | Put a version from the item's history back, in every universe. |
| `push` | Create drafts in the universes that lack them, and send what was edited in `index.toml` by hand. |
| `list` | List the catalogue. |
| `status` | Exit 1 on a draft, an edit not yet pushed, a missing icon or a stale module. For CI. |

Each field flag answers one question, so a command can run with no questions at all:

| Flag | On | What it does |
|---|---|---|
| `--kind pass\|product` | `add` | A game pass or a developer product. |
| `--name <name>` | `add`, `edit` | The name players see. |
| `--key <key>` | `add` | The key your game names it by. Default: from the name. |
| `--description <text>` | `add`, `edit` | The description. Default: none. |
| `--price <robux>` | `add`, `edit`, `gift` | The price. A pass takes `none` for no price. A gift defaults to its item's. |
| `--for-sale`, `--not-for-sale` | `add`, `edit`, `gift` | On sale or off. `add` defaults to on sale; a gift follows its item. |
| `--icon <path>` | `add`, `edit` | A PNG or JPEG icon. |
| `--on-refund propose\|reverse` | `add`, `edit`, products | Propose the reversal to a person (the default), or reverse at once. |
| `--gift`, `--no-gift` | `add` | Also make the gift product `<key>.gift`, or not (the default). |

With `--yes`, a question with no default still refuses, naming its flag: the kind, the name, and a product's price.

#### Examples

```bash
craftsman store add
craftsman store add --kind product --name Coins --price 25 --on-refund reverse --yes   # in CI: saved as a draft
craftsman store edit vip --price 249 --yes
craftsman store gift vip --price 10 --not-for-sale --yes
craftsman store import
craftsman store push
```

#### What to know

- **A protected place group waits.** A write in a universe that holds one prints a link and waits for your passkey in the browser. `store status` shows it as waiting, never as a draft.
- **Subscriptions are made in Creator Hub**, then imported by ID.
- **Never rename a key**, here or in `index.toml`. `push`, `edit` and `status` refuse a key renamed by hand. Rename it back and change its name instead.

---

## Page: /docs/cli/quality-commands

**Quality commands.** Reference for check, which type-checks and lints your code, and doctor, which finds the mistakes that fail silently.

`check` and `doctor` read your project and change nothing in it, so they are safe to run anywhere, including CI.

### check

Type-checks and lints your code with luau-lsp, and fails only on what `check-baseline.json` doesn't allow.

```bash
craftsman check [<path>...] [--project <file>] [--update-baseline] [--json] [--explain <rule>]
```

| Argument or option | What it does |
|---|---|
| `<path>...` | The files or folders to check. Default `src`. |
| `--project <file>` | The Rojo project the sourcemap is built from. Default `default.project.json`. |
| `--update-baseline` | Write `check-baseline.json` from this run, allowing everything it found. |
| `--json` | Print the result as JSON, for agents and CI. |
| `--explain <rule>` | Print what one of Craftsman's own rules means, such as `RawAssetId`, and check nothing. |

#### Examples

```bash
craftsman check                                # src, against check-baseline.json
craftsman check src/Client src/Shared
craftsman check --explain RawAssetId
craftsman check --update-baseline              # allow what is there now
craftsman check src test --project test.project.json
```

#### How it works

- **The tools are pinned.** luau-lsp and the Roblox type definitions are downloaded once, at fixed versions, and checked by SHA-256 before use. You don't install luau-lsp yourself.
- **The baseline** lists each diagnostic by file, code and message, with a count and no line number, so moving code doesn't break it. A run fails only when some diagnostic appears more often than the baseline allows. Fewer passes, and is reported so you can lower the baseline.
- **Package folders aren't checked**: `CraftsmanPackages`, `CraftsmanDevPackages`, `Packages`, `DevPackages` and `_Index`.
- **Your `sourcemap.json` is never touched.** `check` builds its own temporary one.
- **Craftsman's own rules only warn**, and never count against the baseline. `RawAssetId` finds an `rbxassetid://` written into your code: the ID belongs in the [asset index](/docs/project-setup/assets), named through `Assets`.

| Exit | Meaning |
|---|---|
| 0 | Nothing new beyond the baseline. |
| 1 | More of some diagnostic than the baseline allows. |
| 2 | A configuration problem: no project file, a path that doesn't exist, an unreadable baseline. |
| 3 | No verdict: the tools couldn't be downloaded, or luau-lsp failed without a diagnostic. |

### doctor

Looks for the mistakes that fail silently, and says how to fix each. Reads only.

```bash
craftsman doctor [--offline] [--json]
```

| Option | What it does |
|---|---|
| `--offline` | Skip the session check, the only one that reaches the network. |
| `--json` | Print the result as JSON, for agents and CI. |

Each check passes, warns, fails or is skipped, with a line saying how to fix it. Exit 1 when any check fails. A warning doesn't fail.

| Check | What it looks for |
|---|---|
| Toolchain | `rokit.toml` names `craftsman` and `rojo` (and `wally` with a `wally.toml`), and pins this CLI's version. |
| Framework | Control is installed and is this CLI's version, and the game isn't still on Wally's Craftsman. |
| Packages | What is installed is what `craftsman.lock` names, with no local edits. |
| Settings | `Settings.luau` matches `craftsman.toml`, and every value is one its setting can take. |
| Shared packages | No package is both in `CraftsmanPackages` and in your Wally install. Ledger in both fails. |
| Project file | `default.project.json` is what your features route to. |
| Generated declarations | Each feature's `Generated.luau` is current and adopted. |
| Stores | `ServerScriptService.Craftsman.Control.Stores` exists and is a ModuleScript. |
| Places | Each `[places.<role>]` reads, its overlay and baseline exist, and it owns both Craftsman package folders. |
| Session | This machine is signed in, and you are on a project holding one of your universes. |

Run `doctor` after setup, after `update`, and whenever the console can't see something your game declares.

---

## Page: /docs/cli/account-and-agents

**Account and agent commands.** Reference for login, logout, whoami, skills, help and --version.

These commands manage who the CLI acts as, and set up agent skills in your repository.

### login

Signs this machine in to the current console with your passkey.

```bash
craftsman login
```

It opens the console in your browser, where you confirm with your passkey. See [Sign in from the terminal](/docs/getting-started/sign-in-from-the-terminal).

### logout

Signs this machine out and forgets its session.

```bash
craftsman logout
```

If the console can't be reached, the session is still removed from this machine, and the command exits 3. Revoke it from your Account page to end it in the console too.

### whoami

Says who this machine is signed in as, until when, and in which projects.

```bash
craftsman whoami
```

It exits 1 when this machine isn't signed in. When the console says the session has ended or been revoked, it removes the local copy.

None of the three takes options other than `--help`.

### skills

Mirrors `.agents/skills` into `.claude/skills`, or checks that the two match.

```bash
craftsman skills [--check]
```

| Option | What it does |
|---|---|
| `--check` | Only report drift. For CI. |

One agent skill, `craftsman`, ships with the CLI. It covers all of Craftsman: setting a game up, planning a system, Control, Lifecycle, the kit, the CLI and shipping. It tells an agent which call to reach for, what fails silently, what only a person can do, and carries these docs for an agent with no web access.

Install it with the skills CLI:

```bash
npx skills add averyark/craftsman-cli
```

Or copy `skills/craftsman` from `averyark/craftsman-cli` into `.agents/skills/` yourself, then run `craftsman skills` to mirror them for Claude Code.

> [!TIP]
> An agent can't finish `craftsman login` for you, because it waits for your passkey in your browser. Run it yourself, then let the agent carry on.

### help and --version

| Command | What it does |
|---|---|
| `craftsman help` | Lists every command. |
| `craftsman help <command>` | Shows one command's options, notes and examples. Same as `craftsman <command> --help`. |
| `craftsman --version` | Prints the CLI's version and the Control it finds in the project. |

An option a command doesn't have is refused with exit 2, naming the nearest one: `publish has no option --aply. Did you mean --apply?`. Write a value after a space, as `--group Production`, never `--group=Production`.

`craftsman package` builds and publishes Craftsman's own packages. It is for Craftsman's repositories, never a game's.

---

## Page: /docs/cli/exit-codes-and-environment

**Exit codes and environment variables.** What each craftsman exit code means, and every environment variable the CLI reads.

Every `craftsman` command exits with one of four codes, so scripts and CI can tell a refusal from an outage.

### Exit codes

| Code | Meaning | What to do |
|---|---|---|
| 0 | Done: what you asked for happened, or a preview was printed. | Read the output anyway. |
| 1 | A verdict against you: refused, a failed build, or failed tests. | Fix what it names. Running it again unchanged gets the same answer. |
| 2 | The command line or its configuration is wrong. Nothing was sent. | Fix the option, path or setting. |
| 3 | Could not tell: unreachable, an unreadable answer, or no verdict. | It isn't about your change. Try again later. |

A few cases worth knowing:

- `publish` without `--apply` is a preview and exits 0, even when it lists problems.
- An error from the console itself (a server error) exits 3, never 1, because an outage isn't a verdict on your change.
- A test that couldn't be run exits 3, never 0.
- Common exit 2 causes: an unknown option, a missing argument, `--activate` without `--group`, a `CONTROL_ENDPOINT` the CLI refuses, a `--universe` the declaration doesn't name, or a project whose Craftsman is missing, of another version, or still on Wally.

### Environment variables

| Variable | What it does | Read from |
|---|---|---|
| `CONTROL_ENDPOINT` | The console the CLI talks to. Default: the hosted console. | Your shell, then `.env`, then `[console] endpoint` in `craftsman.toml` |
| `CONTROL_DEFINITIONS` | The declaration module `publish` and `status` load, when your places name different ones or none. | Your shell, then `.env` |
| `GH_TOKEN`, `GITHUB_TOKEN` | The GitHub token `publish --apply` starts the build with. It must be allowed to run workflows. Without either, the CLI asks `gh auth token`, then Git's stored login. | Your shell |
| `CRAFTSMAN_LUAU_AST` | A `luau-ast` you already have, for a machine that can't download it. Used when a file calls `:Declare(`. | Your shell |
| `CRAFTSMAN_LUAU_LSP` | A luau-lsp you already have, for `check`. | Your shell |
| `CI` | When set, progress is printed as plain lines. | Your shell |
| `TERM=dumb` | Plain progress lines. | Your shell |
| `NO_COLOR` | No colour in the output. | Your shell |

`.env` is the file at your project's root, next to `craftsman.toml`.

> [!NOTE]
> A project file (`.env` or `craftsman.toml`) may point `CONTROL_ENDPOINT` only at the hosted console or at this machine. Any other host is refused with exit 2, so a cloned repository can't redirect your sign-in. Set it in your own shell to reach another host.

The CLI never reads your game key or an Open Cloud key, from `.env` or anywhere else.

### Files the CLI keeps

| Path | What it is |
|---|---|
| `~/.craftsman/session` | Your sign-in, one per console. `%USERPROFILE%\.craftsman\session` on Windows. |
| `~/.craftsman/tools/` | Pinned tools the CLI downloads, such as luau-lsp and luau-ast. |
| `~/.craftsman/bundles/` | Downloaded packages, reused while they still match `craftsman.lock`. |

---

# Collection: Lifecycle

The module loader. It starts your modules in order and stops them in reverse.

---

## Page: /docs/lifecycle/lifecycle-overview

**What Lifecycle is.** The module loader, and how to boot a game with it.

Lifecycle is the module loader. Point it at a folder. It requires every module in it, runs each `Init` and then each `Start` in dependency order, and later runs each `Stop` in reverse.

A game can use Lifecycle without the kit, and the kit without Lifecycle.

`craftsman install` puts it in `ReplicatedStorage.CraftsmanPackages`. See [Packages and the lock](/docs/project-setup/packages-and-lock).

### Boot a game

```lua
local ServerScriptService = game:GetService("ServerScriptService")
local Lifecycle = require("@game/ReplicatedStorage/CraftsmanPackages/Lifecycle")

Lifecycle:LoadModulesAsync(ServerScriptService.Services):await()
```

Use one server Script and one client LocalScript, each with its own folder.

### API

| Member | What it does |
| --- | --- |
| `Lifecycle:LoadModulesAsync(folder)` | Loads a folder. Returns a Promise of the modules this call loaded. |
| `Lifecycle:StopModulesAsync()` | Stops every module this loader loaded. Returns a Promise. |
| `Lifecycle.Loader()` | Makes an independent loader. |
| `Lifecycle.new(module)` | Adds a `Keeper` to a plain table. |
| `Lifecycle.LoadedModules` | Every loaded module, by name. |
| `Lifecycle.IsLoaded` | True once every `Start` has settled. |
| `Lifecycle.HangWarningSeconds` | See [Settings](/docs/lifecycle/lifecycle-settings). |
| `Lifecycle.TopLevelModulesOnly` | See [Settings](/docs/lifecycle/lifecycle-settings). |

Call `LoadModulesAsync` and `StopModulesAsync` with `:`.

`Lifecycle` is itself a loader. Use `Lifecycle.Loader()` in a test, so stopping your fixtures does not stop the game.

You can load more than one folder into the same loader. A module in a later folder may depend on one from an earlier folder, and one `StopModulesAsync` stops them all.

Next: [Write a module](/docs/lifecycle/lifecycle-write-a-module)

---

## Page: /docs/lifecycle/lifecycle-write-a-module

**Write a module.** The shape of a module, and what goes in Init, Start and Stop.

A module is a table with optional `Init`, `Start` and `Stop`.

```lua
local Inventory = require("./Inventory")

local Shop = {
	Dependencies = { Inventory },
}

function Shop:Init()
	-- Set up state. Inventory:Init has already run.
end

function Shop:Start()
	-- Connect events and loops. Inventory:Start has already finished.
	self.Keeper:Add(workspace.ChildAdded:Connect(function() end))
end

function Shop:Stop()
	-- Runs before Inventory:Stop. The Keeper is destroyed after this returns.
end

return Shop
```

### Members

| Member | When it runs | Use it for |
| --- | --- | --- |
| `Dependencies` | Read at load. | The modules this one needs. |
| `Init` | Before any `Start`. | State and signals. It must not yield. |
| `Start` | After its dependencies finish `Start`. | Connections, loops and calls to other modules. |
| `Stop` | Before its dependencies stop. | Teardown. |
| `Keeper` | Added by the loader, unless the module has one. | Anything to clean up. |

`Dependencies` holds the module tables, not their names. A dependency this loader has not loaded is warned about and ignored.

A module that is not a table is required and registered, and left alone.

### Rules

- Put state and signals in `Init`, and connections and loops in `Start`. Every `Init` runs before any `Start`, so another module can read your fields.
- Put only singletons in a loaded folder. The loader calls `Init` and `Start` on anything that has them.
- Require another game module only if you list it in `Dependencies`. Otherwise talk through a `Signal` it exposes.

### A feature from Control

The loader answers one question: is the code loaded. It answers once, at boot. Craftsman Control answers another: should this feature run now. A feature declared in Control registers in `Start`, and does its work in the handler:

```lua
local Grass = {}

function Grass:Start()
	self.Keeper:Add(Definitions:OnActivate(function(keeper)
		-- Build. Control cleans keeper each time the feature goes off.
	end))
end
```

See [Features](/docs/features/features).

Next: [Which modules load](/docs/lifecycle/lifecycle-which-modules)

---

## Page: /docs/lifecycle/lifecycle-which-modules

**Which modules load.** How the loader walks a folder, and how to skip one.

The loader walks every instance under the folder, and stops at a ModuleScript. A ModuleScript is one singleton. Anything inside it belongs to it, and is never looked at.

```
ServerScriptService.Features   Folder, walked
  Combat                       ModuleScript, loaded
    Replication                ModuleScript, Combat's own, not loaded
  Shared                       Folder, walked
    Wind                       ModuleScript, loaded
```

So a feature written as a folder with an `init.luau` is one module. Two features may each have a part named `Types` without clashing.

A singleton that must load on its own belongs in a Folder, not inside another ModuleScript.

To load every ModuleScript instead, see [Settings](/docs/lifecycle/lifecycle-settings).

### Skip something

| Skipped | Why |
| --- | --- |
| A ModuleScript whose name ends in `.spec` | It is a test. |
| Everything under an instance with `CraftsmanLifecycleIgnore` set to `true` | You asked. |
| The children of a ModuleScript | They are its own. |

With Rojo, mark a folder with an `init.meta.json`:

```json
{
  "attributes": {
    "CraftsmanLifecycleIgnore": true
  }
}
```

### Keep these out

Do not load `ServerScriptService.Craftsman.Control` or `ServerScriptService.Craftsman.Verify`. Control opens its stores when `Stores` is required, because a Luau Execution session runs no Script, so no `Init` or `Start` would ever run there. See [Open your stores](/docs/players-and-data/open-stores).

### Names

Each module is known by its ModuleScript name. Two modules with one name refuse the load, before anything is required.

Next: [Order and failures](/docs/lifecycle/lifecycle-order-and-failures)

---

## Page: /docs/lifecycle/lifecycle-order-and-failures

**Order and failures.** What runs when, and what happens when a module fails.

### What it guarantees

| Phase | Order | When something fails |
| --- | --- | --- |
| Require | Every module, at once, before any `Init`. | It is reported, and nothing it would have run runs. The rest still load. |
| `Init` | One at a time, each after every module it depends on. | Its dependents skip `Init` and `Start`. Other modules carry on. |
| `Start` | At once, each after its dependencies finish `Start`. | Its dependents skip `Start`. |
| `Stop` | Each after every module that depends on it has stopped. | It is reported. Its Keeper is still destroyed. |

### The load

`LoadModulesAsync` resolves once every `Start` has finished or failed.

It rejects before any `Init` in three cases:

| Case | Do |
| --- | --- |
| Two modules share a name. | Rename one. |
| Modules depend on each other in a cycle. | Break the cycle with a `Signal`. |
| A module returns a frozen table. | Do not freeze it, or keep it out of the loaded folder. |

### Warnings

| Warning | Means | Do |
| --- | --- | --- |
| `Init` yielded | `Init` must not yield. The loader waits anyway, so the order holds. | Move the waiting into `Start`. |
| `Start` or `Stop` has not finished | It is still running after `HangWarningSeconds`. The warning names the modules waiting on it. | Find out what it waits for. |

Next: [Settings](/docs/lifecycle/lifecycle-settings)

---

## Page: /docs/lifecycle/lifecycle-settings

**Lifecycle settings.** The two settings in craftsman.toml, and how to set them per loader.

Set them in `craftsman.toml`:

```toml
[lifecycle]
top_level_modules_only = true
hang_warning_seconds = 10
```

| Setting | Loader field | Default | What it does |
| --- | --- | --- | --- |
| `top_level_modules_only` | `TopLevelModulesOnly` | `true` | Do not load a ModuleScript inside another ModuleScript. |
| `hang_warning_seconds` | `HangWarningSeconds` | `10` | Seconds before a running `Start` or `Stop` is reported. |

Lifecycle reads them once, when it is first required. See [craftsman.toml](/docs/project-setup/craftsman-toml).

### Set one on a loader

A field set in code wins, for that loader:

```lua
Lifecycle.TopLevelModulesOnly = false
Lifecycle:LoadModulesAsync(folder):await()
```

With `false`, it loads every ModuleScript under the folder, including ones inside other ModuleScripts.

Next: [Keeper](/docs/lifecycle/lifecycle-keeper)

---

## Page: /docs/lifecycle/lifecycle-keeper

**Keeper.** How to clean up connections, instances, threads and promises.

A Keeper cleans up what you give it. Every loaded module, `Entity` and `Pathfind` owns one. Give every connection to the Keeper of whatever owns it.

```lua
local Keeper = require("@game/ReplicatedStorage/CraftsmanPackages/Keeper")
```

### API

| Call | What it does |
| --- | --- |
| `Keeper.new()` | Makes a Keeper. |
| `keeper:Add(object, key?, cleanup_method?)` | Tracks an Instance, a connection, a function, a thread, a promise, or anything with `Destroy` or `Disconnect`. Returns the object. |
| `keeper:Connect(signal, fn, key?)` | Connects, and tracks the connection. |
| `keeper:Once(signal, fn, key?)` | Connects once, and tracks it. |
| `keeper:Construct(class_or_fn, key?, ...)` | Makes an object, and tracks it. The key comes second. |
| `keeper:Clone(instance, key?)` | Clones an instance, and tracks the clone. |
| `keeper:AddPromise(promise, key?)` | Cancels the promise on cleanup. |
| `keeper:BindToRenderStep(name, priority, fn, key?)` | Binds, and unbinds on cleanup. |
| `keeper:Extend(key?)` | Makes a child Keeper, cleaned with this one. |
| `keeper:Remove(object, key?)` | Removes and cleans one object. |
| `keeper:Pop(object?, key?)` | Stops tracking an object, and does not clean it. |
| `keeper:Clean(key?)` | Cleans everything, or only that key. The Keeper stays usable. |
| `keeper:WrapClean(key?)` | Returns a function that calls `Clean`. |
| `keeper:AttachToInstance(instance)` | Cleans the Keeper when the instance is destroyed. Setting its Parent to nil does not trigger it. The instance must be in the DataModel. |
| `keeper:Destroy()` | The same as `Clean`. The Keeper stays usable. |

`keeper:Add` errors if that Keeper is being cleaned.

### In your own class

Make the Keeper, attach it, then add everything through it:

```lua
function Turret.new(model: Model)
	local self = setmetatable({}, Turret)
	self.Keeper = Keeper.new()
	self.Keeper:AttachToInstance(model)
	self.Keeper:Connect(RunService.Heartbeat, function(dt)
		self:Step(dt)
	end)
	return self
end
```

---

# Collection: Craftsman Kit

Entities, tags, state machines, pathfinding, task control and utilities.

---

## Page: /docs/craftsman-kit/kit-overview

**What the kit is.** Which module to use for each job, how to require it, and how to boot it.

The kit is a Luau framework: entities, tags, state machines, pathfinding, task control and utilities. If a row below fits your job, use that module.

`craftsman install` puts it in `ReplicatedStorage.CraftsmanPackages`. See [Packages and the lock](/docs/project-setup/packages-and-lock).

```lua
local Craftsman = require("@game/ReplicatedStorage/CraftsmanPackages/Craftsman")
```

Each module loads the first time you use it. `Craftsman.StateMachineServer` is `nil` on a client.

### Modules

| Need | Use | Guide |
| --- | --- | --- |
| A wrapper around a character | `Entity` | [Entity](/docs/craftsman-kit/kit-entity) |
| Status effects and timed buffs | `Tag` | [Tag](/docs/craftsman-kit/kit-tag) |
| Character states, replicated if you want | `State`, `Machine`, `StateMachine` | [State machines](/docs/craftsman-kit/kit-state-machine) |
| NPC movement and chasing | `Pathfind` | [Pathfind](/docs/craftsman-kit/kit-pathfind) |
| Async jobs in order, with retries | `Queue` | [Queue](/docs/craftsman-kit/kit-queue) |
| Debounce, throttle, a lock or a cooldown | `Debounce`, `Concurrency` | [Debounce and Concurrency](/docs/craftsman-kit/kit-debounce-and-concurrency) |
| Tweens and springs | `TweenUtil`, `Spring` | [Tweens and springs](/docs/craftsman-kit/kit-tween-and-spring) |
| Character animations | `AnimationUtil` | [AnimationUtil](/docs/craftsman-kit/kit-animation) |
| Sounds | `SoundUtil` | [SoundUtil](/docs/craftsman-kit/kit-sound) |
| Keybinds and device type | `InputUtil` | [InputUtil](/docs/craftsman-kit/kit-input) |
| Mobile buttons | `TouchUtil` | [TouchUtil](/docs/craftsman-kit/kit-touch) |
| UI scale | `InterfaceUtil` | [InterfaceUtil](/docs/craftsman-kit/kit-interface) |
| Casts, overlaps and nearest | `WorldUtil` | [WorldUtil](/docs/craftsman-kit/kit-world) |
| Lerp, angles and number formats | `MathUtil` | [MathUtil](/docs/craftsman-kit/kit-math) |
| String and table helpers | `StringUtil`, `TableUtil` | [StringUtil and TableUtil](/docs/craftsman-kit/kit-string-and-table) |
| Printing values | `PrintUtil`, `Inspect` | [PrintUtil and Inspect](/docs/craftsman-kit/kit-print-and-inspect) |
| Settings | `Config` | [Kit settings](/docs/craftsman-kit/kit-settings) |

To load your own modules, use [Lifecycle](/docs/lifecycle/lifecycle-overview). To clean up, use [Keeper](/docs/lifecycle/lifecycle-keeper).

### Dot or colon

The wrong one shifts the arguments.

| Call with | What |
| --- | --- |
| `:` | Every `WorldUtil` function, `InterfaceUtil:AutoScale`, `PrintUtil`, `StateMachineServer:Start`, `StateMachineClient:Start`, and methods on objects such as `queue:Enqueue`. |
| `.` | Constructors and module functions, such as `Tag.Get`, `Entity.Get`, `TweenUtil.Play`, `InputUtil.BindAction`, `MathUtil.Lerp`. |

### Boot it

The order matters: start the state machine, load your modules, then wrap characters.

```lua
-- Server
Craftsman.StateMachineServer:Start()

Lifecycle:LoadModulesAsync(ServerScriptService.Services):await()

local function track(player: Player)
	player.CharacterAdded:Connect(function(character)
		Craftsman.Entity.new(character, player)
	end)
	if player.Character then
		Craftsman.Entity.new(player.Character, player)
	end
end

Players.PlayerAdded:Connect(track)
for _, player in Players:GetPlayers() do
	track(player)
end
```

```lua
-- Client
Craftsman.StateMachineClient:Start()

Lifecycle:LoadModulesAsync(ReplicatedStorage.Controllers):await()
```

Skip what your game does not use.

### For coding agents

The `craftsman` agent skill covers the kit. See [Account and agent commands](/docs/cli/account-and-agents).

Next: [Kit settings](/docs/craftsman-kit/kit-settings)

---

## Page: /docs/craftsman-kit/kit-settings

**Kit settings.** The settings of the kit in craftsman.toml, and Config.Configure at run time.

Set them in `craftsman.toml`. Keys are lower case. A `Vector2` is `[x, y]`, and a `Color3` is `[r, g, b]` from 0 to 255.

```toml
[kit.interface]
reference = [1920, 1080]
ten_foot_boost = 1.5

[kit.touch]
label_color_idle = [200, 200, 200]

[kit.state_machine]
client_update_min_interval = 0.1
```

The CLI checks each value, and writes it into `CraftsmanPackages/Settings.luau`. That file ships to every client, so a setting is never a secret. See [craftsman.toml](/docs/project-setup/craftsman-toml).

### At run time

```lua
Craftsman.Config.Configure({ PATHFIND = { MAX_COMPUTES_PER_FRAME = 2 } })
```

`Configure` takes the upper case keys. Each call starts again from `craftsman.toml`, so it replaces the call before it. A key you leave out goes back to its `craftsman.toml` value. An array is replaced whole.

Call it before you use any other module, because a few settings are read once.

### kit.state_machine

| Setting | Default | What it does |
| --- | --- | --- |
| `enabled` | `true` | Runs the networking. False turns off both halves. |
| `auto_start_server` | `true` | False makes `StateMachineServer:Start()` do nothing. `StateMachineServer:Init()` still starts it. |
| `auto_start_client` | `true` | False makes `StateMachineClient:Start()` do nothing. |
| `sync_client` | `true` | Sends machine state to clients, and answers their requests. False also stops the server accepting client transitions. |
| `client_update_min_interval` | `0.05` | Fewest seconds between two updates accepted from one player for one machine. Read when the module loads. |

### kit.interface

| Setting | Default | What it does |
| --- | --- | --- |
| `reference` | `[1920, 1080]` | The screen size you design at. |
| `mode` | `"Fit"` | `"Fit"`, `"Height"`, `"Width"` or `"Diagonal"`. |
| `min_scale` | `0.5` | The smallest scale. |
| `max_scale` | `2` | The largest scale. |
| `damping` | `1` | The power the scale is raised to. Below 1 lifts small screens. |
| `ten_foot_boost` | `1.25` | Multiplies the scale on a console. |

### kit.pathfind

Read every frame, so `Configure` applies at once.

| Setting | Default | What it does |
| --- | --- | --- |
| `max_computes_per_frame` | `4` | The most path computations started in one frame. |
| `max_concurrent_computes` | `12` | The most running at once. |

### kit.touch

| Setting | Default | What it does |
| --- | --- | --- |
| `default_asset` | `"rbxassetid://16147465077"` | The image of a button with no icon. |
| `rect_size` | `[144, 144]` | `ImageRectSize` of a sprite sheet button. |
| `rect_offset_idle` | `[1, 146]` | `ImageRectOffset` at rest. |
| `rect_offset_pressed` | `[146, 146]` | `ImageRectOffset` while pressed. |
| `label_color_idle` | `[200, 200, 200]` | Label colour at rest. |
| `label_color_pressed` | `[55, 55, 55]` | Label colour while pressed. |
| `label_text_size` | `14` | Label text size. |
| `overlay_transparency` | `0.3` | Overlay transparency at rest. |
| `overlay_press_delta` | `0.2` | How much that drops while pressed. |
| `overlay_scale` | `0` | Scale part of the overlay size. |
| `overlay_offset` | `32` | Offset part of the overlay size, in pixels. |
| `base_size` | `70` | The pixel size a button is built at. |
| `default_size` | `70` | The size of a button given none, when the jump button is not matched. |
| `match_jump_size` | `true` | Matches a button with no size to the jump button. |
| `jump_button_editable` | `true` | Lets the player move and resize the jump button, at start. |
| `zone_padding` | `8` | Pixels between buttons in one zone. |
| `zone_margin` | `16` | Pixels between a zone and the screen edge. |
| `display_order` | `-1` | `DisplayOrder` of the ScreenGui. Read once, at the first button. |
| `disabled_transparency` | `0.6` | Transparency of a disabled button. |
| `auto_scale` | `false` | Scales the controls with `InterfaceUtil`. Read once, at the first button. |
| `min_size` | `40` | The smallest a button can be resized to. |
| `max_size` | `160` | The largest a button can be resized to. |
| `edit_grid` | `0` | The grid a moved button snaps to. 0 places it freely. |
| `drag_threshold` | `6` | Pixels a touch must move to start a drag. |
| `edit_outline_color` | `[120, 200, 255]` | Outline colour in edit mode. |
| `edit_outline_transparency` | `0.2` | Outline transparency in edit mode. |
| `handle_size` | `16` | Pixel size of the resize handle. |
| `handle_asset` | `"rbxassetid://99822241004086"` | Image of the resize handle. |
| `handle_color` | `[120, 200, 255]` | Colour of the resize handle. |
| `layout_version` | `2` | The version a saved layout carries. Raise it to discard saved layouts. |

Next: [Entity](/docs/craftsman-kit/kit-entity)

---

## Page: /docs/craftsman-kit/kit-entity

**Entity.** A wrapper around a character, with rig references, a Keeper and signals.

`Entity` wraps a character `Model`. The server and each client keep their own wrappers.

Nothing makes an entity for you. Wrap a character on `CharacterAdded`, and an NPC when it spawns.

### Make one

| Call | What it does |
| --- | --- |
| `Entity.new(model, player?)` | Returns the wrapper, making it if needed. Yields up to 5 seconds each for `Humanoid` and `HumanoidRootPart`, and errors if one is missing. |
| `Entity.Get(model)` | The same, and never returns nil. |
| `Entity.Entities[model]` | A lookup that never makes one. |

### An entity

| Member | What it is |
| --- | --- |
| `Instance` | The model. |
| `Player` | The player, if any. |
| `Humanoid` | The humanoid. |
| `HumanoidRootPart` | The root part. |
| `Keeper` | Destroyed when the entity dies. |
| `entity:GetHumanoid()` | Reads the rig now. Never yields. |
| `entity:GetRoot()` | Reads the rig now. Never yields. |
| `entity:Destroy()` | Fires `Removing`, and destroys the model. |

### Signals

They are on the module, so connect to `Entity.Died`, not `entity.Died`. Each passes the entity.

| Signal | Fires when |
| --- | --- |
| `Entity.Spawned` | An entity is made. |
| `Entity.Died` | `Humanoid.Died` fires, or the model leaves the DataModel. |
| `Entity.Removing` | `entity:Destroy()` is called. |

```lua
Craftsman.Entity.Spawned:Connect(function(entity)
	entity.Keeper:Connect(entity.Humanoid.HealthChanged, function(health)
		-- Ends when the entity dies.
	end)
end)
```

### A dead body

After a death, the model may still exist. `Entity.Get` on it makes a new wrapper, and that wrapper fires `Died` only when the model leaves the DataModel. Check `Entity.Entities[model]` first.

Next: [Tag](/docs/craftsman-kit/kit-tag)

---

## Page: /docs/craftsman-kit/kit-tag

**Tag.** Status effects and flags on instances, which stack, expire and replicate.

A tag is a child `NumberValue` named `__TAG_<identifier>`. Tags stack, can expire and can carry attributes. A tag made on the server replicates. Use `Tag`, not CollectionService.

```lua
local Stunned = Craftsman.Tag.Get("Stunned")

-- Server
Stunned:Add(character, 1.5, { Source = "Slam" })

-- Client
entity.Keeper:Add(Stunned:ListenToInstance(character, function(is_stunned)
	set_stun_vfx(is_stunned)
end))
```

Register each tag on both the server and the client. Change tags on the server. On a client, only read them or listen.

### Module functions

| Call | What it does |
| --- | --- |
| `Tag.Get(identifier)` | Returns the tag, making it if needed. Use this one. |
| `Tag.new(identifier)` | Makes a tag. Errors if it exists. |
| `Tag.HasTags(instance, tags)` | True if any is present. Takes a name, a tag or a list. |
| `Tag.GetTags(instance)` | Every registered tag on the instance. |
| `Tag.GetRegistered()` | Every registered tag. |
| `Tag.Is(value)` | True if the value is a tag. |

### Methods

With no `interval`, a tag lasts until removed. With one, it is removed after that many seconds. Calling a tag directly, `tag(instance, ...)`, is the same as `Add`.

| Call | Returns | What it does |
| --- | --- | --- |
| `Add(instance, interval?, attributes?, overwrite_id?)` | id, value | Adds another one. Tags stack. |
| `AddTag(instance, ...)` | value | The same. |
| `SetTag(instance, interval?, attributes?)` | value | Removes the existing ones, then adds one. |
| `ReplaceTag(...)` | value | The same as `SetTag`. |
| `RemoveOthersAndAdd(instance, interval?, attributes?)` | id, value | The same as `SetTag`. |
| `Remove(instance?, id)` | boolean | Removes one. |
| `RemoveAll(instance, filter?)` | nothing | Removes all made on this machine. |
| `IsTagged(instance, only_local?)` | boolean | Sees replicated tags unless you say otherwise. |
| `GetTags(instance)` | values | Includes replicated tags. |
| `GetDuration(instance)` | number | Time left on all timed tags. Wrong on a client for a server tag. |
| `GetToggleTags(instance)` | number | How many untimed tags. |
| `ListenToInstance(instance, callback)` | connection | Calls back now with the state, then on every change. Works on clients. |
| `ListenToInstanceTags(instance, callback)` | connection | Calls back with the value and whether it was added: now for each tag, then on every add or remove. |
| `WhenTagRemoved(id, callback)` | connection | Fires when that id is removed. Does not disconnect itself. |
| `GetTagId(instance, value)` | id | The id of a tag value. |
| `Destroy()` | nothing | Removes every tag made on this machine, destroys its signals, and unregisters it. |

### Signals

These fire only for changes made through this machine.

| Signal | Passes |
| --- | --- |
| `OnTagged` | instance, id, interval |
| `OnTagRemoved` | instance, id |
| `OnInstanceTagged` | instance, is tagged |

### Traps

- Read an attribute with `value:GetAttribute(name)`. `TagIdentifier`, `TagId` and `TagExpiresAt` are reserved.
- Ids are global per tag. Reusing `overwrite_id` on another instance removes the first tag.
- A client cannot remove a server tag.

Next: [State machines](/docs/craftsman-kit/kit-state-machine)

---

## Page: /docs/craftsman-kit/kit-state-machine

**State machines.** Nested states, with optional replication from the server to clients.

A machine holds nested states, and can replicate from the server to clients.

```lua
local machine = Craftsman.Machine.new(`Combat:{player.UserId}`)
local alive = Craftsman.State.new("Alive")
local idle = Craftsman.State.new("Idle")
local attacking = Craftsman.State.new("Attacking", {
	Enter = function(_state, context)
		-- context.Combo
	end,
})

machine:AddState(alive)
machine:AddState(idle, alive)
machine:AddState(attacking, alive)
machine:SetInitial(idle)
machine:Start()
machine:Transition(attacking, { Combo = 1 })
```

### State

`Craftsman.State.new(name, handlers?)`. The handlers are all optional:

| Handler | Arguments |
| --- | --- |
| `Enter` | state, context |
| `Exit` | state, context |
| `Update` | state, dt, context |

A handler does not get the machine. Pass what it needs in `context`. Do not rename a state after you add it.

### Machine

`Craftsman.Machine.new(id?, is_client_authoritative?)`. Replication needs an `id`.

| Method | What it does |
| --- | --- |
| `AddState(state, parent?)` | Registers a state. Add the parent first. |
| `GetState(name)` | Finds a state. |
| `GetCurrent()` | The current state. |
| `IsRunning()` | True while started. |
| `SetInitial(state)` | The state `Start` enters. |
| `Start(context?)` | Enters from the root down to the initial state. |
| `Stop(context?)` | Exits from the current state up to the root. |
| `Transition(target, context?)` | Moves to `target`. False if it is already current. Inside `Enter` or `Exit`, it is queued and returns true. |
| `Update(dt, context?)` | Runs `Update` handlers from the root down. Only while running. |
| `Destroy()` | Fires `Destroyed`, clears the machine and ends replication. |
| `Replicate(targets?)` | Starts replication. |
| `Unreplicate()` | Ends replication. |
| `SetReplicationTargets(targets?)` | Server only. |
| `GetId()`, `SetId(id)` | The machine id. |
| `IsClientAuthoritative()`, `SetClientAuthoritative(state)` | Client authority. |
| `SyncReplication(targets?)` | Sends the state again, or asks for it again. |

| Signal | Passes |
| --- | --- |
| `Transitioned` | machine, from, to, context, source |
| `Destroyed` | machine |

### Traps

- Nothing calls `Update` for you. Drive it from Heartbeat.
- `machine:Replicate` is the same as `StateMachine.Register(machine, targets?)`, and `machine:Unreplicate` as `StateMachine.Unregister(machine)`.
- Do not call `Transition` before `Start`. It changes the state without starting, and a later `Start` into that state fails.

### Replicate

1. Start the hosts when you boot: `StateMachineServer:Start()` and `StateMachineClient:Start()`.
2. Build the machine with the same `id` and state names on both sides. Put the builder in a shared module.
3. Call `machine:Replicate(targets?)` on both sides. With no targets, it goes to all players.

The client runs its own `Enter` and `Exit`, with the context of the server. An initial sync has no context.

Replication keeps a machine alive. Call `Unreplicate` or `Destroy` when its owner goes away, or it leaks.

### Client authority

Mark the machine on both sides, before `Replicate`. The server accepts a client transition only if all of these hold:

- The player is in the targets. With no targets, any player can drive it.
- The context is `nil` or a table.
- It respects `client_update_min_interval`. Faster updates are dropped.

The server checks only that the state exists. Validate game rules yourself.

Settings are in [Kit settings](/docs/craftsman-kit/kit-settings).

Next: [Pathfind](/docs/craftsman-kit/kit-pathfind)

---

## Page: /docs/craftsman-kit/kit-pathfind

**Pathfind.** Move an NPC along a path, chase a target, and recover when stuck.

`Pathfind` drives an NPC along paths. It handles new paths, stuck recovery, blocked paths, jumps and moving targets.

```lua
local humanoid = npc:FindFirstChildOfClass("Humanoid")
local pathfind = Craftsman.Pathfind.new({ Entity = npc, Humanoid = humanoid })

pathfind.OnArrived:Connect(function()
	attack(target)
end)

pathfind:Chase(target, 6)
```

Do not use it on a player character. On the server, it takes network ownership of the parts.

### Make one

`Pathfind.new({ Entity, Humanoid?, Mover?, Config? })`

| Field | What it is |
| --- | --- |
| `Entity` | The NPC, a Model or BasePart. |
| `Humanoid` | The humanoid. |
| `Mover` | Your own `(Vector3) -> ()`, instead of a humanoid. |
| `Config` | Overrides. An unknown key is dropped. A table replaces the default whole. |

It errors if you give neither `Humanoid` nor `Mover`.

### Methods

| Call | What it does |
| --- | --- |
| `Goto(position)` | Computes a path and walks it. Cancels a chase. |
| `GotoAsync(position)` | The same, with a Promise. It resolves on arrival. |
| `Chase(target, stopping_distance?)` | Follows a part or model. Stops when the target is gone or dead. |
| `Stop()` | Cancels the chase, clears the path and fires `OnPathEnded`. |
| `Destroy()` | Cleans up. It does not call `Stop`. |
| `ComputePathToAsync(position)` | A Promise of the status and waypoints. Not while walking or chasing. |
| `SetLinkHandler(label, handler?)` | Handles links with this label, for this NPC. |
| `RecalculatePath()` | Computes the path again. |
| `GetRoot()` | The root part. |
| `GetCurrentPosition()` | The current position. |

`GotoAsync` rejects with `"Stuck"`, `"Unreachable"`, a `PathStatus` name, the compute error, or `"Link:<label>"`.

A direct path starts inside the call, so connect signals first. Otherwise the compute is queued, and the earliest it starts is the next frame.

Fields: `IsMoving`, `Waypoints`, `CurrentWaypointIndex`, `TargetPosition`, `ChaseTarget`, `Keeper`.

### Signals

| Signal | Fires when |
| --- | --- |
| `OnPathCalculated(waypoints)` | A path is computed. |
| `OnPathEnded()` | Movement ends, for any reason. |
| `OnArrived()` | The target is reached. |
| `OnStuck()` | Recovery fails more than `STUCK_RETRIES` times. |
| `OnPathFailed(reason)` | No usable path was found, the path ended short of the target, the compute errored, or a link handler threw. |

### Module functions

| Call | What it does |
| --- | --- |
| `Pathfind.SetLinkHandler(label, handler?)` | Handles links for every NPC. A handler on the NPC wins. |
| `Pathfind.GetStats()` | `Agents`, `Queued` and `InFlight`. |
| `Pathfind.Movers.Humanoid(humanoid)` | The default mover. |

### Config

| Key | Default |
| --- | --- |
| `RECALCULATE_THROTTLE` | `0.2` |
| `RECALCULATE_DISTANCE_THRESHOLD` | `4` |
| `IDLE_RECALCULATE_INTERVAL` | `1` |
| `AGENT_RADIUS` | `2` |
| `AGENT_HEIGHT` | `5` |
| `AGENT_CAN_JUMP` | `true` |
| `AGENT_COSTS` | `{}` |
| `WAYPOINT_SPACING` | `4` |
| `WAYPOINT_ARRIVAL_RADIUS` | `1.5` |
| `WAYPOINT_ADVANCE_RADIUS` | `2.8` |
| `STUCK_TIMEOUT` | `2` |
| `STUCK_RETRIES` | `2` |
| `STALL_TIMEOUT` | `2` |
| `PARTIAL_PATH` | `true` |
| `DIRECT_PATH` | `true` |
| `DIRECT_PATH_MAX_DISTANCE` | `40` |
| `DIRECT_PATH_MAX_STEP` | `2` |
| `VISUALISE` | `false` |

`VISUALISE` draws the path, and is read only when you make the pathfinder.

### Direct paths

With `DIRECT_PATH` on, the NPC walks straight at the target, with no path service, when all of these hold:

- `AGENT_COSTS` is empty.
- The target is within `DIRECT_PATH_MAX_DISTANCE` studs across and `AGENT_HEIGHT` up or down.
- A spherecast to the target hits nothing.
- Ground is found along the whole line, with no height change above `DIRECT_PATH_MAX_STEP` between samples.

`OnPathCalculated` then passes one waypoint. Set `DIRECT_PATH = false` if you need the full list, or costs must apply.

### Links

A handler is `(pathfind, from, to) -> ()`. It runs when the NPC reaches a waypoint with `Action = Custom` and a matching label. It may yield. When it returns, the NPC moves on.

If it throws, the NPC halts, and a pending `GotoAsync` rejects with `"Link:<label>"`. A custom waypoint with no handler is walked like any other.

Next: [Queue](/docs/craftsman-kit/kit-queue)

---

## Page: /docs/craftsman-kit/kit-queue

**Queue.** Run async jobs one at a time, with retries, timeouts and deduplication.

`Queue` runs async jobs one at a time, in order. Use it for anything that must not overlap.

```lua
local SaveQueue = Craftsman.Queue.new({
	DefaultMaxRetries = 3,
	DefaultRetryDelay = 1,
	DefaultBackoffFactor = 2,
	DefaultTimeout = 10,
})

SaveQueue:Enqueue(function(context)
	return Save(context.Key, context.Data)
end, { Key = `save:{player.UserId}`, Context = { Key = player.UserId, Data = data } })
	:catch(function(err)
		warn(`save failed: {err}`)
	end)

game:BindToClose(function()
	SaveQueue:Drain():await()
end)
```

### Options

`Craftsman.Queue.new(options?)`

| Option | Default |
| --- | --- |
| `DefaultMaxRetries` | `0` |
| `DefaultRetryDelay` | `0` seconds |
| `DefaultBackoffFactor` | `1` |
| `DefaultJitterFactor` | `0` |
| `DefaultTimeout` | None |
| `MaxSize` | No limit |
| `RetryOn(err, attempt, context, entry)` | Retries on every error |

### Enqueue

`queue:Enqueue(executor, options?)` returns the Promise, the id and whether it merged.

- `executor(context, attempt, id)` may yield, return a value, return a Promise, or throw. On an idle queue, it starts inside `Enqueue`.
- Options: `Key`, `Context`, `Timeout`, `MaxRetries`, `RetryDelay`, `BackoffFactor`, `JitterFactor`, `RetryOn`.
- If a job with the same `Key` is pending or running, you get its Promise, and `merged` is true.

Always `:catch` the Promise.

| When | Rejects with |
| --- | --- |
| At once | `"QueueClosed"`, `"QueueFull"`, `"InvalidExecutor"` |
| Later | The error, `"QueueTimeout"`, the cancel reason, `"QueueCleared"`, `"QueueClosed"`, `"QueueDestroyed"` |

### Retries

Retry *n* waits `RetryDelay * BackoffFactor^(n-1)` seconds, then applies jitter. A job makes `MaxRetries + 1` attempts. With `RetryOn` set, it can make one more.

### Methods

| Call | What it does |
| --- | --- |
| `Cancel(id, reason?)` | Rejects a pending job. A running job is only flagged, and still resolves if it succeeds. |
| `Clear(reason?)` | Rejects all pending jobs. |
| `Pause()` | Pauses. |
| `Resume()` | Resumes. |
| `Drain()` | A Promise. True once empty. False if paused with jobs pending. |
| `Close(reason?)` | Refuses new jobs, and clears pending ones. |
| `Destroy(reason?)` | Destroys the queue. |
| `GetSize()` | The size. |
| `GetPendingCount()` | How many are pending. |
| `IsPaused()` | True while paused. |
| `IsClosed()` | True once closed. |
| `GetStats()` | The stats. |

### Signals

| Signal | Passes |
| --- | --- |
| `Enqueued` | id, context, key |
| `Started` | id, attempt, context, key |
| `Succeeded` | id, result, context, key, attempts |
| `Failed` | id, reason, context, key, attempts |
| `Retried` | id, error, attempt, delay, context, key |
| `Cancelled` | id, reason, context, key |
| `Drained` | stats |

Next: [Debounce and Concurrency](/docs/craftsman-kit/kit-debounce-and-concurrency)

---

## Page: /docs/craftsman-kit/kit-debounce-and-concurrency

**Debounce and Concurrency.** Debounce or throttle a function, and lock or cool down per key.

| Need | Use |
| --- | --- |
| Async jobs in order, with retries | [Queue](/docs/craftsman-kit/kit-queue) |
| One call after input settles | `Debounce.new` |
| At most one call every N seconds | `Debounce.throttle` |
| No overlapping runs per key | `Concurrency.AsyncLock` |
| A cooldown per key | `Concurrency.KeyedDebounce` |

### Debounce

All waits are in seconds. The option keys are lower case.

| Call | Defaults |
| --- | --- |
| `Debounce.new(fn, wait, { leading?, trailing?, maxWait? }?)` | `leading` false, `trailing` true |
| `Debounce.throttle(fn, wait, { leading?, trailing? }?)` | Both true. `maxWait` is `wait`. |

Call it as `d(...)`, `d:Call(...)` or `d.Call(...)`. `d.Call` is bound, so you can pass it as a callback.

| Method | What it does |
| --- | --- |
| `d:Cancel()` | Drops a pending call. There is no `Destroy`, so use this to clean up. |
| `d:Flush()` | Runs a pending call now. |
| `d:IsPending()` | True if a call is pending. |

- It returns the result of the last run, which may be old.
- A trailing call gets the last arguments and runs in its own thread, so its errors never reach you.

```lua
local fire = Craftsman.Debounce.throttle(shoot, 0.2)
tool.Activated:Connect(fire.Call)
```

### AsyncLock

`Craftsman.Concurrency.AsyncLock.new()` is a lock per key.

| Method | What it does |
| --- | --- |
| `lock:Execute(key, fn, ...)` | False if locked. Otherwise spawns `fn`, holds the lock until it ends, and returns true. Errors are warned. Results are dropped. |
| `lock:IsLocked(key)` | True if locked. |
| `lock:Release(key)` | Forces it open. Avoid this while `fn` runs, because a second run could overlap. |

### KeyedDebounce

`Craftsman.Concurrency.KeyedDebounce.new(cooldown)` is a cooldown per key.

| Method | What it does |
| --- | --- |
| `kd:Call(key, fn, ...)` | False while cooling down. Otherwise records the time, spawns `fn`, and returns true. A refused call does not extend the cooldown. |
| `kd:Clear(key)` | Clears the cooldown. |

```lua
local AbilityCooldown = Craftsman.Concurrency.KeyedDebounce.new(1)

AbilityRemote.OnServerEvent:Connect(function(player)
	AbilityCooldown:Call(player, use_ability, player)
end)
```

### Keys

Keys are held weakly. A table or Instance with no other reference can be collected, which resets its lock or cooldown without a word. Strings, numbers and players in the game are safe.

Next: [Tweens and springs](/docs/craftsman-kit/kit-tween-and-spring)

---

## Page: /docs/craftsman-kit/kit-tween-and-spring

**Tweens and springs.** Named animation templates with TweenUtil, and physics with Spring.

### TweenUtil

Register a named template once, then play it. Use `QuickPlay` for a one off.

```lua
TweenUtil.new("PanelOut", Enum.EasingStyle.Quad, { BackgroundTransparency = 1, Title = { TextTransparency = 1 } }, 0.25)
TweenUtil.new("Pop", "Spring", { Size = UDim2.fromOffset(120, 120) }, 0.6, 4)

TweenUtil.Play("Pop", button)
TweenUtil.QuickPlay(frame, { Position = UDim2.fromScale(0.5, 0.5) }, 0.4, Enum.EasingStyle.Back)
```

`TweenUtil.new(name, style_or_type, properties, duration_or_damping?, direction_or_frequency?)`

| Style | Fourth argument | Fifth argument |
| --- | --- | --- |
| An `Enum.EasingStyle` | Duration, default `0.3` | Direction, default `Out` |
| `"Spring"` | Damping, default `1` | Frequency in Hz, default `8` |

A property whose value is a table targets a child of that name.

Registering a name again warns if the definition matches, and errors if it differs. Register each once, from one place.

| Call | What it does |
| --- | --- |
| `TweenUtil.Play(name, instance)` | Plays. Returns a `Tween`, a `TweenGroup` if it drives more than one instance, or `nil`. |
| `TweenUtil.PlayFrom(name, instance, start)` | Sets `start` at once, then plays. |
| `TweenUtil.QuickPlay(instance, ...)` | Always TweenService. Arguments come in any order: a number is the duration, a table the properties, an `EasingStyle` the style, an `EasingDirection` the direction. |
| `TweenUtil.Stop(instance, property?)` | Stops one property, or everything on the instance and its descendants. |

`Play` returns `nil` when the name is missing, the template is a spring, or no property resolves.

A `TweenGroup` has `Tweens`, `PlaybackState`, `Completed`, `:Play()`, `:Pause()` and `:Cancel()`. `Completed` fires once, after every tween ends.

Before each play, TweenUtil cancels any tracked tween on the same instance and property. It cancels the whole old tween, so its other properties stop too. It does not track a tween you made with TweenService.

### Spring

| Value | Means |
| --- | --- |
| Damping `1` | No overshoot. |
| Damping below `1` | Bounces. |
| Damping above `1` | Sluggish. |
| Frequency | In Hz. Higher is faster. `math.huge` snaps at once. |

| Call | What it does |
| --- | --- |
| `Spring.target(instance, damping, frequency, properties)` | Moves properties, keeping their velocity. A target of another type errors. Also takes `Pivot` on a PVInstance and `Scale` on a Model. |
| `Spring.stop(instance, property?)` | Stops, without firing `completed`. |
| `Spring.completed(instance, callback)` | Fires once every spring has settled. |
| `Spring.new(value, damping, frequency, render_phase?)` | Makes a spring on a value. |

A value spring has `:setGoal`, `:getValue`, `:getGoal`, `:getVelocity`, `:setVelocity`, `:setDampingRatio`, `:setFrequency`, `:reset(value?)`, `:stop()`, `:isAnimating()`, `:onComplete(callback)` and `:destroy()`.

`:getVelocity` and `:setVelocity` error on a CFrame spring.

It works on number, boolean, UDim, UDim2, Vector2, Vector3, Color3, CFrame, NumberRange and ColorSequence.

Next: [AnimationUtil](/docs/craftsman-kit/kit-animation)

---

## Page: /docs/craftsman-kit/kit-animation

**AnimationUtil.** Register animations by name, and play them on a rig.

Register each animation once, then use its name.

```lua
AnimationUtil.BulkRegister({ Slash = { AnimationId = "rbxassetid://789" } }, true)

local animate = AnimationUtil.GetSharedAnimate(entity.Humanoid)
animate:Load({ FromRegistry = "Slash" })
animate:Play("Slash", { FadeTime = 0.1, Priority = Enum.AnimationPriority.Action })
```

Properties are `FadeTime`, `Weight`, `Speed`, `Priority` and `Looped`, all optional.

### Register

| Call | What it does |
| --- | --- |
| `AnimationUtil.Register(name, id, should_preload?)` | Registers one. Preloads on the client only. |
| `AnimationUtil.BulkRegister(map, should_preload?)` | Registers many. Reads only `AnimationId`. |
| `AnimationUtil.Unregister(name)` | Removes one. Errors if it is not registered. |

### Play once

`AnimationUtil.QuickPlay(animator, name, properties?)` returns the track. It never loops, and the track cleans itself up. It errors if the name is not registered.

### Play on a rig

| Call | What it does |
| --- | --- |
| `AnimationUtil.GetSharedAnimate(container)` | The same `Animate` for a container, every time. Use an `Animator`, `Humanoid` or `AnimationController`. A Model is accepted here, but `:Load` then errors. |
| `AnimationUtil.GetLocalAnimate()` | Client only. `nil` if the character is not ready. It does not wait. |
| `AnimationUtil.new(container)` | An `Animate` that is not shared. |

| Method | What it does |
| --- | --- |
| `:Load({ FromRegistry = name, AnimationName? })` | Loads from the registry. |
| `:Load({ AnimationName, AnimationId })` | Loads directly. |
| `:Play(name, properties?)` | Plays. |
| `:Stop(name, fade?)` | Stops one. |
| `:StopAll(fade?)` | Stops all. |
| `:Unload(name)` | Unloads one. |
| `:UnloadAll()` | Unloads all. |
| `:Destroy()` | Destroys it. |

`:Load` returns the track, and also takes `ShouldPreload`. `Play`, `Stop` and `Unload` error if the name is not loaded.

`Destroy` does not remove an `Animate` from the shared cache. Destroy it only if you made it with `.new`.

Next: [SoundUtil](/docs/craftsman-kit/kit-sound)

---

## Page: /docs/craftsman-kit/kit-sound

**SoundUtil.** Sound groups, a registry, one shots and a player for each object.

```lua
SoundUtil.ConfigureSoundGroup("SFX", { Volume = 0.8 })
SoundUtil.BulkRegister({
	Hit = { SoundId = "rbxassetid://123", SoundGroup = "SFX" },
}, true)

SoundUtil.QuickPlay({ SoundName = "Hit", Position = hit_position })
```

Properties are `SoundGroup`, `Volume`, `Looped`, `RollOffMode`, `RollOffMaxDistance`, `RollOffMinDistance`, `TimePosition` and `PlaybackSpeed`, all optional.

### Sound groups

| Call | What it does |
| --- | --- |
| `SoundUtil.AddSoundGroup(name_or_group)` | Gets or makes the group under SoundService. |
| `SoundUtil.ConfigureSoundGroup(name_or_group, properties)` | Sets its properties. |
| `SoundUtil.GetSoundGroup(name)` | Finds it. Never makes it. |
| `SoundUtil.RemoveSoundGroup(name)` | Removes it. Errors unless SoundUtil gave it to you. |

### Register

| Call | What it does |
| --- | --- |
| `SoundUtil.Register(name, id, properties?, should_preload?)` | Registers one. An existing name is replaced. |
| `SoundUtil.BulkRegister(map, should_preload?)` | Registers many. |
| `SoundUtil.Unregister(name)` | Removes one. Errors if it is not registered. |

A string `SoundGroup` here makes the group if needed.

### Play once

`SoundUtil.QuickPlay(params)` returns the sound. Params are `SoundName`, `SoundId?`, `SoundGroup?`, `Properties?`, `Part?`, `CFrame?` and `Position?`.

| You give | It plays |
| --- | --- |
| `Part` | Inside that part. |
| `CFrame` or `Position` | From a temporary anchored part there. |
| Neither | With no position. |

`Looped` is always off. The sound and any temporary part are cleaned up when it ends, when you stop it, or when either is destroyed.

### A player for each object

| Call | What it does |
| --- | --- |
| `SoundUtil.GetSharedSoundPlayer(container)` | One shared player per container. |
| `SoundUtil.GetLocalSoundPlayer()` | The shared player for SoundService. It is not per player. |
| `SoundUtil.new(container)` | A player that is not shared. |

| Method | What it does |
| --- | --- |
| `:Load({ FromRegistry = name })` | Loads from the registry. |
| `:Load({ SoundName, SoundId, ... })` | Loads directly. |
| `:Play(name, { PlayDuplicate = true, ... }?)` | Without `PlayDuplicate`, replays the cached sound and ignores the properties. |
| `:Stop(name)`, `:Pause(name)`, `:Resume(name)` | Acts on one. |
| `:StopAll()`, `:PauseAll()`, `:ResumeAll()` | Acts on all. |
| `:Unload(name)`, `:UnloadAll()` | Unloads. |
| `:Destroy()` | Destroys it. |

`:Load` returns the sound, and also takes `ShouldPreload`. A method that takes a name errors if the name is not loaded.

Next: [InputUtil](/docs/craftsman-kit/kit-input)

---

## Page: /docs/craftsman-kit/kit-input

**InputUtil.** Named actions, rebinding and device type.

`InputUtil` works with named actions. Keys, gamepad buttons, touch buttons and your own code can all hold one action. `Began` fires on the first press, and `Ended` on the last release.

Listen to actions, not to `UserInputService`.

```lua
InputUtil.SetPrimitiveBinding("Dash", { Enum.KeyCode.Q, Enum.KeyCode.ButtonX })
InputUtil.SetPrimitiveBinding("Attack", { Enum.UserInputType.MouseButton1 })

self.Keeper:Add(InputUtil.Began:Connect(function(action)
	if action == "Dash" then
		dash()
	end
end))
```

Call every function with `.`. It is safe to require on the server, but only a client gets input. Game processed input, such as typing in chat, never starts an action.

### Bindings

| Call | What it does |
| --- | --- |
| `SetPrimitiveBinding(action, keys)` | Records the default, and binds it if the action is unbound. Use this for defaults. |
| `BindAction(action, keys)` | Replaces the binding, for a player rebind. Held keys are released. Do not change `keys` after this. |
| `ResetBinding(action?)` | Restores the default of one action, or all. |
| `GetBinding(action)` | A copy of the keys, or `nil`. |
| `UnbindAction(action)` | Removes the binding. Does not fire `BindingChanged`. |

### State

| Call | What it does |
| --- | --- |
| `SetActionState(action, is_active, source, input_object?)` | Presses or releases for `source`, which is any unique value. |
| `IsActionActive(action)` | True while any source holds it. |

### Signals

| Signal | Passes |
| --- | --- |
| `Began` | action, input object |
| `Ended` | action, input object |
| `BindingChanged` | action, new keys, old keys |
| `DeviceChanged` | device, previous device |

The input object can be a frozen stand in, for example after focus is lost.

### Helpers

| Call | What it does |
| --- | --- |
| `GetMovementVector()` | A unit vector, or zero, from WASD and the left stick of Gamepad1. Forward is -Z. Any stick tilt gives full length. It ignores bindings, and has no deadzone. |
| `GetCameraDelta()` | The mouse delta. |
| `GetDeviceType()` | `"KeyboardMouse"`, `"Gamepad"` or `"Touch"`. |

Next: [TouchUtil](/docs/craftsman-kit/kit-touch)

---

## Page: /docs/craftsman-kit/kit-touch

**TouchUtil.** Mobile buttons that fire InputUtil actions, with player layouts.

A touch button is one more source for an [InputUtil](/docs/craftsman-kit/kit-input) action, so one `Began` handler serves every device. Client only.

Make buttons on every device. Each shows only on the devices in `ShowOn`, which defaults to `{ "Touch" }`.

```lua
self.Keeper:Add(TouchUtil.Button("Dash", { Text = "Dash", Zone = "BottomRight", VisibleWhen = { Combat = true } }))
TouchUtil.SetContext("Combat", true)
```

### Options

`TouchUtil.Button(action, options?)`

| Group | Fields |
| --- | --- |
| Look | `Icon`, `IsNormalImage`, `Text`, `Overlay`, `OverlayTransparency`, `OverlayScale`, `OverlayOffset` |
| Place | `Zone`, `Order`, `Position` |
| Size | `Size`, `MinSize`, `MaxSize` |
| Visible | `ShowOn`, `VisibleWhen` |
| Editing | `LayoutId`, `Movable`, `Resizable` |
| Input | `Sink` |

`Zone` is `TopLeft`, `Top`, `TopRight`, `Left`, `Center`, `Right`, `BottomLeft`, `Bottom` or `BottomRight`. The default is `BottomRight`. `LayoutId` defaults to the action name.

### A button

| Kind | Members |
| --- | --- |
| Setters, which chain | `:SetIcon`, `:SetText`, `:SetOverlay`, `:SetSize`, `:SetSizeRange`, `:SetZone(zone, order?)`, `:SetPosition`, `:SetVisibleWhen`, `:SetShowOn`, `:SetEnabled`, `:SetVisible`, `:SetMovable`, `:SetResizable` |
| Others | `:IsPressed()`, `:GetLayout()`, `:ApplyLayout(entry)`, `:ResetLayout()`, `:Destroy()` |
| Overlay | `:SetOverlayTransparency`, `:SetOverlayScale`, `:SetOverlayOffset` |
| Signals | `Pressed` and `Released` pass the input object, `Moved` the position, `Resized` the size |

Put buttons in a Keeper.

### Contexts

`SetContext(name, state)`, `SetContexts(map)` and `GetContext(name)` switch groups of buttons on and off. A context never set reads as false.

### Edit mode

`SetEditMode(state)` lets the player drag and resize. Buttons fire no actions while it is on. See also `IsEditMode()`, `SetGrid(pixels?)` and `SetZoneOffset(zone, offset)`.

### Layouts

Saving a layout is up to you.

| Call | What it does |
| --- | --- |
| `GetLayout()` | `Version` and `Entries`, only for buttons the player changed. |
| `ApplyLayout(layout)` | False if the version differs. Make the buttons first. |
| `ResetLayout(layout_id?)` | Restores one button, or all. |

```lua
TouchUtil.ApplyLayout(saved_layout)
self.Keeper:Add(TouchUtil.LayoutChanged:Connect(function()
	save_layout(TouchUtil.GetLayout())
end))
```

### Existing buttons

Adopting adds editing and layouts to a GUI, and leaves its input alone. An adopted button never drives InputUtil.

| Call | What it does |
| --- | --- |
| `Adopt(gui_object, options?)` | Adopts any GuiObject. |
| `AdoptJumpButton(options?)` | Adopts the Roblox jump button. `nil` if `jump_button_editable` is false or there is none. |
| `SetJumpButtonEditable(state)` | Lets the player edit it. |
| `GetJumpButton()` | The adopted jump button, or `nil` if none is adopted. |

### Other calls

| Call | What it does |
| --- | --- |
| `GetButtons(action)` | The buttons of an action. |
| `GetButton(layout_id)` | One button. |
| `GetRoot()` | The root. |
| `SetEnabled(state)` | Shows or hides the whole layer. |
| `DebugLayout()` | Prints the layout. |
| `Destroy()` | Removes it. |

| Signal | Passes |
| --- | --- |
| `ContextChanged` | context, state, previous |
| `EditModeChanged` | enabled |
| `LayoutChanged` | layout id, entry |

Its look is under `[kit.touch]`. See [Kit settings](/docs/craftsman-kit/kit-settings).

Next: [InterfaceUtil](/docs/craftsman-kit/kit-interface)

---

## Page: /docs/craftsman-kit/kit-interface

**InterfaceUtil.** Scale UI to any screen by driving a UIScale.

`InterfaceUtil` scales UI to any screen, by driving a `UIScale` from the viewport size.

```lua
self.Keeper:Add(InterfaceUtil:AutoScale(hud_gui, { Mode = "Height", Damping = 0.7 }))
```

### API

| Call | What it does |
| --- | --- |
| `InterfaceUtil.ComputeScale(viewport, options?)` | Works out a number. Call it with `.`. |
| `InterfaceUtil:AutoScale(target, options?)` | Call it with `:`. Client only. Returns a function that unbinds. |

`AutoScale` reuses the `UIScale` of the target, or makes one. It follows the viewport and camera. Calling it again on one target replaces the old binding. Give the returned function to a Keeper.

### Options

| Option | What it is |
| --- | --- |
| `Reference` | The screen size you design at. |
| `Mode` | `"Fit"`, `"Height"`, `"Width"` or `"Diagonal"`. |
| `Min` | The smallest scale. |
| `Max` | The largest scale. |
| `Damping` | The power the scale is raised to. |
| `Boost` | A multiplier. In `AutoScale`, it defaults to `ten_foot_boost` on a console and otherwise 1. In `ComputeScale`, it defaults to 1. |

Any other option you leave out comes from `[kit.interface]`. See [Kit settings](/docs/craftsman-kit/kit-settings).

Next: [WorldUtil](/docs/craftsman-kit/kit-world)

---

## Page: /docs/craftsman-kit/kit-world

**WorldUtil.** Casts, ground checks, overlaps, nearest and screen projection.

Always call `WorldUtil` with `:`. With `.`, the arguments shift and it errors.

```lua
local params = RaycastParams.new()
params.FilterType = Enum.RaycastFilterType.Exclude
params.FilterDescendantsInstances = { workspace.Characters }
WorldUtil:AddRayParams("IgnoreCharacters", params)

local grounded = WorldUtil:IsGrounded({ Position = root.Position, Distance = 3.5, RayParamsId = "IgnoreCharacters" })
local hits = WorldUtil:GetPartsInRadius({ Center = root.Position, Radius = 12 })
```

### Params

A cast needs `RayParams` or a registered `RayParamsId`. There is no default, so a missing id errors. An overlap falls back to the engine defaults.

Register them once, usually in `Init`:

| Rays | Overlaps |
| --- | --- |
| `:AddRayParams(id, params)` | `:AddOverlapParams(id, params)` |
| `:GetRayParams(id)` | `:GetOverlapParams(id)` |
| `:RemoveRayParams(id)` | `:RemoveOverlapParams(id)` |

### Casts

| Call | Param table | Returns |
| --- | --- | --- |
| `:RayBelow` | `Position`, `Distance`, `RayParamsId` or `RayParams` | Result or `nil` |
| `:IsGrounded` | The same | Boolean |
| `:GetGroundPosition` | The same | `Vector3` or `nil` |
| `:GetGroundNormal` | The same | `Vector3` or `nil` |
| `:RayFront` | `CFrame`, `Distance`, `Direction?`, `RayParamsId` or `RayParams` | Result or `nil`, along `LookVector` |
| `:RaySide` | The same | Result or `nil`, along `RightVector` |
| `:RayUp` | The same | Result or `nil`, along `UpVector` |

`Direction` is added to the cast vector.

Each cast also takes these:

| Option | Values | Default |
| --- | --- | --- |
| `Mode` | `"Raycast"`, `"Block"`, `"Sphere"` | `"Raycast"` |
| `BlockSize` | `Vector3` | `Vector3.one` |
| `SphereRadius` | Number | `1` |

### Overlaps

Each returns a list of parts, and takes `OverlapParamsId` or `OverlapParams`.

| Call | Param table |
| --- | --- |
| `:GetPartsInRadius` | `Center`, `Radius` |
| `:GetPartsInBox` | `CFrame`, `Size` |
| `:GetPartsInPart` | `Part` |

### Nearest

| Call | Returns | Note |
| --- | --- | --- |
| `:GetDistance(a, b)` | Number | Each is a Vector3, CFrame, BasePart or Model. |
| `:GetNearest({ Origin, Instances })` | Instance, distance | `GetNearestPart` is the same. |
| `:GetNearestPoint(origin, points)` | Vector3, distance | Takes plain arguments. |

### Screen

Both use viewport coordinates, so the GUI inset is not included.

| Call | Returns |
| --- | --- |
| `:WorldToScreen(position, camera?)` | `Vector2` or `nil`, and whether it is on screen |
| `:ScreenToWorld(screen_position, depth, camera?)` | `Vector3` or `nil` |

Next: [MathUtil](/docs/craftsman-kit/kit-math)

---

## Page: /docs/craftsman-kit/kit-math

**MathUtil.** Interpolation, angles, curves, random picks and number formats.

All functions are pure, except the random ones. Call them with `.`.

### Interpolation

| Function | What it does |
| --- | --- |
| `Lerp(a, b, t)` | Not clamped. |
| `InverseLerp(a, b, v)` | Not clamped. |
| `Map(v, in_min, in_max, out_min, out_max)` | Not clamped. |
| `SmoothLerp(current, target, rate, dt)` | The same at any frame rate. |

### Numbers

| Function | What it does |
| --- | --- |
| `RoundTo(v, step)` | Rounds to the nearest multiple of `step`. |
| `Wrap(v, min, max)` | Wraps into the range, up to but not including `max`. |
| `ApplyDeadzone(v, threshold)` | Rescales what is above the threshold. |

### Vectors

| Function | What it does |
| --- | --- |
| `Direction(origin, target)` | A unit vector, or zero. |
| `Flatten(v)` | Sets Y to 0. |
| `FlatDistance(a, b)` | Distance on the XZ plane. |
| `AngleBetween(a, b)` | In radians. |

### Angles

| Function | What it does |
| --- | --- |
| `ShortestAngle(current, target)` | In degrees. |
| `ShortestAngleRad(current, target)` | In radians. |
| `ApproachAngle(current, target, alpha)` | In radians, along the shortest path. |

### Curves and random

| Function | What it does |
| --- | --- |
| `QuadraticBezier(t, p0, p1, p2)` | `t` comes first. |
| `CubicBezier(t, p0, p1, p2, p3)` | `t` comes first. |
| `WeightedRandom(weights, rng?)` | Returns a key, or `nil` if the table is empty. Uses `rng` if you pass a `Random`. |
| `RandomInRadius(center, radius)` | A random point on the XZ disc. |

### Formats

| Function | Example |
| --- | --- |
| `Abbreviate(n)` | `"1.5k"`, `"2.3M"` |
| `AddCommas(n)` | `"1,000,000"` |
| `NumberToWords(n)` | The number in words. |
| `FormatTime(seconds)` | `"MM:SS"`. There is no hours field. |

Next: [StringUtil and TableUtil](/docs/craftsman-kit/kit-string-and-table)

---

## Page: /docs/craftsman-kit/kit-string-and-table

**StringUtil and TableUtil.** String and table helpers, and which ones change their input.

Call all of these with `.`.

### StringUtil

Lengths are in bytes.

| Group | Functions |
| --- | --- |
| Tests | `StartsWith`, `EndsWith`, `Contains`, `IsWhitespace` |
| Whitespace | `Trim`, `RemoveWhitespace` |
| Case | `ToTitleCase`, `ToSnakeCase` |
| Length | `PadStart(text, length, char?)`, `PadEnd(text, length, char?)`, `Truncate(text, max, suffix?)` |
| Words | `Pluralize(text, count, suffix?)` |
| Numbers | `FormatDecimal(v, places?)` |
| Rich text | `EscapeRichText`, `StripRichText` |
| Templates | `FormatTemplate(template, values)` replaces `{key}`. |
| Random | `Random(length)`. It is not secure. |

- `Contains` is a plain find, not a pattern.
- `ToTitleCase` lower cases the rest of each word.
- `ToSnakeCase` turns spaces and hyphens into `_`, and does not split camelCase.
- The suffix counts toward `max` in `Truncate`.

`RemoveWhitespace`, `ToTitleCase`, `EscapeRichText`, `StripRichText` and `FormatTemplate` return a string and a count. Wrap the call in parentheses when it is the last argument of another call, or the count is passed along too.

### TableUtil

These return a new table:

| Function | Note |
| --- | --- |
| `DeepClone` | A table with a metatable is copied by reference. |
| `Merge(...)` | Shallow. |
| `DeepMerge(...)` | Arrays merge index by index. |
| `Except(first, second)` | Filters an array by value, or a dictionary by key. |
| `Keys` | |
| `Values` | |

These only read:

| Function | Note |
| --- | --- |
| `IsEmpty` | |
| `Count` | |
| `HasKey` | |
| `GetOrDefault` | |
| `FindKeyByValue` | |
| `EqualDeep` | Deep equality, safe on cycles. |

These change the table:

| Function | Note |
| --- | --- |
| `RemoveValue` | Leaves holes in an array. |
| `RemoveWhere(dictionary, predicate)` | |
| `SafeRemove(array, value)` | Removes the first match, and shifts the rest down. |
| `SafeRemoveAll(array, value)` | Removes every match. |

Next: [PrintUtil and Inspect](/docs/craftsman-kit/kit-print-and-inspect)

---

## Page: /docs/craftsman-kit/kit-print-and-inspect

**PrintUtil and Inspect.** Print tables and render any value as text.

```lua
PrintUtil:ListPrint("Inventory", { "Item", "Qty" }, { { "Sword", 1 }, { "Potion", 12 } })
print(Craftsman.Inspect(state, { Pretty = true, MaxDepth = 3 }))
```

### PrintUtil

Call both with `:`.

| Call | What it does |
| --- | --- |
| `PrintUtil:ListPrint(title?, columns?, rows?, max_width?)` | Prints a table. |
| `PrintUtil:PrintObject(object)` | Prints the fields and values of an object. |

`rows` is a list of lists. It can also be a dictionary: each key becomes the first column, and the other columns read the fields of the row by header name.

### Inspect

The module is the function: `Inspect(value, settings?)` returns a string of Luau like text.

| Setting | What it does |
| --- | --- |
| `Pretty` | Uses several lines. |
| `Indent` | The indent. |
| `MaxDepth` | How deep it goes. |
| `MaxItems` | How many items it shows. |
| `SortKeys` | Sorts keys. |
| `Precision` | Float precision in datatypes. |
| `FullName` | Shows the full name of an instance. |
| `ProperFullName` | On by default. |
| `ClassName` | Shows the class of an instance. |
| `Metamethods` | Shows metamethods. |
| `Semicolons` | Uses semicolons. |
