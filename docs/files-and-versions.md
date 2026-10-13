# Files, versions, and duplicate decisions

The [CSV inventory](../reference/local-artifacts.sha256.csv) records 81 files from the OnePlus project directory, using project-relative paths, byte counts, and SHA-256 hashes. These are local identification hashes, not checks against signed upstream releases. The original directory name uses `KB005`; the PDF and OTA metadata identify KB2005.

## Main project files

Paths in this section are relative to the original `Oneplus 8T/` working folder.

| Local file | Role recorded in the PDF | Additional file evidence |
| --- | --- | --- |
| `crDroidAndroid-13.0-20230929-kebab-v9.9.zip` | Android 13 ROM | OTA metadata names OnePlus8T, Android 13, SDK 33, A/B update, security patch 2023-09-05 |
| `NikGapps-core-arm64-13-20250716-signed.zip` | GApps | Version/date are recorded by filename; signature authenticity was not checked |
| `Magisk/Magisk-v27.0.apk` and `.zip` | Root app and recovery installer | Exact duplicate bytes with different extensions |
| `RO2RW-StableBeta.v3.7.2.1.zip` | Writable-system conversion | Version identified by filename and PDF |
| `kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip` | NetHunter installation | Contains its own kernel payload; `module.prop` separately says 1.4.0, which is module metadata, not the 2025.3 distribution version |
| `KERNEL/NetErnel_v1_opkona.zip` | Replacement kernel | Contains `Image.gz`, `dtbo.img`, AnyKernel installer, and license; installer credits NetErnel v1 by Ygrek178, based on Meteoric V10 by HELLBOY017 |
| `KERNEL/NetErnel_modules_opkona.zip` | Kernel modules | Module metadata says v1.0 / versionCode 2004; includes the five Wi-Fi modules named in the PDF |
| `APPS/com.termux_1002.apk` | Terminal app | Filename identifier only; no tested app version recorded |
| `APPS/com.smartpack.kernelmanager_175.apk` | Kernel flashing and boot script manager | Filename identifier only; no tested app version recorded |
| `TWRP/recovery.img` | Recovery candidate | Different from both other recovery images; exact recovery build not identified |
| `CRDROID/*.img` | Extracted partition images | Present locally; the PDF does not identify a specific source path for its short image names |
| `MSMkebab_15_O.12_210201.zip` | Possible stock-restore bundle | Same member payloads as the separately named OxygenOS 11.0.7.9 archive |

No SHA-256 match to a maintainer-published checksum, APK signature verification, firmware flashing, archive installation, or kernel loading was performed.

## Distinct recovery images

All three images have different hashes. None was discarded or selected automatically:

| Path relative to the OnePlus project directory | Bytes | SHA-256 |
| --- | ---: | --- |
| `Oneplus 8T/TWRP/recovery.img` | 104857600 | `669fc0691a4019309f65801968b161da1bde30e01519ac9d0543b233e379fdab` |
| `Oneplus 8T/CRDROID/recovery.img` | 100663296 | `b3c22bbe317c479be5d18a8bc9b409e02d9a0b1b2f6df7a2fa54d0e759584ca7` |
| `extracted_20251002_182614/recovery.img` | 104857600 | `0938395d1cf279a73cb1e5f7c9d07d5b0c1f77e9a20fde809189515a1347f86e` |

The two sets of `vbmeta.img` and `vbmeta_system.img` also need explicit source selection. Matching filenames do not establish interchangeable content.

## Other firmware evidence

- `23c8b5ca8cb69cdbb889cc8684ac3e7dae569017.zip` internally identifies the KB2005 C.33 Android 12 OTA and the 2022-07-05 security patch. This is distinct from the Android 11 restore stage and the Android 13 custom ROM.
- `OnePlus_8T_Global_OxygenOS_11.0.7.9.zip` contains the `kebab_15_O.12_210201` restore bundle.
- The separate `kebab_15_O.12_200918/` folder is an older-looking, differently named restore set. It is retained separately; the PDF does not select it.
- `extracted_20251002_182614/` contains another group of partition images. Its folder name alone does not prove which firmware archive produced them.
- The separately supplied Platform-Tools folder records revision 36.0.0 and includes Windows `.exe` and `.dll` files. It is not part of the 81-file OnePlus manifest.

## Confirmed duplicate content

| Items compared | Finding | Package decision |
| --- | --- | --- |
| Magisk 27.0 APK and ZIP | Same 12,498,796 bytes; SHA-256 `f511bd33d3242911d05b0939f910a3133ef2ba0e0ff1e098128f9f3cd0c16610` | Document both historical uses; distribute neither binary |
| `Oneplus 8T.zip` and unpacked `Oneplus 8T/` | All 23 file members match the unpacked files by SHA-256; six additional entries are directories | Keep one PDF copy in this repository; do not upload the 9,801,192,318-byte wrapper |
| OxygenOS 11.0.7.9 ZIP and `MSMkebab_15_O.12_210201.zip` | Different whole-file hashes, but all 15 member names and their payloads are equivalent; raw compressed streams, sizes, CRC32 values, and compression attributes match | Record as equivalent payloads in different containers, not byte-identical ZIP files |

No original files were removed. Duplicate counts for loose files and duplicate content inside archives are separate measurements; they should not be added as if every item were an independent copy.

The [wrapper member comparison](../reference/wrapper-duplicate-evidence.json) and [OxygenOS payload comparison](../reference/oxygenos-payload-equivalence.json) preserve the corresponding machine-readable evidence.

## Missing or unresolved artifacts

- The PDF's `super-rw-sparse-fastboot-active-_b.img` is absent.
- The supplied NetHunter archive still contains `boot-patcher/Image.gz-dtb`, AnyKernel installation files, and a `4.19.226-NetHunter` module directory. The PDF says to remove an included kernel, but no modified ZIP or exact edit record was found. The observed member is evidence about the supplied archive, not proof of what was removed during the original session.
- No handset screenshots, flashing logs, root-check output, module-load output, adapter model, or performance measurements were supplied.
- No license or provenance record establishes redistribution rights for the complete firmware/tool collection. Only documentation and license notices are included here.
