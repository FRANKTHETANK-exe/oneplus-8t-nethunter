# OnePlus 8T NetHunter project

Project notes for a OnePlus 8T KB2005 running crDroid 9.9 (Android 13), Magisk 27.0, RO2RW, Kali NetHunter, and NetErnel. The original manual reports that an external Wi-Fi adapter worked after loading the kernel modules.

This repository preserves that setup as a historical project record. The phone was not connected or tested during documentation review. The supplied manual contains instructions and a completion statement, but no device screenshots, command output, adapter model, or repeatable test results.

## Start here

- [Recorded installation sequence](docs/installation-record.md): the manual's stages, commands, and version-specific gaps.
- [Files and versions](docs/files-and-versions.md): what was actually in the project folder.
- [Results and troubleshooting](docs/results-and-troubleshooting.md): reported outcomes, unresolved points, and evidence still needed.
- [Sources and attribution](THIRD_PARTY_NOTICES.md): upstream projects and the limits of this archive.
- [Original four-page manual](reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf).

## Device and software

| Component | Recorded version or identity | Evidence |
| --- | --- | --- |
| Phone | OnePlus 8T KB2005 | PDF title on pages 1 and 4; the enclosing folder abbreviates this as `KB005` |
| ROM | crDroid 9.9 / Android 13, `kebab`, build 2023-09-29 | PDF and local ROM filename |
| Initial stock restore | Android 11 using MSM Tool | PDF; several stock firmware packages are present, with no exact selection recorded |
| GApps | NikGapps Core, ARM64, Android 13, 2025-07-16 | PDF and local filename |
| Root | Magisk 27.0 | PDF and APK/ZIP filenames |
| Writable system conversion | RO2RW StableBeta 3.7.2.1 | PDF and local filename |
| NetHunter | 2025.3, package named `oneplus8-oos-twelve-full` | PDF and local filename; compatibility with the Android 13 ROM was not independently established |
| Kernel | NetErnel v1 `opkona` plus its modules package | PDF and local filenames |
| Host tools | Android Platform-Tools 36.0.0, Windows files | Local `source.properties` and `.exe` files; the PDF does not specify this version |

The project is **OnePlus 8T**, not a verified guide for every OnePlus 8-series device. Do not infer device or ROM compatibility from similar names.

## Before attempting to reproduce it

The manual includes a bootloader unlock, partition flashes, a writable-system conversion, and a factory reset. Those are destructive operations. Its `_b` super-image filename, recovery choice, and stock firmware choice must be resolved for the actual device before use. The generated super image is absent from the reviewed collection.

The PDF's final claim that repeating the vbmeta flashes cannot erase data is not a reliable guarantee. The [official Magisk installation guide](https://topjohnwu.github.io/Magisk/install.html) warns that a vbmeta operation may wipe data and currently deprecates recovery-based Magisk installation. These notes preserve the original sequence; they do not certify it as a current installation procedure.

Use a device you own and networks or equipment you have permission to test. No device commands, installers, firmware, or security tools were executed while preparing this repository.

## What is included

Only the project documentation, original manual, and a checksum inventory are included. Firmware, APKs, Windows executables, partition images, and upstream tool archives remain outside this repository. Their presence on disk does not establish redistribution rights or authenticity.

The local SHA-256 inventory identifies reviewed files. It is not an upstream signature check and does not make a download trustworthy.

## Scope and authorship

The supplied manual is the project author's record. The Markdown documents organize that record and distinguish it from independently observed file evidence and current upstream cautions. Android, crDroid, Kali NetHunter, Magisk, NetErnel, RO2RW, NikGapps, Termux, and SmartPack are separate upstream projects; this repository does not claim authorship of them.
