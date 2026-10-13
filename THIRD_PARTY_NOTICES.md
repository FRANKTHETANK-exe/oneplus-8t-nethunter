# Sources and attribution

## Project source

The original `OnePlus8T_NetHunter_Instruction_Manual_Final.pdf` was supplied by the project author. Its PDF metadata records creation on 2025-10-06. The readable Markdown documentation was prepared from its four pages and the accompanying local file inventory. Historical claims remain attributed to that manual.

## Upstream references

The following primary documentation was consulted on 2026-10-13. It supplies context and cautions, not independent verification of this particular build:

- [Kali NetHunter installation](https://www.kali.org/docs/nethunter/installing-nethunter/): device/version-specific setup and Magisk-module installation.
- [Magisk installation](https://topjohnwu.github.io/Magisk/install.html): current installation methods, deprecation of recovery installation, and potential data loss during vbmeta changes.
- [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools): official host tools and release information.
- [AOSP bootloader locking and unlocking](https://source.android.com/docs/core/architecture/bootloader/locking_unlocking): unlock confirmation and reset behavior.

The supplied files also name crDroid, NikGapps, NetErnel, RO2RW, Termux, SmartPack Kernel Manager, TWRP, MSM Tool, and OnePlus firmware. Their exact download provenance was not supplied in the manual. No replacement download source has been guessed.

The local NetErnel installer credits Ygrek178 and the Meteoric V10 base by HELLBOY017. Its AnyKernel framework credits osm0sis. The NetHunter package credits the Kali NetHunter Team and includes its own AnyKernel attribution.

Two distinct bundled notices are preserved without modification:

- [NetErnel's AnyKernel/tooling notice](notices/NetErnel-AnyKernel-LICENSE.txt), from `NetErnel_v1_opkona.zip` member `LICENSE`; SHA-256 `247498e167d7cba9255e62e74937c3cf317329e1b4043c26e1ed085fe8327314`.
- [NetHunter's AnyKernel/tooling notice](notices/NetHunter-AnyKernel-LICENSE.txt), from the NetHunter ZIP member `boot-patcher/LICENSE-AnyKernel`; SHA-256 `cce10c19337419412968d7388654f521f39bbc495c96014d14ca4659674093a2`.

These notices cover their stated components. They are not clearance to redistribute the complete firmware or tool archives.

## Distribution scope

No third-party APKs, ROMs, firmware, partition images, kernel binaries, Windows executables, or upstream source code are redistributed here. Their original Desktop copies and any bundled notices remain untouched. A local checksum list is provided for identification only.

No open-source license has been invented for the author's documentation. Publishing this repository does not relicense any upstream component or imply that its code was written by the project author. Reuse of those components remains subject to their own licenses and terms.
