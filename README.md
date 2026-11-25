# robbiegal-tools

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces).

```
/plugin marketplace add robbiegal/plugin-marketplace
/plugin install cutlist@robbiegal-tools
/cutlist:cutlist-doctor
```

Each plugin ships a doctor. Run it first — they all depend on things they
cannot install for you (external binaries, and in tamlul's case multi-GB
models), and the doctor is what tells you which one is missing instead of a
render or a transcription failing halfway.

The marketplace is named `robbiegal-tools`, not after this repository — that is
the name you install against, and it is registered once per user. The plugins
themselves live in their own repositories; this repo is only the catalogue.

## Plugins

| Plugin | What it does |
|---|---|
| [cutlist](https://github.com/robbiegal/cutlist) | Cuts, grades, annotates, redacts and mixes a finished video from raw footage using ffmpeg, then proves the delivered file frame by frame — because an encoder exits 0 on a render that is entirely black. |
| [presentation-coach](https://github.com/robbiegal/presentation-coach) | Audits a rehearsal recording against its slide deck — timing, pace, fillers, figures the speaker got wrong, slides never mentioned, and vocal delivery — and tracks progress across dry runs. |
| [tamlul](https://github.com/robbiegal/tamlul) | Transcribes and annotates Hebrew conversations on the machine they were recorded on — speaker-attributed, timestamped and verbatim, with turns, overlaps, action items and summaries. No audio leaves the machine. |

## Why the source is an explicit https URL

A `github` shorthand source (`{"source": "github", "repo": "owner/name"}`)
clones over **SSH by default**. On a machine with no SSH key — which is most
machines, including ones where `gh` is authenticated and every other git
operation works — the marketplace adds cleanly and then the plugin install
fails with `Permission denied (publickey)`. That is a confusing failure,
because the marketplace step succeeded moments earlier by falling back to
HTTPS on its own.

The user-side fix is `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`, but requiring an
environment variable to install is not a reasonable ask. An explicit `url`
source clones over HTTPS for everyone, and picks up `gh`'s credential helper
for private repositories.

## Notes for me, later

- Adding a plugin: append an entry to `.claude-plugin/marketplace.json`. Users
  pick it up with `/plugin marketplace update`.
- **Renaming or removing one needs a top-level `renames` map**, or existing
  users get `plugin-not-found` rather than a migration.
- Whether an update reaches anyone is decided by version resolution: an
  explicit `version` in the plugin's own `plugin.json` wins, and a stale one
  silently blocks every update.
