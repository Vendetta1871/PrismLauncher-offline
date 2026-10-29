# Prism Launcher (Offline)

A fork of [Prism Launcher](https://github.com/PrismLauncher/PrismLauncher) with
[offline (cracked) account](https://wiki.vg/Mojang_API#Offline) support enabled.
It exists to feed the AUR packages `prismlauncher-offline-git` (built from this
branch) and `prismlauncher-offline-bin` (installed from the GitHub releases of
this repository).

The upstream project stopped being mirrored by the previously used
`Diegiwg/PrismLauncher-Cracked` fork in July 2026, so the offline-account
changes are now maintained here, rebased onto upstream releases as they appear.

## What is changed compared to upstream

- `LaunchController`: no "at least one account must own Minecraft" gate; when
  launching without any account, an offline account is offered to be created
  right away; the requested launch mode is used as-is (no forced demo mode).
- `MinecraftAccount::ownsMinecraft()` and `AccountList::anyAccountIsValid()`
  always report a usable account.
- The "Add Microsoft" button is disabled; the accounts page points at
  "Add Offline".
- `MinecraftInstance::javaArguments()` points Minecraft's session/auth/services
  API hosts at `nope.invalid`, so the game never talks to Mojang auth.
- Updater / bug-tracker URLs point at this repository; upstream community
  (Matrix/Discord/Reddit) links are removed, since upstream does not support
  offline-account builds.
- `PRISMLAUNCHER_VERSION` environment variable can stamp the version at
  configure time (used by the release workflow).
- `release.yml` builds Linux x86_64 assets only and publishes the release
  automatically; the build deliberately omits `prismupdater` (updates are
  delivered by the AUR packages).

Everything else — branding, data locations, meta files — is untouched upstream
behavior: the launcher still uses `~/.local/share/PrismLauncher` and installs
itself under the canonical `org.prismlauncher.PrismLauncher` names.

## Versioning

Upstream version `X.Y.Z` becomes `X.Y.Z.B` here, where `B` is this fork's build
counter, reset to `1` whenever upstream releases a new version. The first
release of this fork is `11.1.1.1` (upstream 11.1.1 + offline accounts).

## Making a release

1. Merge the desired upstream tag into `main` (or just commit the change).
2. Bump `Launcher_VERSION_*` in `CMakeLists.txt` if needed.
3. `git tag X.Y.Z.B && git push origin X.Y.Z.B`.
4. The `Build and publish Linux release` workflow builds the AppImage and the
   portable tarball and publishes the release.

## Syncing with upstream

`main` is based on upstream tag `11.1.1`. To update:

```
git fetch upstream --tags
git merge <new-upstream-tag>   # resolve, bump version, tag, push
```

The `upstream` remote points at `https://github.com/PrismLauncher/PrismLauncher`.
