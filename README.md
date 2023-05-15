# sbupdate

Generate signed Unified Kernel Images

> This tool is originally developed by [Andrey Vihrov](https://github.com/andreyv/sbupdate),
> now rewritten as a mkinitcpio post hook by YHNdnzj

## Installation

```console
$ aur_helper -S sbupdate-mkinitcpio
```

## Usage

### Generate custom Secure Boot keys

Various ways of doing this can be found on [ArchWiki](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot#Creating_keys), and the simplest of which is to use [sbkeys](https://github.com/electrickite/sbkeys):

```console
# mkdir -m 0700 /etc/efi-keys
# cd /etc/efi-keys
# sbkeys
```

`/etc/efi-keys` is the default location used by `sbupdate` to search for Secure Boot keys. This can be changed in `/etc/sbupdate.conf` through `KEY_DIR` setting.

### Configure sbupdate

`sbupdate` is a mkinitcpio post hook, so it automatically retrieves kernel image and initramfs locations.

However, you need to configure which kernels to generate UKI for and kernel parameters. All available settings are listed in `/etc/sbupdate.conf` with examples.

#### ESP mountpoint

`sbupdate` uses `bootctl --print-boot-path` to acquire the mountpoint of EFI System Partition or XBOOTLDR. This shouldn't need manual configuration.

#### EXTRA_SIGN

This is an extra function provided by `sbupdate` beside generating UKIs. A list of extra EFI binaries can be provided for `sbupdate` to sign using the configured Secure Boot keys. A [systemd.path(5)](https://man.archlinux.org/man/systemd.path.5.en) unit is also enabled to trigger re-signing when the binaries get modified.

### Generate signed UKIs

```console
# mkinitcpio -P
```

And confirm that UKIs are put into place as configured using `UKI_DIR` 😉
