# Mealie add-on

Custom Home Assistant OS add-on that wraps the official Mealie image
(`ghcr.io/mealie-recipes/mealie:latest`). Single container, sqlite backend.

## Install

1. Create a new GitHub repo (e.g. `ha-addons`) and upload `repository.yaml`
   plus the whole `mealie/` folder to its root. Update the `url:` field in
   `repository.yaml` to your repo's URL.
2. In Home Assistant: **Settings → Add-ons → Add-on Store → ⋮ (top right) →
   Repositories** → paste your repo URL → Add.
3. Mealie appears in the store. **Install** it (first build pulls the Mealie
   image, takes a few minutes on a Pi).
4. **Start** the add-on, turn on **Start on boot**, then **Open Web UI**
   (port 9925 on your HA host).
5. Register your admin account. Signup is closed to the public, but the
   very first account can always register — that becomes the admin.
6. Create a household (e.g. "Bergin"), then add Molly via user management.

## Updating Mealie

Bump `version:` in `mealie/config.yaml`, commit, and HA will offer the
add-on update. The rebuild pulls the latest Mealie image.

**Before updating:** take a Mealie backup from inside the app
(Settings → Backups → download the file). Recipes live in the add-on's
`/data` (included in HA backups), but the in-app backup is your safety net —
restore it after the update if anything looks off.

## Notes

- `ALLOW_SIGNUP=false` is baked in. You add users yourself from the admin
  panel; nobody can self-register, which matters once this is behind a
  public Cloudflare hostname.
- `BASE_URL` defaults to `http://homeassistant.local:9925`. It only affects
  generated links (shares, notifications), not the UI itself. If you want it
  to match your tunnel hostname, edit the Dockerfile `ENV` line and bump
  the version to rebuild.
- Phone access: Tailscale to `http://<ha-host>:9925`, or the Mealie mobile
  app with server URL + API token.
