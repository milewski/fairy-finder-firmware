# Fairy Finder firmware releases

Public app-only firmware and update manifests for the Waveshare ESP32-S3-Zero.
Devices and the browser-assisted updater use the latest **stable** release;
`main/firmware.json` and `main/fairy-finder.bin` are its matching browser mirror.

Only `fairy-finder.bin` and `firmware.json` are release assets. Device Wi-Fi
credentials and private project sources are not included. The workflow validates
board, version, size and hash before publishing the matching immutable tag.
A tag's `release-channel.txt` selects `stable` or `test`; old tags without it
retain stable publication behavior. The application version stays a normal
numeric version so existing offline upload receivers can accept a test image.

## Spare-device test OTAs

A test release is marked **Prerelease** and explicitly excluded from Latest.
Publishing it pushes only its immutable tag: the normal stable `main` mirror and
update feed keep their previous firmware. No final device is automatically offered
the test through the normal feed.

Download the test release's complete `fairy-finder.bin`, open admin on the spare
device, and use its local file upload. Keep power connected and give the fresh
physical installation approval. Use a device already on the supported dual-slot
layout; these app-only files cannot add OTA prerequisites to pre-OTA hardware.

Publication is separate from physical acceptance. After the spare device passes
its required checks, a maintainer can deliberately run **Publish firmware assets**
with the existing test tag and `promote=true`:

```sh
gh workflow run publish.yml --repo milewski/fairy-finder-firmware \
  --ref main -f tag=firmware-v0.3.17 -F promote=true
```

Promotion verifies the already-published release files against the immutable tag,
refuses an older version or changed bytes at the same version, updates the stable
raw mirror, and marks that release stable and Latest. It does not rebuild or
replace the immutable tag/image. This action must wait for explicit acceptance;
ordinary tag pushes and dispatches default to `promote=false`.

## Maintainer deployment

This README and `publish.yml` are maintained in the private source repository's
`firmware/release-repository/`. Before publishing the first test tag, copy the
reviewed workflow to `.github/workflows/publish.yml` and this file to `README.md`
on the public repository's existing `main`, then commit and push those two files.
Keep its current firmware image and manifest unchanged. The source publication
job refuses a test push while the public workflow still lacks channel support.
Set the private source's `firmware/release-channel.txt` to `test` for a spare-device
release or `stable` for an authorized ordinary production release.

Use USB installation instructions supplied with your device for its first OTA
migration. Release binaries are app-only images, not complete flash backups.
