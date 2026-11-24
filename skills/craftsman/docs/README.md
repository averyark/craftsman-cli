# Reading the Craftsman docs

The docs are read through **`craftsman mcp`**, never the website. The site turns agents away,
and a fetch of `/docs/...` returns an empty page anyway. The docs tools need no sign-in.

## Connect once

```bash
claude mcp add craftsman -- craftsman mcp
```

Use the same command in any MCP client's settings. It needs the `craftsman` CLI, version
0.11.17 or later (`craftsman --version`). Without the server, ask the person to add it, and
until then work from the source in `CraftsmanPackages/`.

## The tools

| Tool | Does |
|---|---|
| `search_docs` | Pages whose title or summary match a short phrase, by address |
| `list_docs` | The sections and their collections, or every page in one collection |
| `read_doc` | One page as markdown, by its address, such as `/docs/fms/fms-fallback` |

Search with two or three words, then read the page. A page's links are addresses
`read_doc` takes. `index.md`, beside this file, lists every page with its summary, so you
can often go straight to `read_doc`.

## How the docs are laid out

Each system has its own section, and every page in it starts with the system's
abbreviation:

| Section | System | Pages start |
|---|---|---|
| Features and configs | Feature Management System | `/docs/fms/fms-` |
| Players and data | Data Management System | `/docs/dms/dms-` |
| Commands | Change Control System | `/docs/ccs/ccs-` |
| Moderation | Trust and Safety System | `/docs/tss/tss-` |
| Servers | Live Server Registry | `/docs/lsr/lsr-` |
| Releases | Release Management System | `/docs/rms/rms-` |
| Assets | Asset Management System | `/docs/ams/ams-` |
| Store | Purchase and Ownership Records | `/docs/por/por-` |
| Team and access | Identity and Access Management | `/docs/iam/iam-` |
| Privacy | Erasure Request System | `/docs/ers/ers-` |
| Audit log | Action Audit Log | `/docs/aal/aal-` |
| Open Cloud usage | Open Cloud Usage Log | `/docs/ocul/ocul-` |
| Agents | Supervised Agent Access | `/docs/saa/saa-` |

A system's `-overview` page says what it is for. Its feature pages say how to do each
thing. Its `-sdk`, `-ui`, `-api` and `-cli` pages are the reference for each interface.
*Getting started*, *Set up* and *Reference* sit beside the systems, and *Packages* holds
Lifecycle and Craftsman Kit.

When the docs and the source disagree, the source wins, and you say so.
