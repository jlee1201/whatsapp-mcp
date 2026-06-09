# Fork notes

This is `jlee1201/whatsapp-mcp`, a fork of [`lharries/whatsapp-mcp`](https://github.com/lharries/whatsapp-mcp)
used as a git submodule by the `dealia` repo (the Dealia family bot).

## Local patches on top of upstream

- **whatsmeow context API compat** -- pass `context.Background()` to `Download`,
  `sqlstore.New`, and `GetFirstDevice` (upstream whatsmeow added required `ctx` args).
- **REST API on `:48089`** (was `:8080`) -- avoids colliding with other local services;
  `whatsapp.py` points at the same port.
- **Headless QR auth** -- the bridge also writes the pairing QR string to
  `/tmp/wa-qr-string.txt` and extends the scan timeout to 10 minutes, so the bot host
  can be paired without an attached terminal.
- **`list_chats` query refactor** in `whatsapp.py`.
- **gitignore** the runtime `whatsapp-bridge/store/` (SQLite message history + auth keys)
  and the compiled `whatsapp-bridge` binary so they never get committed.

## Why a fork

Upstream is read-only for us, and these patches must travel with the bot across machines.
Both Macs' submodule remote points here; the dealia auto-pull keeps the bot host in sync.

When you commit in this submodule on the new Mac, the dealia bump-guard
(`tools/scripts/submodule-post-commit-bump.sh`) auto-pushes the commit and bumps
the superproject gitlink, so the change reaches the bot host automatically.
