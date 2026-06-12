# Changelog

## 0.25.1.2

- Remove the `remote_configuration` option added in 0.25.1.1 — the upstream
  project discourages it for security reasons. **If you enabled it, this
  update turns it back off.**
- Instead, `slskd.yml` now lives in the add-on configuration folder and can be
  edited from Home Assistant (File editor / Samba) at
  `/addon_configs/..._slskd/slskd.yml`. Most changes hot-reload. An existing
  `/data/slskd/slskd.yml` is migrated automatically.

## 0.25.1.1

- Add `remote_configuration` option (default `true`): edit slskd's full
  configuration (slskd.yml) from the web UI under System > Options.

## 0.25.1

- Initial release, wrapping upstream [slskd 0.25.1](https://github.com/slskd/slskd/releases/tag/0.25.1).
- Home Assistant ingress support (UI in the sidebar, HA authentication).
- Configurable shared/download directories under `/media`.
- Persistent state in `/data/slskd`.
