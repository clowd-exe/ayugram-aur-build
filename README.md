# Build `ayugram-desktop` from the AUR with GitHub Actions

There is no `appayugram-desktop` in the AUR. The package is
[`ayugram-desktop`](https://aur.archlinux.org/packages/ayugram-desktop) (the
`-bin` variant exists but is stale, 7.0.9 vs 7.2.9).

This repository builds the AUR package, and its only AUR dependency
(`libtg_owt-git`), inside an Arch Linux container on GitHub's runners. Nothing
is compiled on your machine; you just download the finished `.pkg.tar.zst` and
install it with pacman.

## Run the build

GitHub web UI: **Actions → Build AUR package → Run workflow**, wait, then
download the `ayugram-desktop-x86_64` artifact from the run page.

Or with the `gh` CLI:

```sh
gh workflow run build-ayugram.yml
gh run watch
gh run download --name ayugram-desktop-x86_64 -D pkg
```

The first build takes roughly 1-2 hours (TDLib, WebRTC and AyuGram are large).

## Install on Arch

```sh
sudo pacman -Syu
sudo pacman -U pkg/ayugram-desktop-*.pkg.tar.zst
```

Update by running the workflow again and installing the new artifact.

## Notes

- Make the repo **public** so Actions minutes are free/unlimited. Private repos
  get 2,000 free minutes per month, and this build can use a big chunk of that.
- The workflow runs `pacman -Syu` in the container first, so the package is
  built against current Arch libraries. Keep your system updated before
  installing it.
- To override the bundled Telegram API ID/hash, edit the PKGBUILD build step or
  add `MAKEPKG_AYUGRAM_API_ID` / `MAKEPKG_AYUGRAM_API_HASH` environment
  variables to the build step.
