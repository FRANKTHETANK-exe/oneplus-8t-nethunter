# Downloads and file preparation

Version-specific downloads and preparation for the [installation guide](README.md).

## Required downloads

| File | Source and preparation |
| --- | --- |
| `kebab_15_O.12_210201.zip` — OxygenOS 11.0.7.9.KB05AA with MSM Tool | [AndroidFileHost release by Some_Random_Username](https://androidfilehost.com/?fid=2188818919693750637). Select **Click Here to Start Download**, then choose a mirror. Extract the complete bundle and keep `MsmDownloadTool V4.0.exe`, the DLLs, and `kebab_15_O.12_210201.ops` together. This is the community-hosted Global KB05AA bundle. Check the download against the release page's MD5: `d7ebbe5aa52584f19135a4752b0cd922`. Mirror availability may vary. |
| `platform-tools_r36.0.0-win.zip` | [Google's Windows Platform-Tools 36.0.0 ZIP](https://dl.google.com/android/repository/platform-tools_r36.0.0-win.zip). Extract it and open the terminal in `platform-tools`. [Google's release notes](https://developer.android.com/tools/releases/platform-tools) identify this as the April 2025 version. |
| `crDroidAndroid-13.0-20230929-kebab-v9.9.zip` | [crDroid's exact SourceForge release](https://sourceforge.net/projects/crdroid/files/kebab/9.x/crDroidAndroid-13.0-20230929-kebab-v9.9.zip/download). Choose the September 29, 2023 Android 13 build for **kebab**, not the site's generic latest-download banner. Keep the ZIP intact for sideload and use it to extract the three images below. |
| `NikGapps-core-arm64-13-20250716-signed.zip` | [NikGapps' July 16, 2025 Core release](https://sourceforge.net/projects/nikgapps/files/Releases/Android-13/16-Jul-2025/NikGapps-core-arm64-13-20250716-signed.zip/download). Select **Core / arm64 / Android 13**. Keep the ZIP intact for sideload. |
| `Magisk-v27.0.apk` and `Magisk-v27.0.zip` | Download the APK from [Magisk's official v27.0 release](https://github.com/topjohnwu/Magisk/releases/tag/v27.0). Keep the APK for the app. Make a second copy and change its extension to `.zip` for recovery sideload; do not compress it. Both copies should have identical bytes. |
| `RO2RW-StableBeta.v3.7.2.1.zip` | [LeeGarChat's exact SourceForge release](https://sourceforge.net/projects/multi-function-patch/files/RO2RW/RO2RW-StableBeta.v3.7.2.1.zip/download). Copy the ZIP to the phone for installation in Magisk. |
| `com.termux_1002.apk` — Termux 0.118.3 | [F-Droid package page](https://f-droid.org/packages/com.termux/) → **0.118.3 (1002)** → [APK](https://f-droid.org/repo/com.termux_1002.apk). Install it on the phone. |
| `kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip` | [Kali's exact 2025.3 download](https://old.kali.org/nethunter-images/kali-2025.3/kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip). Verify it using [Kali's SHA256SUMS](https://old.kali.org/nethunter-images/kali-2025.3/SHA256SUMS), then prepare a copy as described below. Use this filename for the version pairing described in the guide. |
| `NetErnel_v1_opkona.zip` and `NetErnel_modules_opkona.zip` | Open [the original XDA release thread, page 20](https://xdaforums.com/t/rom-unofficial-nethunter-oneplus-8t-android-11-12-26-08-21.4324555/page-20), find Ygrek178's NetErnel post, and download the two attachments with these exact names. Copy both ZIPs to the phone. See the version checks below. |
| `com.smartpack.kernelmanager_175.apk` — SmartPack Kernel Manager 17.5 | [F-Droid package page](https://f-droid.org/packages/com.smartpack.kernelmanager/) → **v17.5 (175)** → [APK](https://f-droid.org/repo/com.smartpack.kernelmanager_175.apk). Install it before flashing NetErnel and creating the boot script. |

## Recovery and vbmeta images

Obtain `recovery.img`, `vbmeta.img`, and `vbmeta_system.img` from **the exact crDroid 9.9 ROM above**. The ROM's SHA-256 is:

```text
be2a4f7a75dbe106f1be1b861ab24fa0d71e4965978700e47f27f6ff7aad1ce3
```

To extract fresh copies on a computer:

1. Get the appropriate binary from [payload-dumper-go 2.1.0](https://github.com/ssut/payload-dumper-go/releases/tag/2.1.0) and check it against the release checksums.
2. Follow the project's [installation instructions](https://github.com/ssut/payload-dumper-go/blob/2.1.0/README.md), including its `xz` prerequisite, and put the tool on the terminal's `PATH`.
3. From the folder containing the ROM ZIP, run:

```sh
payload-dumper-go -p recovery,vbmeta,vbmeta_system -o crdroid-9.9-images crDroidAndroid-13.0-20230929-kebab-v9.9.zip
```

4. Check the resulting files against these SHA-256 values, then copy them into `platform-tools`:

```text
b3c22bbe317c479be5d18a8bc9b409e02d9a0b1b2f6df7a2fa54d0e759584ca7  recovery.img
91475c74ab8b3306de1c4b375de0254f6fbc0489b14195e93a001bf45986a7cf  vbmeta.img
679ef59220b2cab41d18257a09292bbe93fc53ec6415be49ff50603bed53ac35  vbmeta_system.img
```

On Windows PowerShell, calculate a file's hash with `Get-FileHash .\recovery.img -Algorithm SHA256`. On macOS, use `shasum -a 256 recovery.img`; on Linux, use `sha256sum recovery.img`.

Use the crDroid images extracted above. Stock recovery images belong to different builds, and [ApexLegend007's TWRP V1 release](https://github.com/ApexLegend007/twrp_device_oneplus_lemonades/releases/tag/V1) requires OOS 13 firmware on both slots. That TWRP release is not a substitute in this sequence.

## Preparing the NetHunter ZIP

The unchanged official download has SHA-256:

```text
a315a0f718b780656b7df4afd41ecc5b10588997ee52a2b8353a8dd89b85a262
```

1. Verify that checksum before editing the ZIP.
2. Keep the original and make a working copy.
3. Open the working copy in an archive manager that can delete individual ZIP entries.
4. Delete **only the top-level `kernel-nethunter.zip`**. Leave the `boot-patcher` directory and all other entries intact; do not extract and rebuild the entire archive.
5. Use this edited copy for the Magisk installation step.

The Magisk installer skips its bundled kernel-flash step when the top-level kernel ZIP is absent. The edited ZIP will have a different checksum from Kali's original download.

## NetErnel version checks

Download both named attachments from the XDA thread. If XDA returns HTTP 403 or an attachment is unavailable, use an existing copy only if it matches these SHA-256 values:

```text
e9afd9e750c1946542b5bf51a91f207f488731a702dcf5bc5aef59d42c81ed67  NetErnel_v1_opkona.zip
475d903a79fb4c4f77ce2dc5c1680c815afddbc8623de7c7685a146f6aa94f81  NetErnel_modules_opkona.zip
```

The kernel identifies itself as `4.19.275-NetErnel-v1_by@Ygrek178_base@HELLBOY017`, built March 9, 2023. The module package is version 1.0 / versionCode 2004 and contains the five Wi-Fi modules used in the boot script. Do not substitute another release if either hash differs without checking its matching kernel and module requirements.

## Generated RO2RW image

`super-rw-sparse-fastboot-active-_b.img` is generated by RO2RW on the phone; it is not a separate download. With `/data` mounted, version 3.7.2.1 writes it under `/data/media/0/RO2RW_SUPER/`. Its filename follows the active slot. Copy the phone's own output to the computer before the format-data step in the [installation guide](README.md#5-make-the-system-writable-with-ro2rw).

## Credits

Firmware, APKs, kernels, modules, and ROMs remain distributed by their respective authors. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for credits and the included notices.
