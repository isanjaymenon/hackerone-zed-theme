# DEVELOPMENT.md

How to install, inspect, and publish the first HackerOne Theme release for Zed.

> [!NOTE]
> This guide is frozen for tag `v0.1.0`. For later versions use [DEVELOPMENT.md](DEVELOPMENT.md).

> [!TIP]
> Check out the tag before you install or submit the gallery PR:
> `git checkout v0.1.0`

## What shipped

Theme-only extension. No Rust/Wasm crate and no `Cargo.toml`.

| Field        | v0.1.0 value                                          |
| ------------ | ----------------------------------------------------- |
| Extension id | `hackerone-theme`                                     |
| Display name | HackerOne Theme                                       |
| Version      | `0.1.0`                                               |
| Themes       | HackerOne Dark, HackerOne Light                       |
| Theme file   | `themes/hackerone-dark.json` & `hackerone-light.json` |
| Schema       | [v0.2.0](https://zed.dev/schema/themes/v0.2.0.json)   |

## Layout at this tag

```sh
.
├── extension.toml
├── themes/
├── ├── hackerone-dark.json
│   └── hackerone-light.json
├── LICENSE
├── DEVELOPMENT.md
├── CHANGELOG.md
└── README.md
```

## Install this release as a dev extension

```sh
git clone https://github.com/isanjaymenon/hackerone-zed-theme.git
cd hackerone-zed-theme
git checkout v0.1.0
```

In Zed:

1. Command Palette → `zed: install dev extension`
2. Select the repository root (the directory that contains `extension.toml`)

If a published copy is already installed, Zed replaces it and the Extensions page shows **Overridden by dev extension**.

Reload after JSON edits with the theme selector (`cmd-k cmd-t` / `ctrl-k ctrl-t`). If colors do not update, remove and re-install the dev extension.

Logs: `zed: open log`. Verbose: `zed --foreground`.

## Edit colors (only if you are patching 0.1.0)

Colors live in `themes/hackerone.json`.

| Field                 | Meaning                              |
| --------------------- | ------------------------------------ |
| `themes[].name`       | `HackerOne Dark` / `HackerOne Light` |
| `themes[].appearance` | `dark` or `light`                    |
| `themes[].style`      | UI + syntax tokens                   |

Validate:

```sh
python3 -m json.tool themes/hackerone.json > /dev/null
```

Upstream reference: [HackerOne VS Code Theme](https://github.com/Hacker0x01/HackerOne-VS-Code-Theme). Zed token names are not the same as VS Code’s.

> [!WARNING]
> Do not bump `extension.toml` while still calling this `v0.1.0`. A version change is a new release.

## Publish v0.1.0 to the Zed gallery

The repo must be **public**. MIT is an allowed license.

1. Tag and push, if you have not already:

   ```sh
   git tag v0.1.0
   git push origin main
   git push origin v0.1.0
   ```

2. Fork [zed-industries/extensions](https://github.com/zed-industries/extensions).
3. Add this repo as a submodule (HTTPS only):

   ```sh
   git submodule add https://github.com/isanjaymenon/hackerone-zed-theme.git extensions/hackerone-theme
   cd extensions/hackerone-theme
   git checkout v0.1.0
   cd ../..
   ```

4. Add an entry to `extensions.toml`:

   ```toml
   [hackerone-theme]
   submodule = "extensions/hackerone-theme"
   version = "0.1.0"
   ```

The version must match `extension.toml` at the checked-out commit.

5. Run `pnpm sort-extensions`.
6. Open a PR. Docs: [Publishing Guide](https://zed.dev/docs/extensions/publishing/publishing-guide).

The id `hackerone-theme` already meets Zed’s rules: kebab-case, unique, no `zed` or `extension` in the id.

## Style notes for this release

- Unofficial port. Do not describe it as an official HackerOne product.
- Dual copyright in `LICENSE`: HackerOne 2022 + Sanjaymenon 2026.
- `authors` in `extension.toml` is the Zed maintainer, not the original VS Code designer.

## See also

- [README.md](README.md) — user-facing release notes
- [CHANGELOG.md](CHANGELOG.md) — `[0.1.0]` section
- [DEVELOPMENT.md](DEVELOPMENT.md) — living maintainer guide
