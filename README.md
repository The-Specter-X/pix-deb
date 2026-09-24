# Pix packaging for Debian

This repository contains the Debian packaging for [Linux Mint's Pix](https://github.com/linuxmint/pix), based on upstream tag **3.4.11**. The source version is **3.4.11+ds-1**. The `+ds` source excludes the upstream `debian/` directory; this repository supplies Debian's packaging files.

The source builds `pix` (image browser, editor and bundled extensions), `pix-data` (icons, translations, schemas and help) and `pix-dev` (headers, pkg-config file and extension development macro). Installing `pix` also installs `pix-data`. Debhelper produces automatic debug symbol packages.

The upstream web services option is disabled: its build probes combine libsoup 2 with WebKitGTK 4.1 (libsoup 3). Image editing, local import, metadata, RAW, JPEG XL, HEIF/AVIF, video, color management, web albums and Brasero integration remain enabled by their build dependencies. Verify the Meson configuration summary during each build.

## Build on Debian

On an up-to-date Debian unstable build VM install `devscripts`, `build-essential`, `dpkg-dev` and `lintian`, then use this packaging checkout:

```sh
uscan --download-current-version
mkdir -p ../pix-3.4.11+ds
tar -xf ../pix_3.4.11+ds.orig.tar.xz -C ../pix-3.4.11+ds --strip-components=1
cp -a debian ../pix-3.4.11+ds/
cd ../pix-3.4.11+ds
sudo apt build-dep .
dpkg-buildpackage -us -uc
lintian -i -I --pedantic ../pix_3.4.11+ds-1_*.changes
```

`debian/tests/smoke` checks the installed command and core files. Test image opening, editing, saving, thumbnails and plugins in a separate Debian Cinnamon desktop VM, including a Wayland session.

## Salsa and sponsorship

Push the packaging history to `https://salsa.debian.org/Overseer/pix`. Enable Salsa CI's standard pipeline in **Settings → CI/CD → General pipelines**, with path `debian/salsa-ci.yml`. Check WNPP and file or claim an ITP before requesting sponsorship. When the source, binary, tests, licensing review and CI pass, change `UNRELEASED` to `unstable`, build and sign the source package, and upload it to mentors.debian.net for review. GitHub and Salsa do not upload to Debian by themselves.

