# myndr-plugins

First-party plugin marketplace for [Myndr AI](https://myndr.ai). The desktop app
reads `.agents/plugins/marketplace.json` from this repository and lists every
entry on the Plugins page.

Packages follow the [Agent Plugins 1.0.0](https://agent-plugins.org) standard:
a root `plugin.json` carrying the standard `$schema`, skills at
`skills/<name>/SKILL.md`. Myndr-specific presentation lives under the
`extensions["ai.myndr"]` key, so the same package loads in any client that
implements the standard.

## Layout

```
.agents/plugins/marketplace.json          the catalog the app fetches
plugins/<name>/plugin.json                one package per directory (Agent Plugins 1.0.0)
plugins/<name>/mcp.json                   MCP servers, Agent Plugins 1.0.0 format (optional)
plugins/<name>/skills/<skill>/SKILL.md    Agent Skills; frontmatter name equals the directory
plugins/<name>/assets/icon.svg            logo for light backgrounds
plugins/<name>/assets/icon-dark.svg       logo for dark backgrounds
plugins/<name>/README.md                  what it does, skill provenance, logo source and trademark notice
```

## Myndr extension fields

`extensions["ai.myndr"].interface` carries `displayName`, `shortDescription`, `longDescription`, `category`,
`icon` and `icon_dark` (`./` paths inside the package, SVG or PNG). A client missing one icon uses the other.
`mcp.json` never carries credentials: Myndr picks the sign-in for a server from its address (a Myndr
connection, a secret the user enters, or the server's own OAuth).

Skills name tools as `<server key>__<tool>`, the name the model sees. When two or more accounts are connected and
the agent has none pinned, Myndr adds a `myndr_account` argument to each tool of that server; a skill that talks
about choosing an account names `myndr_account`, never a bare `account`.

## Marketplace entry fields

Standard fields: `name`, `description`, `category`, `version`, `source`,
`interface.displayName`, `interface.shortDescription`.

Myndr honors three extra fields on this repository only (they are ignored on
any third-party marketplace):

| Field | Effect |
| --- | --- |
| `featured` | eligible for the Popular row on the Plugins page |
| `rank` | order within Popular, descending — a higher rank is shown first |
| `blocked` + `blocked_reason` | kill switch: the app refuses to install or load the plugin and shows the reason |

## Adding a plugin

1. Create `plugins/<name>/` with a root `plugin.json`, a `README.md`, and at least one
   skill or an `mcp.json`.
2. Add the entry to `.agents/plugins/marketplace.json` with a
   `{"source": "local", "path": "./plugins/<name>"}` source.
3. Open a pull request. Plugin names are permanent once shipped — renaming one
   requires a `renames` entry in the marketplace manifest.

## Logos

Connection plugins ship the service's official mark, unmodified, from the owner's brand resources. The plugin
README names the source URL and carries the trademark notice. The repository is MIT licensed (see `LICENSE`);
the logo files are the exception, since each mark stays the property of its owner and is not covered by
that license.

## Validation

`MYNDR_PLUGINS_REPO=<this checkout> go test ./v2/internal/plugins -run 'TestFirstParty'`
in the Myndr repository's `local-backend` loads every package with the desktop's own loader.
