# Build as a custom HAOS add-on wrapping the upstream Mealie image.
#
# Why this shape:
# - HAOS doesn't let you run arbitrary compose stacks; add-ons are the
#   supported path. This repo gives you a one-click Mealie install in the
#   add-on store.
# - Mealie keeps its data in /app/data inside the container. The symlink
#   points it at /data, which is the add-on's persistent directory (it
#   survives restarts and is included in HA backups). If the upstream image
#   declares /app/data as a VOLUME, Docker will mount an anonymous volume
#   over the symlink instead — harmless, but then you MUST take a Mealie
#   in-app backup (Settings -> Backups) before updating the add-on.
# - sqlite keeps this a single container. Plenty for family use.
FROM ghcr.io/mealie-recipes/mealie:latest

RUN rmdir /app/data && ln -s /data /app/data

ENV DB_ENGINE=sqlite \
    ALLOW_SIGNUP=false \
    TZ=America/New_York \
    PUID=1000 \
    PGID=1000 \
    BASE_URL=http://homeassistant.local:9925
