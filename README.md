# mcs-plugin-registry

Curated index of community plugins for **Midnight Cutter Studio** (MCS).

The app's built-in **Plugins → Marketplace** tab fetches
[`registry.json`](registry.json) over HTTPS, lists the plugins, and installs the
selected `.mcsplugin` archive after verifying its SHA-256.

## Format

`registry.json` is a single object validated by
[`registry.schema.json`](registry.schema.json):

```jsonc
{
  "version": 1,
  "plugins": [
    {
      "id": "acme.wordcount",                       // matches manifest.id
      "name": "Word Count",
      "version": "1.2.0",                           // semver of this release
      "apiVersion": "1.0",                          // Plugin-API contract (major.minor)
      "description": "Counts words on the canvas.",
      "author": "Acme",                             // optional
      "permissions": ["network"],                   // optional; shown in a consent dialog on install
      "downloadUrl": "https://github.com/acme/mcs-wordcount/releases/download/v1.2.0/wordcount.mcsplugin",
      "sha256": "<64 lowercase hex chars>",          // of the .mcsplugin archive; verified before unpack
      "iconUrl": "https://.../icon.png",             // optional
      "homepage": "https://github.com/acme/mcs-wordcount"  // optional
    }
  ]
}
```

`downloadUrl`, `iconUrl` and `homepage` must be `https://`. `version` is the
plugin release version (compared by semver to detect updates); `apiVersion` is
the host contract the plugin was built against (the app refuses a major
mismatch).

## Submitting a plugin

1. Publish a `.mcsplugin` archive as a stable HTTPS asset (e.g. a GitHub
   release). Build it with [`@midnightcrafters/mcs-plugin-sdk`](https://www.npmjs.com/package/@midnightcrafters/mcs-plugin-sdk).
2. Compute its checksum: `sha256sum your-plugin.mcsplugin`.
3. Open a PR adding one entry to `plugins` in `registry.json`. CI validates the
   file against the schema.
4. A maintainer reviews the plugin and merges.

The registry is intentionally curated: every entry is reviewed before it
appears in the app.

## Serving

The app reads the `main` branch raw URL:

```
https://raw.githubusercontent.com/MidnightCrafters/mcs-plugin-registry/main/registry.json
```
