# The console

An agent cannot use the console. It asks for a passkey, and nothing in the CLI reads a player's document or runs a command. So when the work is here, hand the person these steps. Pages are at `/dashboard/<project id>/<page>`, and the place group menu at the top decides which group a page acts on.

Full steps are in `docs-bundle.md` under each path.

## Where things are

| Page | What a person does there | Page in the bundle |
|---|---|---|
| **Overview** | Reads **Needs attention**, drawn only when something is wrong. Halts operations | `/docs/monitoring/read-the-overview` |
| **Configuration** | Switches features, changes configs, uses the fallback | `/docs/features/change-a-config` |
| **Players & Data** | Opens a player or a document, bans, runs operations, reverts, investigates | `/docs/players-and-data/find-a-player` |
| **Servers** | Lists live servers. Needs `Registry = true` | `/docs/servers-and-commands/see-live-servers` |
| **Commands** | Sends a command to one server or every server | `/docs/servers-and-commands/commands-page` |
| **Purchases** | Reads unfulfilled purchases and refunds, assigns a gift | `/docs/store-and-purchases/look-up-purchases` |
| **Privacy** | Handles erasure requests | `/docs/privacy/erasure-requests` |
| **Releases** | Tests and deploys builds | `/docs/releases/deploy-a-release` |
| **Store**, **Assets** | Read only mirrors of what the repository uploaded | `/docs/assets/assets` |
| **Health** | Reads which key permissions are proven, and rate limits | `/docs/monitoring/check-health` |
| **Audit** | Reads every change, by person, action and source | `/docs/monitoring/search-the-audit-log` |
| **Approvals** | Approves, declines or cancels what is waiting | `/docs/team-and-access/approve-a-request` |
| **Settings** | Universes and keys, place groups, members, roles, release settings, signing, the webhook | `/docs/project-settings/add-a-universe` |

The sidebar shows a page only if the person may read it.

## What each action asks of a person

| Action | Asks |
|---|---|
| Switch a feature off | Permission only |
| Switch a feature on | A passkey |
| Change a config | Permission. The save may ask for a passkey |
| Use the fallback on a group | Permission only |
| Stop the fallback | A passkey |
| Halt operations | Craftsman Staff or Superpower, no passkey |
| Resume operations | A passkey |
| Override one server | `craftsman.config.override`, no passkey. Ends when the server restarts |
| Run a `routine` or `valued` operation | Permission, and a passkey if it is marked so |
| Run a `structural` operation | A passkey and 15 minutes, and a second approver if the operation asks and someone else can give one |
| Edit or revert a document | `craftsman.data.edit` and a passkey. No delay. A second approver if someone else can give one |
| Ban or lift a ban | `craftsman.player.restrict` and a passkey |
| Deploy to a protected group | `craftsman.releases.deploy.protected`, a passkey and a passing test |
| Deploy to an open group | `craftsman.releases.deploy.open` |

A passkey tap covers 10 minutes in that browser. It does not extend.

## When a result comes back

| Word | Means | Tell them |
|---|---|---|
| `applied` | It is done | Nothing |
| `refused` | A decision: the rules said no, and nothing changed | Read the reason |
| `failed` | Nobody knows if it applied | Check before sending again. **Send as a new operation** can apply twice |
| `queued` | Sent to servers, and none has answered | Wait |
| `waiting` | A durable command, and the player is not here. Can last days | Wait |
| `delayed` | A structural operation, in its delay | Wait, or cancel on **Approvals** |

## In an incident

Smallest first. None of these stops the game from running.

| Problem | Brake | Where |
|---|---|---|
| One feature is broken | Switch it off | **Configuration**, or `/feature` in Discord |
| A hosted value is bad | Use the fallback | **Configuration**, or `/fallback` in Discord |
| Operations are doing harm | Halt all operations | **Overview**, or `/halt-ops` in Discord |
| An account is compromised | Freeze it | **Settings**, **Members**, or `/freeze` in Discord |

Pass `place_group` in Discord. Without it, a command acts on the first group, usually Development.

## A player's data went wrong

Do not change anything first. In this order:

1. Open the document on **Players & Data**, then **Investigate**. It reads only when a button is pressed.
2. Press **Compare** with **Changes around** set to the moment, or **Whole history**. **Also in this window** lists deploys, config edits and operations.
3. **Copy for LLM** gives you a redacted brief. It asks for a passkey, and the audit records it. Ask the person to paste it.
4. The fix is an operation, or a revert a person requests. A log entry cannot be removed.

There is no verdict in the console, and a rate is an average over a gap nobody can see inside. A real DataStore keeps at most one version per key per hour.
