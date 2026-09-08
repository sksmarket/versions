# versions

Version manifests for the SKS apps. Every app polls its own file here and
compares it against the running build; installers are still served from
sksmarket.com/software and Google Drive, never from this repo.

Read at `https://raw.githubusercontent.com/sksmarket/versions/main/<folder>/<file>.json`.

## Layout

One folder per product, one file per brand. The folder is fixed in the app's
code (`VersionManifest` in the `sks_app_update` package); the filename comes
from `brands/<brand>/config.json` -> `versionJsonFile`, so adding a white-label
brand means adding a file here, not changing code.

| Folder | App | Files |
|---|---|---|
| `market/` | sks_market (seller / POS) | `sksmarket.json`, `hearty.json` |
| `buyer/` | buyer marketplace | `sksmart.json`, `matworld.json`, `krishnatex.json` |
| `jewellery/` | jewellery | `sksjewellery.json`, `jjjewellery.json` |
| `fieldsales/` | field_sales | `sksfieldsales.json` |
| `cable/` | sks_cable | `skscable.json` |
| `tools/` | companion apps (single build each) | `skswaiter.json`, `sksserver.json`, `onlinesync.json`, `sksconnect.json`, `sksnexus.json` |

## File shapes

Two shapes are in use. Most apps read the plain `*Version` keys:

```json
{ "webVersion": "1.1.8", "androidVersion": "1.1.8", "windowsVersion": "1.1.8" }
```

`market/` and `cable/` read the `*VersionCode` keys instead (historical, the
apps declare the field names explicitly). `market/` also carries the offline
build and the Windows service:

```json
{
  "androidVersionCode": "4.5.4",
  "offlineAppVersionCode": "4.5.4",
  "windowsVersionCode": "4.5.4",
  "webVersionCode": "4.5.4",
  "serviceVersion": 55
}
```

A missing key means "no update for that platform" — it never breaks the check.

## Releasing

Bump only the platform you actually published. On web, a bump makes every open
tab clear its service-worker caches and reload, so publish the build first and
bump second — otherwise tabs reload onto the old files.
