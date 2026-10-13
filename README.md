# OnePlus 8T NetHunter

Installation guide for Kali NetHunter on a **OnePlus 8T KB2005**, using **Android 13 / crDroid 9.9**, Magisk, RO2RW, and NetErnel with external USB Wi-Fi drivers.

[PDF manual](reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf) · [Downloads and file preparation](DOWNLOADS.md)

Download and prepare the version-specific files before connecting the phone.

## Before starting

- A OnePlus 8T KB2005, a Windows PC for MSM Tool, and a reliable USB data cable.
- A backup stored off the phone. Stock restore, bootloader unlock, and the RO2RW format step erase data.
- Android Platform-Tools 36.0.0, with a terminal open in its `platform-tools` folder.
- USB debugging enabled whenever Android is running; accept the computer's authorization prompt. Enable OEM unlocking before the bootloader-unlock step.
- The files in [DOWNLOADS.md](DOWNLOADS.md), with the ROM, Gapps, recovery, and vbmeta files copied into the terminal's working directory.

Flashing the wrong model, partition image, or slot can leave the phone unable to boot. This procedure targets the KB2005 and the versions listed below; check compatibility before using other models or releases. Keep the stock recovery bundle available before proceeding.

## 1. Restore stock Android 11 with MSM Tool

Use the **OxygenOS 11.0.7.9.KB05AA / `kebab_15_O.12_210201`** bundle from the download guide. Extract the whole bundle and keep its executable, DLLs, and `.ops` file together.

1. Open MSM Tool on the Windows PC.
2. Power off the phone.
3. Hold **Volume Up + Volume Down**, without holding Power, and connect the USB cable.
4. Wait for the phone to appear under **ENUM**, then select **Start**.
5. Let the restore finish. The phone screen can remain black during the restore.

Set up Android 11 and **do not install an OxygenOS update** before continuing. If MSM does not detect the phone, check the cable and Windows device/driver detection before continuing.

## 2. Unlock the bootloader

With USB debugging and OEM unlocking enabled:

```sh
./adb devices
./adb reboot bootloader
./fastboot flashing unlock
```

Confirm the unlock on the phone. This wipes the phone. Complete Android setup again and re-enable USB debugging.

## 3. Install crDroid 9.9 and NikGapps

Place these files in `platform-tools`:

```text
crDroidAndroid-13.0-20230929-kebab-v9.9.zip
NikGapps-core-arm64-13-20250716-signed.zip
recovery.img
vbmeta.img
vbmeta_system.img
```

