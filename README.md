# robbiegal-tools

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces).

```
/plugin marketplace add robbiegal/plugin-marketplace
/plugin install presentation-coach@robbiegal-tools
/presentation-coach:coach-doctor
```

The marketplace is named `robbiegal-tools`, not after this repository — that is
the name you install against, and it is registered once per user. The plugins
themselves live in their own repositories; this repo is only the catalogue.

## Plugins

| Plugin | What it does |
|---|---|
| [presentation-coach](https://github.com/robbiegal/presentation-coach) | Audits a rehearsal recording against its slide deck — timing, pace, fillers, figures the speaker got wrong, slides never mentioned, and vocal delivery — and tracks progress across dry runs. |

## Notes for me, later

- Adding a plugin: append an entry to `.claude-plugin/marketplace.json`. Users
  pick it up with `/plugin marketplace update`.
- **Renaming or removing one needs a top-level `renames` map**, or existing
  users get `plugin-not-found` rather than a migration.
- Whether an update reaches anyone is decided by version resolution: an
  explicit `version` in the plugin's own `plugin.json` wins, and a stale one
  silently blocks every update.
