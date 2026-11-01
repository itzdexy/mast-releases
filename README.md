# Mast releases

Installers and the signed update feed for **Mast**, a private, calm browser
built on Chromium. The source lives in a separate repository; this one holds
release files only.

## What each release contains

- The Windows installer (`mast_installer_<version>.exe`) and, from the first
  Linux release on, a Linux archive (`mast-<version>-linux-x64.tar.xz`).
- `update.json`: a manifest naming the version, the release date and, for
  each platform, the download's URL, size and SHA-256, signed with Ed25519.

## How Mast updates itself

Every 5 hours while it runs, Mast reads
`https://github.com/itzdexy/mast-releases/releases/latest/download/update.json`.
It installs a newer release only when:

1. the manifest's signature checks out against the public key built into
   Mast (below);
2. the manifest is not older than one it has already seen;
3. the downloaded file has exactly the size and SHA-256 the manifest names.

The new version installs next to the running one; Mast offers to relaunch.
The request carries no cookies and no data about you.

Release key `mast-2026` (Ed25519 public key, base64):

```
lN4DMuC0lvnzk/TPgti99zk0hJrrpi/Bnbb3I1TimGw=
```

## Checking a download yourself

Compare the file's SHA-256 with the one in the release's `update.json`
manifest:

```
certutil -hashfile mast_installer_<version>.exe SHA256
sha256sum mast-<version>-linux-x64.tar.xz
```

The Windows installer is not Authenticode-signed yet, so Windows SmartScreen
may warn before the first install. Updates are checked by Mast's own
signature either way.
