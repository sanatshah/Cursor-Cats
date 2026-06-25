# AGENTS.md

## Cursor Cloud specific instructions

Cursor Cats is a single **Electron desktop app** (no backend services, no database). It is a frameless, transparent, always-on-top overlay "desktop pet" powered by the Cursor SDK (`@cursor/sdk`).

### Running

- Dev: `npm run dev` (runs `electron-vite dev` — builds main/preload and serves the renderer on a fixed port `56247`). See `package.json` scripts for `build`/`start`/`prepare`.
- The cloud VM is headless Linux but already has an X server on `DISPLAY=:1` (Xvfb), so the Electron GUI launches without extra setup. Electron is a long-running foreground process — start it in a tmux/background session, not a blocking one-shot.
- Startup logs include benign `bus.cc` (DBus), `viz_main_impl`/`command_buffer_proxy` (GPU), and `gtk_widget_add_accelerator` (GTK) errors. These are expected in headless Linux and do not indicate failure — the app still runs.

### Demonstrating / using the UI

- The main overlay window is transparent and full-screen, so it looks invisible until a cat is spawned. The primary interactive UI is the **"New Cat" modal**, opened with the global shortcut **Ctrl+Shift+C** (Cmd+Shift+C on macOS). The shortcut works under Xvfb.
- The modal has a task prompt, an API key field, and `Local` / `Cloud` / `Cats` tabs with a `Spawn` button.

### CURSOR_API_KEY (needed for live agent runs)

- Spawning a real cat (an actual SDK agent run) requires `CURSOR_API_KEY`. When it is not set, the New Cat modal blocks `Spawn` and prompts for the key, and any cat would "appear briefly and disappear". The app itself launches and the full modal UI works without the key.
- Provide `CURSOR_API_KEY` as an environment variable / secret to exercise cat agents end-to-end.

### Tests / lint

- There are no test or lint scripts defined in `package.json`; there is no test framework or ESLint/Prettier config in the repo.
