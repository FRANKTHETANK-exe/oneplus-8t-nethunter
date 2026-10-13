# OnePlus 8T NetHunter

Kali NetHunter on a **OnePlus 8T KB2005**, using **Android 13 / crDroid 9.9**, Magisk, RO2RW, and NetErnel with external USB Wi-Fi drivers. This guide follows the sequence in the [original four-page manual](reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf).

The [download guide](DOWNLOADS.md) has the exact versions, sources, and preparation instructions. Start there before connecting the phone.

## Before starting

- A OnePlus 8T KB2005, a Windows PC for MSM Tool, and a reliable USB data cable.
- A backup stored off the phone. Stock restore, bootloader unlock, and the RO2RW format step erase data.
- Android Platform-Tools 36.0.0, with a terminal open in its `platform-tools` folder. The commands below use the manual's `./adb` and `./fastboot` syntax.
- USB debugging enabled whenever Android is running; accept the computer's authorization prompt. Enable OEM unlocking before the bootloader-unlock step.
- The files in [DOWNLOADS.md](DOWNLOADS.md), with the ROM, Gapps, recovery, and vbmeta files copied into the terminal's working directory.

Flashing the wrong model, partition image, or slot can leave the phone unable to boot. These instructions use the manual's versions and KB2005 setup; other models and newer releases need their own compatibility checks. Keep the stock recovery bundle available before proceeding.

## 1. Restore stock Android 11 with MSM Tool

Use the **OxygenOS 11.0.7.9.KB05AA / `kebab_15_O.12_210201`** bundle from the download guide. Extract the whole bundle and keep its executable, DLLs, and `.ops` file together.

1. Open MSM Tool on the Windows PC.
2. Power off the phone.
3. Hold **Volume Up + Volume Down**, without holding Power, and connect the USB cable.
4. Wait for the phone to appear under **ENUM**, then select **Start**.
5. Let the restore finish. A black phone screen during this step is expected in the manual.

Set up Android 11 and **do not install an OxygenOS update** before continuing. If MSM does not detect the phone, stop at this stage and check the cable and Windows device/driver detection; the manual does not specify a separate USB-driver package.

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

The manual notes that the computer-side progress can stop around 47%. Check the recovery's completion message before continuing; the percentage alone does not establish success. Reboot to Android and allow several minutes for the first boot.

## 4. Install Magisk 27.0

Make the `.zip` and `.apk` copies described in the download guide.

```sh
./adb reboot recovery
```

Select ADB sideload in recovery:

```sh
./adb sideload Magisk-v27.0.zip
```

Reboot, install `Magisk-v27.0.apk`, complete Magisk's setup, and confirm root access. This is the manual's recovery method; [Magisk now marks recovery installation as deprecated](https://topjohnwu.github.io/Magisk/install.html#custom-recovery).

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

Use the selections recorded for **RO2RW 3.7.2.1**:

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

The manual's output was named `super-rw-sparse-fastboot-active-_b.img`. Use the output produced for the phone's actual active slot, not an image from another device. For the filename recorded in the manual:

```sh
./adb reboot bootloader
./fastboot flash super super-rw-sparse-fastboot-active-_b.img
./fastboot reboot recovery
```

In recovery, perform **Factory reset → Format data**, then boot Android. Confirm root access and check RO2RW from a root Termux shell:

```sh
su
ro2rw --check
```

## 6. Install NetHunter 2025.3

Prepare `kali-nethunter-2025.3-oneplus8-oos-twelve-full.zip` as described in [Preparing the NetHunter ZIP](DOWNLOADS.md#preparing-the-nethunter-zip): remove its top-level `kernel-nethunter.zip` entry from a copy of the download. Leave the remaining archive contents intact.

Install that prepared ZIP through **Magisk → Modules → Install from storage**. The manual allows about **25 minutes** for installation. Wait for completion, reboot, and open the NetHunter app and its Kali terminal.

Run the commands from the manual in the Kali environment:

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

The manual reports working external Wi-Fi after this step. It does not name the adapter model or include driver-loading logs, so support for a particular adapter still depends on its chipset and the installed kernel/modules. Use NetHunter only on devices and networks where testing is authorized.

## Final vbmeta step from the manual

The manual ends by repeating the vbmeta flash as a stability fix. From bootloader mode, with the same crDroid images:

```sh
./fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
./fastboot --disable-verity --disable-verification flash vbmeta_system vbmeta_system.img
./fastboot reboot
```

This disables verified-boot checks. It is not a general guarantee against boot problems, and vbmeta changes can require a data wipe; back up before repeating them.

## Files

- [`DOWNLOADS.md`](DOWNLOADS.md): version-specific downloads, image extraction, and ZIP preparation.
- [`reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf`](reference/OnePlus8T_NetHunter_Instruction_Manual_Final.pdf): original manual.
- [`scripts/external_wifi_driver_load.sh`](scripts/external_wifi_driver_load.sh): the five driver commands from the manual.
- [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md): project credits and bundled license notices.