Use the three images extracted from the exact crDroid 9.9 ROM, as described in [Recovery and vbmeta images](DOWNLOADS.md#recovery-and-vbmeta-images). Do not select an image solely because it is named `recovery.img`.

```sh
./adb reboot bootloader
./fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
./fastboot --disable-verity --disable-verification flash vbmeta_system vbmeta_system.img
./fastboot boot recovery.img
```

In recovery, select its ADB sideload option, then run:

```sh
./adb sideload crDroidAndroid-13.0-20230929-kebab-v9.9.zip
```

Return to the sideload option for Gapps:

```sh
./adb sideload NikGapps-core-arm64-13-20250716-signed.zip
```

ADB progress on the computer may stop around 47%. Check the recovery's completion message before continuing; the percentage alone does not establish success. Reboot to Android and allow several minutes for the first boot.

## 4. Install Magisk 27.0

Make the `.zip` and `.apk` copies described in the download guide.

```sh
./adb reboot recovery
```

Select ADB sideload in recovery:

```sh
./adb sideload Magisk-v27.0.zip
```

Reboot, install `Magisk-v27.0.apk`, complete Magisk's setup, and confirm root access. [Recovery installation is deprecated in Magisk](https://topjohnwu.github.io/Magisk/install.html#custom-recovery).

## 5. Make the system writable with RO2RW

Copy `RO2RW-StableBeta.v3.7.2.1.zip` and the Termux APK to the phone. Install Termux and run:

```sh
apt update && apt upgrade -y
```

In Magisk, install RO2RW as a module and reboot. Open Termux:

```sh
su
ro2rw
```

Use these selections in **RO2RW 3.7.2.1**:

| Prompt | Selection |
| --- | --- |
| Install | `1` |
| Maximum expansion | `1` |
| Continue | `1` |
| YES DFE | `2` |
| Pause | Any key |
| NO REMOVE | `2` |
| NO BACKUP | `2` |
| Force Disable / fastboot sparse output | `1` |

RO2RW generates a `super` image from the phone. With `/data` mounted, it writes to `/data/media/0/RO2RW_SUPER/` (the `RO2RW_SUPER` folder in internal storage); otherwise it falls back to a `RO2RW_SUPER` folder beside the script. Copy the generated image to the computer **before formatting data**.

Use the image generated on this phone for its active slot. The commands below use `super-rw-sparse-fastboot-active-_b.img`; if the output is for slot `_a`, substitute that exact filename:

```sh
./adb reboot bootloader
./fastboot flash super super-rw-sparse-fastboot-active-_b.img
./fastboot reboot recovery
```

In recovery, perform **Factory reset → Format data**, then boot Android. This removes user-installed apps and Magisk modules under `/data`; restore the tools before continuing:

1. Complete Android setup, re-enable USB debugging, and copy back `Magisk-v27.0.apk`, the Termux APK, and `RO2RW-StableBeta.v3.7.2.1.zip` from the computer. Use the same versions in [DOWNLOADS.md](DOWNLOADS.md).
2. Reinstall and open Magisk, complete any additional/environment setup, and reboot if prompted. Reinstall and open Termux to initialize its environment. Run `su -c id`, grant the Magisk request, and confirm `uid=0`. If root is unavailable, repeat [step 4](#4-install-magisk-270) before proceeding; installing the APK alone does not establish root.
3. Reinstall the RO2RW ZIP through **Magisk → Modules → Install from storage**, then reboot. This restores the module and command; it does not repeat the super-image conversion.

Open Termux and run:

```sh
su
ro2rw
```

Select **2 — Check free size** at the first menu to check the partitions. In version 3.7.2.1, `--check` does not select check mode. Choose the menu option explicitly; **Make/Install** starts another conversion.

## 6. Install NetHunter 2025.3

Prepare `kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip` as described in [Preparing the NetHunter ZIP](DOWNLOADS.md#preparing-the-nethunter-zip): remove its top-level `kernel-nethunter.zip` entry from a copy of the download. Leave the remaining archive contents intact.

This configuration combines the `oos-twelve` image with crDroid 9.9 / Android 13 and NetErnel v1. Compatibility of this exact combination is unconfirmed.

Copy the prepared ZIP back to the phone and install it through **Magisk → Modules → Install from storage**. Allow about **25 minutes** for installation. Wait for completion, reboot, and open the NetHunter app and its Kali terminal.

In the Kali terminal, run:

```sh
echo 'APT::Sandbox::User "root";' | sudo tee /etc/apt/apt.conf.d/01-android-nosandbox
apt install kali-linux-everything -y
apt update && apt upgrade -y
apt autoremove -y
```

The APT setting runs package downloads as root and reduces sandbox isolation. `kali-linux-everything` is a large package set; allow sufficient storage and download time.

## 7. Install NetErnel and its modules

Copy these two files to the phone:

```text
NetErnel_v1_opkona.zip
NetErnel_modules_opkona.zip
```

1. Install and open **SmartPack Kernel Manager 17.5**.
2. From its menu, flash `NetErnel_v1_opkona.zip`, then reboot.
3. In **Magisk → Modules**, install `NetErnel_modules_opkona.zip`, then reboot again.

Use both files from the same NetErnel release. The included modules identify the kernel version as `4.19.275-NetErnel_v1`; similarly named kernels are not interchangeable.

## 8. Load the external Wi-Fi drivers at boot

In **SmartPack → Script Manager**, create a script named `external_wifi_driver_load` with these commands:

```sh
insmod /system/lib/modules/rtl_drivers/88XXau.ko
insmod /system/lib/modules/rtl_drivers/8188eu.ko
insmod /system/lib/modules/rtl_drivers/8192eu.ko
insmod /system/lib/modules/rtl_drivers/8192fu.ko
insmod /system/lib/modules/rtl_drivers/8814au.ko
```

Save it, enable **On Boot**, and reboot. The same commands are in [`scripts/external_wifi_driver_load.sh`](scripts/external_wifi_driver_load.sh).

External Wi-Fi requires an adapter chipset supported by the installed modules and kernel. Use NetHunter only on devices and networks where testing is authorized.

## Final vbmeta step

From bootloader mode, reapply the verification flags with the same crDroid images:

```sh
./fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
./fastboot --disable-verity --disable-verification flash vbmeta_system vbmeta_system.img
./fastboot reboot
```

This disables verified-boot checks and reduces boot integrity protection. Vbmeta changes can require a data wipe; back up before repeating them.

## Files

- [`DOWNLOADS.md`](DOWNLOADS.md): version-specific downloads, image extraction, and ZIP preparation.
- [`reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf`](reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf): PDF installation manual.
- [`scripts/external_wifi_driver_load.sh`](scripts/external_wifi_driver_load.sh): Wi-Fi module-loading commands.
- [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md): project credits and bundled license notices.
