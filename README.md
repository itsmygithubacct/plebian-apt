# plebian-apt

The apt repository for [Plebian-OS](https://github.com/itsmygithubacct/plebian-os)
packages. The repository itself is served from the `gh-pages` branch.

**Status: preview.** It carries the first packaged build of the Plebian-OS core
(`0.2.3+packaging1`), published for testing ahead of the 0.2.4 release.
Installing these packages on a machine does not by itself turn it into
Plebian-OS, and the first-boot unit is not enabled by installing them.

Debian 13 (trixie), `amd64` and `arm64`.

## Use

```sh
sudo curl -fsSL -o /usr/share/keyrings/plebian-archive-keyring.gpg \
    https://itsmygithubacct.github.io/plebian-apt/plebian-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/plebian-archive-keyring.gpg] https://itsmygithubacct.github.io/plebian-apt trixie main' \
    | sudo tee /etc/apt/sources.list.d/plebian-os.list
sudo apt update
sudo apt install plebian-archive-keyring
```

The `plebian-archive-keyring` package owns the key file from then on.

### Opt in to the 0.2.4 Kilix package

Kilix `0.2.3+packaging7` recognizes the system EnCodec converters for new
users on amd64 and arm64. The signed `UNRELEASED` suite carries this package.
Keep the `trixie` entry above for its dependencies. To select the preview:

```sh
echo 'deb [signed-by=/usr/share/keyrings/plebian-archive-keyring.gpg] https://itsmygithubacct.github.io/plebian-apt UNRELEASED main' \\
    | sudo tee /etc/apt/sources.list.d/plebian-unreleased.list
sudo apt update
sudo apt install kilix=0.2.3+packaging7
```

The preview carries `NotAutomatic: yes`. The 0.2.4 installer selects the
package by its exact size and SHA-256.

When rebuilding the `trixie` Pages tree, generate its indexes from its own
packages first. Then run `tools/preserve-unreleased GH_PAGES_CHECKOUT REBUILT_PAGES`
from this branch using an up-to-date `gh-pages` checkout. It verifies and copies
the signed preview indexes and their referenced packages before deployment.

## Signing key

```
Plebian-OS Archive <itsmygithubacct@users.noreply.github.com>
1730 239E 771F E87E D7AB 9BCF EDC6 05E7 6D3B 99B0
```

The key is used for this repository only. Name it with `signed-by` as above;
do not add it to apt's global trusted keys.

## Packages

| Package | Contents |
| --- | --- |
| `plebian-archive-keyring` | The public signing key |
| `plebian-os-release` | Release version and manifest |
| `plebian-os-tools` | Updater, closure selector and root helpers |
| `plebian-os-firstboot` | First-boot provisioner and its unit (not enabled on install) |
| `plebian-desktop-base` | Wallpaper, greeter settings and artwork notices |

Source: branch `work/0.2.4-debian-core` of
[plebian-os](https://github.com/itsmygithubacct/plebian-os), directory `debian/`.
