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
.agents/plugins/marketplace.json   the catalog the app fetches
plugins/<name>/plugin.json         one package per directory
plugins/<name>/skills/<skill>/SKILL.md
```

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

1. Create `plugins/<name>/` with a root `plugin.json` and at least one skill.
2. Add the entry to `.agents/plugins/marketplace.json` with a
   `{"source": "local", "path": "./plugins/<name>"}` source.
3. Open a pull request. Plugin names are permanent once shipped — renaming one
   requires a `renames` entry in the marketplace manifest.
