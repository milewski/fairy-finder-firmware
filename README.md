# Fairy Finder firmware releases

Public app-only firmware and update manifests for the Waveshare ESP32-S3-Zero.
The device checks [the latest release](https://github.com/milewski/fairy-finder-firmware/releases/latest).

Only `fairy-finder.bin` and `firmware.json` are release assets. Device Wi-Fi
credentials and private project sources are not included. The `.github` workflow
validates the uploaded files and publishes their matching firmware tag.

Use USB installation instructions supplied with your device for its first OTA
migration. Release binaries are app-only images, not complete flash backups.
