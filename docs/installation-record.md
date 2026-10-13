# Recorded installation sequence

This is a transcription and explanation of the supplied four-page manual. Page references below point to [that PDF](../reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf). Commands are preserved as historical evidence, not an unattended installation script. No step was rerun during review.

## Prerequisites supported by the record

The manual names a OnePlus 8T KB2005, a PC connection, MSM Tool, `adb` and `fastboot`, USB debugging, recovery, and the packages listed in [files and versions](files-and-versions.md). The supplied host executables are Windows binaries. It does not record a host OS version, USB driver version, OEM-unlock eligibility, starting bootloader state, partition checksums, battery requirement, adapter model, or backup procedure.

Before reproduction, establish those missing details and a recovery plan from the maintainers' device-specific documentation. Preserve user data before unlock, restore, or format operations. Do not substitute a different recovery or partition image solely because its filename matches.

## 1. Stock restore (page 1)

The recorded starting point is an MSM Tool restore to Android 11:

1. Power off the phone.
2. Hold both volume buttons without pressing Power.
3. Connect it to the PC.
4. Wait for `ENUM` in MSM Tool and select Start; the manual notes that the screen stays black.
5. The manual says not to update the firmware after this restore.

The folder contains multiple MSM/stock packages. The PDF does not identify which was used. No restore log or result screenshot was provided. Do not select a package from this inventory without resolving its model, region, and firmware compatibility.

## 2. Bootloader unlock (page 1)

```text
./adb devices
./adb reboot bootloader
./fastboot flashing unlock
```

The manual then records confirmation on the phone, a data wipe, reboot, and setup. USB debugging is explicitly mentioned before the ROM stage; its earlier configuration and OEM-unlock eligibility are not described. The [AOSP bootloader documentation](https://source.android.com/docs/core/architecture/bootloader/locking_unlocking) describes the unlock confirmation and factory-reset requirement; it does not certify unlock support for this particular handset.

## 3. crDroid 9.9 and GApps (pages 1-2)

The manual places the ROM ZIP, NikGapps ZIP, `vbmeta.img`, `vbmeta_system.img`, and `recovery.img` with Platform-Tools and records:

```text
./adb reboot bootloader
./fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
./fastboot --disable-verity --disable-verification flash vbmeta_system vbmeta_system.img
./fastboot boot recovery.img
./adb sideload crDroidAndroid-13.0-20230929-kebab-v9.9.zip
./adb sideload NikGapps-core-arm64-13-20250716-signed.zip
```

It says the sideload percentage may stop at 47% and that the first boot can take several minutes. No host output or recovery success screen substantiates that observation. A percentage alone should not be treated as proof of successful installation.

There are multiple `recovery.img` and vbmeta candidates in the collection. The PDF does not resolve their exact paths or hashes. These commands deliberately retain the original short filenames rather than inventing a selection.

## 4. Magisk (page 2)

```text
./adb reboot recovery
./adb sideload Magisk-v27.0.zip
```

The next recorded steps are rebooting, manually installing `Magisk-v27.0.apk`, completing setup, and verifying root. No root-check output is supplied. Current [Magisk documentation](https://topjohnwu.github.io/Magisk/install.html) deprecates recovery installation and describes image patching; the historical method here must not be mistaken for current guidance.

## 5. RO2RW conversion (pages 2-3)

The manual copies RO2RW StableBeta 3.7.2.1 and Termux to the phone, then records this Termux package update:

```text
apt update && apt upgrade -y
```

It installs the RO2RW ZIP through Magisk, reboots, and then runs in Termux:

```text
su
ro2rw
```

It records these menu choices in this order:

| Input | Label in the manual |
| --- | --- |
| 1 | Install |
| 1 | Max expansion |
| 1 | Continue |
| 2 | YES to DFE |
| Any key | Continue |
| 2 | NO REMOVE |
| 2 | NO BACKUP |
| 1 | Force Disable (fastboot/sparse) |

The meaning of each numbered choice depends on that exact tool version. The choice labeled `NO BACKUP` is part of the recorded session, not advice to omit a backup.

The manual then records:

```text
./adb reboot bootloader
./fastboot flash super super-rw-sparse-fastboot-active-_b.img
./fastboot reboot recovery
```

It follows this with Factory reset, Format data, and Boot system, then root verification and `ro2rw --check`.

**Unresolved:** the generated super image is not present; no generation log, transfer step, hash, size, or active-slot check is recorded. The `_b` name is specific to the manual's run. Do not reuse it as a universal image or slot instruction. Formatting data and repartitioning are destructive.

## 6. Kali NetHunter (page 3)

The manual names `kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip`, says to remove its included kernel while leaving the rest archived, and installs the ZIP as a Magisk module. It records a wait of about 25 minutes, a reboot, and opening the NetHunter app.

The exact ZIP member to remove, archive-editing method, and checksum of the modified archive are missing. The package name also identifies an OxygenOS 12 target, while the reported ROM is Android 13. Compatibility has not been independently reproduced.

The following commands are then recorded in the NetHunter environment:

```text
echo 'APT::Sandbox::User "root";' | sudo tee /etc/apt/apt.conf.d/01-android-nosandbox
apt install kali-linux-everything -y
apt update && apt upgrade -y
apt autoremove -y
```

The first line changes APT's sandbox user to root. No error log explains why it was needed. The large metapackage and unpinned updates do not define a reproducible package set; no resulting package list, disk-space measurement, or successful command output is supplied. No individual security-tool use or network testing is documented.

## 7. NetErnel kernel and modules (page 3)

The manual flashes `NetErnel_v1_opkona.zip` in SmartPack Kernel Manager, reboots, installs `NetErnel_modules_opkona.zip` through Magisk Modules, and reboots again. No kernel source commit, kernel version output, module ABI report, or install log is included.

## 8. External Wi-Fi module loading (pages 3-4)

In SmartPack's Script Manager, the manual creates a script named `external_wifi_driver_load` containing:

```text
insmod /system/lib/modules/rtl_drivers/88XXau.ko
insmod /system/lib/modules/rtl_drivers/8188eu.ko
insmod /system/lib/modules/rtl_drivers/8192eu.ko
insmod /system/lib/modules/rtl_drivers/8192fu.ko
insmod /system/lib/modules/rtl_drivers/8814au.ko
```

It enables the script on boot and reboots. The five module names are not proof that five adapters were tested. No adapter model, USB ID, loaded-module output, or wireless-interface output is supplied. The script has no error handling in the PDF, so no success should be inferred if a module is absent or incompatible.

## 9. Final vbmeta step (page 4)

The manual repeats the two vbmeta flashes and reboots:

```text
./fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
./fastboot --disable-verity --disable-verification flash vbmeta_system vbmeta_system.img
./fastboot reboot
```

It attributes this to stability and claims that data is not erased. That claim is unverified and is not adopted here. The document also omits an explicit transition back to the bootloader for this final step. The [Magisk guide](https://topjohnwu.github.io/Magisk/install.html) warns that vbmeta flashing may wipe data. Disabling verification changes the device's integrity checks; a successful boot alone does not establish full stability.
