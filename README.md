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

`assets/` and the plugin `README.md` belong to connection plugins, the ones
that ship an `mcp.json`. A skills-only plugin such as `web-research` needs
only `plugin.json` and its skills; Myndr falls back to a monogram icon.

## Myndr extension fields

`extensions["ai.myndr"]` carries three keys:

- `interface`: `displayName`, `shortDescription`, `longDescription`,
  `category`, `icon` and `icon_dark` (`./` paths inside the package). The app
  loader accepts SVG or PNG, and a client missing one icon uses the other.
  Packages in this repository ship a self-contained SVG at `assets/icon.svg`
  and `assets/icon-dark.svg`: no scripts, no `on*` event handlers, no external
  references. A black mark needs a distinct dark-background variant, not a
  copy of the light file.
- `mcp`: per-server secret declarations, keyed by the `mcp.json` server key,
  for servers that take an API key, for example
  `"acme": {"secret": {"label": "Acme API key", "header": "Authorization",
  "format": "Bearer {secret}"}}`. Use `header` for a remote server and `env`
  for a stdio one.
- `hooks`: hook definitions, which run only once the user has approved them.

`mcp.json` holds only `$schema` and `mcpServers`, never credentials, and a
server URL must be https (loopback excepted). Myndr picks the sign-in for a
server in this order: a Myndr connection whose provider owns the server's
address, else a secret declared under `mcp`, else the server's own OAuth, else
none.

Skills name tools as `<server key>__<tool>`, the name the model sees, where
the server key is the key under `mcpServers`. A skill may name only real tools
of its own server, and a connection plugin's skill names at least one. When
two or more accounts are connected and the agent has none pinned, Myndr adds a
`myndr_account` argument to each tool of that server; a skill that talks about
choosing an account names `myndr_account`, never a bare `account`.

## Marketplace entry fields

Standard fields: `name`, `description`, `category`, `version`, `source`,
`interface.displayName`, `interface.shortDescription`. The `version` must equal
the one in the plugin's `plugin.json`.

Myndr honors three extra fields on this repository only (they are ignored on
any third-party marketplace):

| Field | Effect |
| --- | --- |
| `featured` | eligible for the Popular row on the Plugins page |
| `rank` | order within Popular, descending — a higher rank is shown first; ranks of featured plugins are unique |
| `blocked` + `blocked_reason` | kill switch: the app refuses to install or load the plugin and shows the reason |

## Adding a plugin

1. Create `plugins/<name>/` with a root `plugin.json` and at least one skill or
   an `mcp.json`. A plugin with an `mcp.json` also gets a `README.md` and
   `assets/` icons.
2. Add the entry to `.agents/plugins/marketplace.json` with a
   `{"source": "local", "path": "./plugins/<name>"}` source.
3. Open a pull request. Plugin names are permanent once shipped — renaming one
   requires a `renames` entry in the marketplace manifest.

## Logos

Connection plugins ship the service's official mark, unmodified, from the
owner's brand resources. The plugin `README.md` names the source URL and
carries a `Trademark` notice (the word is checked, capital T included).

## License

The repository is MIT licensed (see `LICENSE`). Two things are not covered by
it: the logo files, since each mark stays the property of its owner, and
vendored skills, which keep the upstream `LICENSE` in their own directory (the
Notion skills are MIT, copyright Notion).

## Validation

```
MYNDR_PLUGINS_REPO=/absolute/path/to/myndr-plugins \
  go test ./v2/internal/plugins -run 'TestFirstParty'
```

Run it in the Myndr repository's `local-backend`; it loads every package with
the desktop's own loader. The path must be absolute, because the test runs
inside the package directory.

For every plugin the test checks that the package loads clean, the Agent
Skills name rule holds, featured ranks are unique and no removed tool is
named. The pinned first-party packages are checked in depth: server URL and
headers, icons, README, tool names and skill names. A new connection plugin
needs a row in the pin table of `firstparty_packages_test.go`
(`local-backend/v2/internal/plugins/` in the Myndr repository) to get that
coverage.
