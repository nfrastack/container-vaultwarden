# nfrastack/container-vaultwarden

## About

This repository will build a container for [Vaultwarden](https://github.com/dani-garcia/vaultwarden). An unofficial Bitwarden compatible server

## Maintainer

- [Nfrastack](https://www.nfrastack.com)

## Table of Contents

- [About](#about)
- [Maintainer](#maintainer)
- [Table of Contents](#table-of-contents)
- [Installation](#installation)
  - [Prebuilt Images](#prebuilt-images)
  - [Quick Start](#quick-start)
  - [Persistent Storage](#persistent-storage)
- [Environment Variables](#environment-variables)
  - [Base Images used](#base-images-used)
  - [Core Configuration](#core-configuration)
- [Users and Groups](#users-and-groups)
  - [Networking](#networking)
- [Maintenance](#maintenance)
  - [Shell Access](#shell-access)
- [Support & Maintenance](#support--maintenance)
- [License](#license)

## Installation

### Prebuilt Images

Feature limited builds of the image are available on the [Github Container Registry](https://github.com/nfrastack/container-vaultwarden/pkgs/container/container-vaultwarden) and [Docker Hub](https://hub.docker.com/r/nfrastack/vaultwarden).

To unlock advanced features, one must provide a code to be able to change specific environment variables from defaults. Support the development to gain access to a code.

To get access to the image use your container orchestrator to pull from the following locations:

```
ghcr.io/nfrastack/container-vaultwarden:(image_tag)
docker.io/nfrastack/vaultwarden:(image_tag)
```

Image tag syntax is:

`<image>:<optional tag>-<optional_distribution>_<optional_distribution_variant>`

Example:

`ghcr.io/nfrastack/container-vaultwarden:latest` or

`ghcr.io/nfrastack/container-vaultwarden:1.0` or optionally

`ghcr.io/nfrastack/container-vaultwarden:1.0-alpine` or optinally

`ghcr.io/nfrastack/container-vaultwarden:alpine`

- `latest` will be the most recent commit
- An optional `tag` may exist that matches the [CHANGELOG](CHANGELOG.md) - These are the safest
- If it is built for multiple distributions there may exist a value of `alpine` or `debian`
- If there are multiple distribution variations it may include a version - see the registry for availability

Have a look at the container registries and see what tags are available.

#### Multi-Architecture Support

Images are built for `amd64` by default, with optional support for `arm64` and other architectures.

### Quick Start

- The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [compose.yml](examples/compose.yml) that can be modified for your use.

- Map [persistent storage](#persistent-storage) for access to configuration and data files for backup.
- Set various [environment variables](#environment-variables) to understand the capabilities of this image.

### Persistent Storage

The following directories are used for configuration and can be mapped for persistent storage.

| Directory | Description |
| --------- | ----------- |
| `/data`   | Data        |
| `/logs`   | Logs        |

### Environment Variables

#### Base Images used

This image relies on a customized base image in order to work.
Be sure to view the following repositories to understand all the customizable options:

| Image                                                   | Description |
| ------------------------------------------------------- | ----------- |
| [OS Base](https://github.com/nfrastack/container-base/) | Base Image  |

Below is the complete list of available options that can be used to customize your installation.

- Variables showing an 'x' under the `Advanced` column can only be set if the containers advanced functionality is enabled.

#### Core Configuration

| Parameter          | Description                                 | Default                     | Advanced |
| ------------------ | ------------------------------------------- | --------------------------- | -------- |
| `SETUP_TYPE`       | Setup mode `AUTO` or `MANUAL`               | `AUTO`                      |          |
| `DATA_PATH`        | Base data directory                         | `/data/`                    |          |
| `ICON_CACHE_PATH`  | Icon cache directory                        | `${DATA_PATH}/icon_cache/`  |          |
| `JWT_PATH`         | JWT Keypair Path`                           | `${DATA_PATH}/jwt/`         |          |
| `JWT_BASE_FILE`    | JWT File base for private/public pair       | `rsa_key`                   |          |
| `ATTACHMENTS_PATH` | Attachments storage directory               | `${DATA_PATH}/attachments/` |          |
| `SENDS_PATH`       | "Sends" directory for temporary send stores | `${DATA_PATH}/sends/`       |          |
| `TMP_PATH`         | Temporary working directory                 | `${DATA_PATH}/tmp/`         |          |
| `TEMPLATES_PATH`   | Templates directory                         | `${DATA_PATH}/templates/`   |          |

#### Web Interface

| Parameter                    | Description                                                           | Default           | Advanced | `_FILE` |
| ---------------------------- | --------------------------------------------------------------------- | ----------------- | -------- | ------- |
| `ENABLE_WEBVAULT`            | Serve the included Web Vault static files                             | `TRUE`            |          |         |
| `WEBVAULT_ENABLE_WEBSOCKETS` | Enable websocket support                                              | `TRUE`            |          |         |
| `WEBVAULT_ADMIN_PASS`        | Admin password (enables `/admin` endpoint)                            |                   |          | x       |
| `LISTEN_IP`                  | IP for Vaultwarden to listen on                                       | `0.0.0.0`         |          |         |
| `LISTEN_PORT`                | Port for Vaultwarden to listen on                                     | `8000`            |          |         |
| `WEBVAULT_PATH`              | Path to the web vault files in the container                          | `/app/web-vault/` |          |         |
| `WEBVAULT_DOMAIN`            | Domain for the web vault                                              |                   |          |         |
| `ENABLE_NGINX`               | Have nginx listen and forward to Web Interface for more customization | `FALSE`           |          |         |
| `WEBVAULT_ADMIN_ALLOWED_IPS` | Comma seperated list of IPs/Networks to allow to /admin endpoint      | `0.0.0.0/0`       |          |         |
|                              | This only works with `ENABLE_NGINX=TRUE`                              |                   |          |         |


#### Database Configuration

| Parameter                      | Description                                                  | Default              | Advanced | `_FILE` |
| ------------------------------ | ------------------------------------------------------------ | -------------------- | -------- | ------- |
| `DB_TYPE`                      | Database Type for storage `sqlite`, `mysql`, `postgresql`)   | `sqlite`             |          | x       |
| `DB_HOST`                      | (mysql/postgres) Hostname of DB Server e.g. `vaultwarden-db` |                      |          | x       |
| `DB_NAME`                      | (mysql/postgres) Database name e.g. `vaultwarden`            |                      |          | x       |
| `DB_PORT`                      | (mysql/postgres) Database port e.g. `3306`                   |                      |          | x       |
| `DB_USER`                      | (mysql/postgres) Username for Database e.g. `vaultwarden`    |                      |          | x       |
| `DB_PASS`                      | (mysql/postgres) Password for Database e.g. `password`       |                      |          | x       |
| `DB_SQLITE_PATH`               | Directory for SQLite DB file                                 | `${DATA_PATH%/}/db/` |          |         |
| `DB_SQLITE_NAME`               | SQLite file name                                             | `db.sqlite`          |          |         |
| `DB_SQLITE_ENABLE_WAL`         | Enable SQLite WAL (Write-Ahead Logging)                      | `true`               |          |         |
| `DB_SQLITE_CONNECTION_RETRIES` | Number of retries when connecting to DB                      | `15`                 |          |         |
| `DB_SQLITE_TIMEOUT_DB`         | Database connect timeout (seconds)                           | `30`                 |          |         |
| `DB_SQLITE_TIMEOUT_IDLE`       | Database idle timeout (seconds)                              | `600`                |          |         |
| `DB_SQLITE_CONNECTIONS_MIN`    | Minimum DB connections                                       | `2`                  |          |         |
| `DB_SQLITE_CONNECTIONS_MAX`    | Maximum DB connections                                       | `10`                 |          |         |
| `DB_SQLITE_INIT`               | DB init string to run on connection                          |                      |          |         |


#### Log Configuration

| Parameter               | Description                                                  | Default                 | Advanced |
| ----------------------- | ------------------------------------------------------------ | ----------------------- | -------- |
| `LOG_TYPE`              | Log output type `console` `file` `both`                      | `file`                  |          |
| `LOG_LEVEL`             | Log level (`trace`, `debug`, `info`, `warn`, `error`, `off`) | `info`                  |          |
| `LOG_PATH`              | Logfiles path                                                | `/logs/vaultwarden`     |          |
| `LOG_FILE`              | Logfile name                                                 | `vaultwarden.log`       |          |
| `LOG_ENABLE_TIMESTAMPS` | Prepend timestamps to log lines                              | `TRUE`                  |          |
| `LOG_TIMESTAMP_FORMAT`  | Timestamp format used for logs                               | `%Y-%m-%d %H:%M:%S.%3f` |          |

#### Notifications

| Parameter                          | Description                         | Default                          | Advanced |
| ---------------------------------- | ----------------------------------- | -------------------------------- | -------- |
| `ENABLE_PUSH_NOTIFICATIONS`        | Enable push notifications           | `false`                          |          |
| `PUSH_NOTIFICASTIONS_RELAY_URI`    | Push notifications relay URI        | `https://push.bitwarden.com`     |          |
| `PUSH_NOTIFICASTIONS_IDENTITY_URI` | Identity URI for push notifications | `https://identity.bitwarden.com` |          |

## Other Unset Variables

>>The following variables are taken from the upstream Vaultwarden package and can only be set by environment variables. All other options missing can be configured via `/admin` endpoint.  Use these to control internal VaultWarden behaviors.

| Parameter                                  | Description                                          | Default                    |
| ------------------------------------------ | ---------------------------------------------------- | -------------------------- |
| `JOB_POLL_INTERVAL_MS`                     | Interval between background job polls (milliseconds) | `30000`                    |
| `SEND_PURGE_SCHEDULE`                      | Cron schedule to purge soft-sent items               | `0 5 * * * *`              |
| `TRASH_PURGE_SCHEDULE`                     | Cron schedule to empty trash buckets                 | `0 5 0 * * *`              |
| `INCOMPLETE_2FA_SCHEDULE`                  | Cron schedule for incomplete 2FA cleanup             | `30 * * * * *`             |
| `EMERGENCY_NOTIFICATION_REMINDER_SCHEDULE` | Cron schedule for emergency notification reminders   | `0 3 * * * *`              |
| `EMERGENCY_REQUEST_TIMEOUT_SCHEDULE`       | Cron schedule for emergency request timeouts         | `0 7 * * * *`              |
| `EVENT_CLEANUP_SCHEDULE`                   | Cron schedule for event cleanup                      | `0 10 0 * * *`             |
| `AUTH_REQUEST_PURGE_SCHEDULE`              | Cron schedule to purge auth requests                 | `30 * * * * *`             |
| `DUO_CONTEXT_PURGE_SCHEDULE`               | Cron schedule for Duo context cleanup                | `30 * * * * *`             |
| `EXPERIMENTAL_CLIENT_FEATURE_FLAGS`        | Enables experimental client features                 | `fido2-vault-credentials`  |
| `ICON_SERVICE`                             | Icon service behaviour (e.g., internal, external)    | `internal`                 |
| `ICON_REDIRECT_CODE`                       | HTTP redirect code used for icon redirects           | `302`                      |
| `ICON_CACHE_TTL`                           | Time-to-live (seconds) for icon cache                | `2592000`                  |
| `ICON_CACHE_NEGTTL`                        | Negative TTL (seconds) for icon cache misses         | `259200`                   |
| `ICON_DOWNLOAD_TIMEOUT`                    | Timeout for icon downloads (seconds)                 | `10`                       |
| `IP_HEADER`                                | Header used to determine client IP                   | `X-Real-IP`                |
| `INVITATION_EXPIRATION_HOURS`              | Hours before an invitation expires                   | `24`                       |
| `ORG_EVENTS_ENABLED`                       | Enable organisation events                           | `true`                     |
| `ORG_GROUPS_ENABLED`                       | Enable organisation groups                           | `false`                    |
| `EVENT_DAYS_RETAIN`                        | Number of days to retain events                      | `90`                       |
| `LOGIN_RATELIMIT_SECONDS`                  | Login rate-limit window (seconds)                    | `60`                       |
| `LOGIN_RATELIMIT_MAX_BURST`                | Login rate-limit max burst                           | `10`                       |
| `ADMIN_TOKEN`                              | Admin token (Argon2 or plaintext)                    | (unset)                    |
| `DISABLE_ADMIN_TOKEN`                      | Disable admin token authentication                   | `false`                    |
| `ADMIN_RATELIMIT_SECONDS`                  | Admin API rate-limit window (seconds)                | `300`                      |
| `ADMIN_RATELIMIT_MAX_BURST`                | Admin API rate-limit burst                           | `3`                        |
| `ADMIN_SESSION_LIFETIME`                   | Admin session lifetime (minutes)                     | `20`                       |
| `DOMAIN_SET`                               | Vendor variable - set domain on startup              | (unset)                    |
| `DOMAIN_ORIGIN`                            | Vendor variable - origin header for domain           | (unset)                    |
| `DOMAIN_PATH`                              | Vendor variable - domain path override               | (unset)                    |
| `ENFORCE_SINGLE_ORG_WITH_RESET_PW_POLICY`  | Enforce single org reset password policy             | `false`                    |
| `SSO_ENABLED`                              | Enable Single Sign-On                                | `false`                    |
| `SSO_ONLY`                                 | Allow only SSO based logins                          | `false`                    |
| `SSO_SIGNUPS_MATCH_EMAIL`                  | Match SSO signups to email domain                    | `true`                     |
| `SSO_ALLOW_UNKNOWN_EMAIL_VERIFICATION`     | Allow unknown emails for SSO signup                  | `false`                    |
| `SSO_AUTHORITY`                            | SSO authority/issuer URL                             | `https://auth.example.com` |
| `SSO_SCOPES`                               | SSO scopes                                           | `email profile`            |
| `SSO_CLIENT_ID`                            | SSO client ID                                        | (unset)                    |
| `SSO_CLIENT_SECRET`                        | SSO client secret                                    | (unset)                    |
| `SSO_PKCE`                                 | Use SSO PKCE for auth                                | `true`                     |
| `SSO_CLIENT_CACHE_EXPIRATION`              | SSO client cache expiration (seconds)                | `0`                        |

## Users and Groups

| Type  | Name          | ID   |
| ----- | ------------- | ---- |
| User  | `vaultwarden` | 1000 |
| Group | `vaultwarden` | 1000 |

### Networking

| Port   | Protocol | Description        |
| ------ | -------- | ------------------ |
| `8000` | tcp      | vaultwarden daemon |

* * *

## Maintenance

### Shell Access

For debugging and maintenance, `bash` and `sh` are available in the container.

## Support & Maintenance

- For community help, tips, and community discussions, visit the [Discussions board](/discussions).
- For personalized support or a support agreement, see [Nfrastack Support](https://nfrastack.com/).
- To report bugs, submit a [Bug Report](issues/new). Usage questions will be closed as not-a-bug.
- Feature requests are welcome, but not guaranteed. For prioritized development, consider a support agreement.
- Updates are best-effort, with priority given to active production use and support agreements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
