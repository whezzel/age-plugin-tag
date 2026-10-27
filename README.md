# age-plugin-tag (Arch PKGBUILD)

Builds `age-plugin-tag` and `age-plugin-tagpq` from the official [age](https://github.com/FiloSottile/age) source, for Arch Linux.

## Why

age 1.3 added tag recipients (`age1tag1…`, `age1tagpq1…`). The `age` CLI handles them natively, but other age clients need the plugin binaries on `$PATH`. Upstream release tarballs include them, while Arch's `extra/age` package only installs `age`, `age-keygen`, `age-inspect` and `age-plugin-batchpass`.

The case that needs them: [age-plugin-tpm](https://github.com/Foxboron/age-plugin-tpm) 1.0 prints `age1tag1…` recipients, so [sops](https://github.com/getsops/sops) can't encrypt to a TPM key without the plugin:

```
failed to wrap key for recipient #0: tag plugin: couldn't start plugin:
"tag" plugin not found: exec: "age-plugin-tag": executable file not found in $PATH
```

This package is a stopgap until the official `age` package ships the plugins.

## Install

```
git clone https://github.com/whezzel/age-plugin-tag.git
cd age-plugin-tag
makepkg -si
which age-plugin-tag      # /usr/bin/age-plugin-tag
```

It builds from the same upstream tarball and checksum as Arch's `age` package, using the same Go build flags (PIE, trimpath, readonly modules).

## Update

The plugins are standalone binaries, so an `age` upgrade won't break them. They do bundle age's crypto code from build time, though, so rebuild when `age` updates:

```
# set pkgver to the new age version and reset pkgrel=1
updpkgsums
makepkg -si
```

## When Arch ships the plugin

Once `extra/age` includes `/usr/bin/age-plugin-tag`, pacman will report a file conflict on upgrade. Remove this package and upgrade:

```
sudo pacman -R age-plugin-tag
sudo pacman -Syu
```

## License

age is BSD-3-Clause; the license is installed to `/usr/share/licenses/age-plugin-tag/`. This repo only contains packaging files.
