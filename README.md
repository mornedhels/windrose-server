# Windrose-Dedicated-Server

[![Docker Pulls](https://img.shields.io/docker/pulls/mornedhels/windrose-server.svg)](https://hub.docker.com/r/mornedhels/windrose-server)
[![Docker Stars](https://img.shields.io/docker/stars/mornedhels/windrose-server.svg)](https://hub.docker.com/r/mornedhels/windrose-server)
[![Docker Image Size (tag)](https://img.shields.io/docker/image-size/mornedhels/windrose-server/latest)](https://hub.docker.com/r/mornedhels/windrose-server)
[![GitHub](https://img.shields.io/github/license/mornedhels/windrose-server)](https://github.com/mornedhels/windrose-server/blob/main/LICENSE)

[![GitHub](https://img.shields.io/badge/Repository-mornedhels/windrose--server-blue?logo=github)](https://github.com/mornedhels/windrose-server)

Docker image for the game Windrose. The image is based on the [steamcmd](https://hub.docker.com/r/cm2network/steamcmd/)
image and uses supervisor to handle startup, automatic updates and cleanup.

> [!WARNING]
> Under construction

## Environment Variables

### Server Settings

| Variable                   | Required | Default | Contraints  | Description                                                                    | WIP |
|----------------------------|:--------:|---------|-------------|--------------------------------------------------------------------------------|:---:|
| `SERVER_NAME`              |          |         | string      | The name of the server                                                         | ⚠️  |
| `SERVER_PASSWORD`          |          |         | string      | Server password. Also sets `IsPasswordProtected` (non-empty=true, empty=false) | ⚠️  |
| `SERVER_INVITE_CODE`       |          |         | string      | The invite code for the server                                                 | ⚠️  |
| `SERVER_MAX_PLAYER_COUNT`  |          |         | integer     | Maximum number of players                                                      | ⚠️  |
| `SERVER_P2P_PROXY_ADDRESS` |          |         | string (ip) | P2P proxy address                                                              | ⚠️  |

> [!NOTE]
> Any `SERVER_*` environment variable is automatically mapped to the corresponding
> `ServerDescription_Persistent` field in `ServerDescription.json` using PascalCase conversion (e.g.,
> `SERVER_MY_SETTING` → `MySetting`). The variables `PersistentServerId`, `WorldIslandId`, and `IsPasswordProtected` are
> excluded from this mapping. If an environment variable is not set, the corresponding field is left unchanged.

### World Settings

| Variable                                              | Required | Default | Contraints | Description                            | WIP |
|-------------------------------------------------------|:--------:|---------|------------|----------------------------------------|:---:|
| `WORLD_NAME`                                          |          |         | string     | The world name                         | ⚠️  |
| `WORLD_PRESET_TYPE`                                   |          |         | string     | World preset type                      | ⚠️  |
| `WORLD_SETTINGS_COOP__SHARED_QUESTS`                  |          |         | boolean    | Enable shared quests                   | ⚠️  |
| `WORLD_SETTINGS_EASY_EXPLORE`                         |          |         | boolean    | Enable easy explore mode               | ⚠️  |
| `WORLD_SETTINGS_MOB_HEALTH_MULTIPLIER`                |          |         | float      | Mob health multiplier                  | ⚠️  |
| `WORLD_SETTINGS_MOB_DAMAGE_MULTIPLIER`                |          |         | float      | Mob damage multiplier                  | ⚠️  |
| `WORLD_SETTINGS_SHIPS_HEALTH_MULTIPLIER`              |          |         | float      | Ships health multiplier                | ⚠️  |
| `WORLD_SETTINGS_SHIPS_DAMAGE_MULTIPLIER`              |          |         | float      | Ships damage multiplier                | ⚠️  |
| `WORLD_SETTINGS_BOARDING_DIFFICULTY_MULTIPLIER`       |          |         | float      | Boarding difficulty multiplier         | ⚠️  |
| `WORLD_SETTINGS_COOP__STATS_CORRECTION_MODIFIER`      |          |         | float      | Coop stats correction modifier         | ⚠️  |
| `WORLD_SETTINGS_COOP__SHIP_STATS_CORRECTION_MODIFIER` |          |         | float      | Coop ship stats correction modifier    | ⚠️  |
| `WORLD_SETTINGS_COMBAT_DIFFICULTY`                    |          |         | string     | Combat difficulty (Normal, Hard, etc.) | ⚠️  |

> [!NOTE]
> Any `WORLD_*` environment variable is automatically mapped to the corresponding field in
> `WorldDescription.json`. Top-level fields use PascalCase conversion (e.g., `WORLD_PRESET_TYPE` → `WorldPresetType`).
> The fields `islandId` and `CreationTime` are excluded from this mapping.

### General Settings

| Variable           | Required | Default     | Contraints           | Description                                                                                                        | WIP |
|--------------------|:--------:|-------------|----------------------|--------------------------------------------------------------------------------------------------------------------|:---:|
| `PUID`             |          | `4711`      | integer              | The UID to run server as (file permission)                                                                         |     |
| `PGID`             |          | `4711`      | integer              | The GID to run server as (file permission)                                                                         |     |
| `UPDATE_CRON`      |          |             | string (cron format) | Update game server files cron (eg. `*/30 * * * *` check for updates every 30 minutes)                              |     |
| `BACKUP_CRON`      |          |             | string (cron format) | Backup game server files cron (eg. `*/15 * * * *` backup saves every 15 minutes) - don't set cron under 10 minutes |     |
| `BACKUP_DIR`       |          | `./backups` | string               | Folder for backups (relative and absolute paths are supported)                                                     |     |
| `BACKUP_MAX_COUNT` |          | `0`         | integer              | Number of backups to keep (0 means infinite)                                                                       |     |
| `RESTART_CRON`     |          |             | string (cron format) | Restart game server cron (eg. `0 3 * * *` restart server daily at 3)                                               |     |
| `GAME_BRANCH`      |          | `public`    | string               | Steam branch (eg. testing) of the Enshrouded server                                                                |     |
| `STEAMCMD_ARGS`    |          | `validate`  | string               | Additional steamcmd args for the updater                                                                           |     |

⚠️: Work in Progress

### Additional Information

* During the update process, the container temporarily requires more disk space (up to 2x the game size).

### Hooks

| Variable            | Description                            | WIP |
|---------------------|----------------------------------------|:---:|
| `BOOTSTRAP_HOOK`    | Command to run after general bootstrap |     |
| `UPDATE_PRE_HOOK`   | Command to run before update           |     |
| `UPDATE_POST_HOOK`  | Command to run after update            |     |
| `BACKUP_PRE_HOOK`   | Command to run before backup & cleanup |     |
| `BACKUP_POST_HOOK`  | Command to run after backup & cleanup  |     |
| `RESTART_PRE_HOOK`  | Command to run before server restart   |     |
| `RESTART_POST_HOOK` | Command to run after server restart    |     |

The scripts will wait for the hook to resolve/return before continuing.

⚠️: Work in Progress

## Image Tags

| Tag                | Virtualization | Description                              |
|--------------------|----------------|------------------------------------------|
| `latest`           | proton         | Latest image based on proton             |
| `<version>`        | proton         | Pinned image based on proton (>= 1.x.x)  |
| `stable-proton`    | proton         | Same as latest image                     |
| `<version>-proton` | proton         | Pinned image based on proton             |
| `stable-wine`      | wine           | Latest image based on wine               |
| `<version>-wine`   | wine           | Pinned image based on wine               |
| `dev-proton`       | proton         | Dev build based on proton                |
| `dev-wine`         | wine           | Dev build based on wine                  |
| `dev-wine-staging` | wine           | Dev build based on wine (staging branch) |

## Ports (default)

TBD

## Volumes

| Volume        | Description                      |
|---------------|----------------------------------|
| /opt/windrose | Game files (steam download path) |

**Note:** By default the volumes are created with the UID and GID 4711 (that user should not exist). To change this, set
the environment variables `PUID` and `PGID`.

## Recommended System Requirements

* CPU: >= 2 cores
* RAM: >= 8 GB
* Disk: >= 35 GB (preferably SSD)

**[Official Docs](https://playwindrose.com/dedicated-server-guide/#wsg-faq)**

## Usage

### Docker

```bash
docker run -d --name windrose \
  --hostname windrose \
  --restart=unless-stopped \
  -v ./game:/opt/windrose \
  -e SERVER_NAME="Windrose Server" \
  -e UPDATE_CRON="*/30 * * * *" \
  -e PUID=4711 \
  -e PGID=4711 \
  mornedhels/windrose-server:latest
```

### Docker Compose

```yaml
services:
  windrose:
    image: mornedhels/windrose-server:latest
    container_name: windrose
    hostname: windrose
    restart: unless-stopped
    stop_grace_period: 90s
    volumes:
      - ./game:/opt/windrose
    # only add ntsync device if your kernel supports it (6.14 or newer)
    devices:
      - /dev/ntsync:/dev/ntsync
    environment:
      - SERVER_NAME=Windrose Server
      - UPDATE_CRON=*/30 * * * *
      - PUID=4711
      - PGID=4711
```

**Note:** The volumes are created next to the docker-compose.yml file. If you want to create the volumes, in the default
location (eg. /var/lib/docker) you can use the following compose file:

```yaml
services:
  windrose:
    image: mornedhels/windrose-server:latest
    container_name: windrose
    hostname: windrose
    restart: unless-stopped
    stop_grace_period: 90s
    volumes:
      - game:/opt/windrose
    # only add ntsync device if your kernel supports it (6.14 or newer)
    devices:
      - /dev/ntsync:/dev/ntsync
    environment:
      - SERVER_NAME=Windrose Server
      - UPDATE_CRON=*/30 * * * *
      - PUID=4711
      - PGID=4711

volumes:
  game:
```

## Backup

The image includes a backup script that creates a zip file of the last saved game state. To enable backups, set
the `BACKUP_CRON` environment variable. To limit the number of backups, set the `BACKUP_MAX_COUNT` environment variable.

To restore a backup, stop the server and simply extract the zip file to the savegame folder and start the server up
again. If you want to keep the current savegame, make sure to make a backup before deleting or overwriting the files.

> [!WARNING]  
> Verify the permissions of the extracted files. The files should be owned by the user with the UID and GID set in the
> environment variables. If the image is running in privileged mode, the files will be automatically chowned to the
> given `UID` and `GID`.

## Commands

* **Force Update:**
  ```bash
  docker compose exec windrose supervisorctl start windrose-force-update
  ```
* **Restart Windrose Server:** (can be used for periodic restarts)
  ```bash
  docker compose exec windrose supervisorctl restart windrose-server
  ```

## Known Issues
