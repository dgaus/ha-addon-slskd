# slskd Home Assistant Add-on

[slskd](https://github.com/slskd/slskd) is a modern client-server application for the
[Soulseek](https://www.slsknet.org/) file sharing network, popular for discovering and
sharing music.

## Setup

1. Set **Soulseek username** and **Soulseek password** in the Configuration tab.
   If you don't have a Soulseek account, just pick a username and password — accounts
   are created automatically on first login.
2. Point **Shared directories** at the folders you want to share (default `/media/music`).
   Sharing files is expected etiquette on Soulseek; users who share nothing often get
   refused by others.
3. Start the add-on and open the UI from the sidebar (**slskd**).

## Port forwarding

For reliable transfers, forward TCP port **50300** on your router to your Home Assistant
machine. Without it, you can only exchange files with peers who have an open port
themselves. If you change the **Soulseek listen port** option, change the port mapping
in the add-on's Network section (and your router rule) to match.

## Storage

| Path | Purpose |
|------|---------|
| `/media/slskd/downloads` | Completed downloads (default, configurable) |
| `/media/music` | Default shared directory (configurable) |
| `/addon_configs/..._slskd/slskd.yml` | slskd's configuration file, editable from Home Assistant (see below) |
| `/data/slskd` | slskd state: databases, search history. Survives updates and rebuilds. |

The `/media` folder is shared with Home Assistant (visible in the Media browser) and
other add-ons such as Samba.

## Web UI access and authentication

By default the UI is only reachable through Home Assistant ingress, which already
authenticates you, so slskd's own login is disabled.

If you map host port **5030** in the Network section for direct/API access:

- **Set the Web UI username and password options.** Otherwise anyone on your network
  can reach an unauthenticated slskd.
- Note that with ingress enabled, slskd serves under the ingress URL path. Direct
  requests must use the same base path (shown in the add-on log at startup as
  `Serving under ingress path: ...`). For API clients, an API key configured in
  slskd's options file (`/data/slskd/slskd.yml`) is the cleaner route.

## Advanced configuration

Anything not covered by the add-on options (upload/download slots, speed limits,
user groups, filters, …) can be set in slskd's own configuration file, which this
add-on keeps in the add-on configuration folder so you can edit it from Home
Assistant: open the **File editor** (or **Studio Code Server** / **Samba**) add-on
and edit `/addon_configs/..._slskd/slskd.yml`. The file starts out as slskd's fully
commented example, and slskd watches it — most changes apply within seconds without
a restart. See the
[slskd configuration docs](https://github.com/slskd/slskd/blob/master/docs/config.md)
for the full reference.

For example, to limit uploads to 10 slots:

```yaml
transfers:
  upload:
    slots: 10
```

Notes:

- Settings managed by this add-on (Soulseek credentials, shared/download
  directories, listen port, web UI auth) are passed as environment variables, which
  **override** the YAML — change those in the add-on's Configuration tab, not in
  slskd.yml.
- slskd's remote configuration feature (editing the YAML from its web UI) is
  deliberately not enabled: the upstream project
  [discourages it](https://github.com/slskd/slskd/blob/master/docs/config.md#remote-configuration)
  because anyone who reaches the UI could read or rewrite the entire
  configuration.
