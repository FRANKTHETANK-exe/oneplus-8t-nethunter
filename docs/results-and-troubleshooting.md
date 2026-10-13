# Results, limitations, and troubleshooting record

## What the manual reports

The final page states that the OnePlus 8T KB2005 runs crDroid 9.9 / Android 13 with Magisk, RO2RW, Kali NetHunter, and external Wi-Fi support. It records a boot-time module-loading script and a final vbmeta workaround.

The supplied evidence supports describing this as a reported completed setup. It does not support claims about every adapter, packet injection, monitor mode, performance, battery life, long-term stability, or compatibility with other phones. None of those results is demonstrated in the PDF or by the presence of downloaded archives.

## What was verified during repository preparation

- All four PDF pages were rendered and visually inspected. They contain text and command blocks, with no screenshots or test logs.
- The PDF has no author/contact metadata, embedded files, or JavaScript entry. Its OpenAction is a page-view setting.
- The local folder contains the named ROM, GApps, Magisk, RO2RW, NetHunter, kernel, modules, and app files.
- Platform-Tools identifies itself as 36.0.0; the supplied binaries are for Windows.
- Filenames and checksums were inventoried without running or flashing their contents.

Local presence and local hashes do not establish upstream authenticity or successful installation.

## Recorded troubleshooting observations

| Observation in PDF | What remains unresolved |
| --- | --- |
| MSM Tool shows `ENUM`; the phone screen remains black | No restore screenshot, host drivers, tool selection, or restore log |
| ROM sideload may stop at 47% | No accompanying recovery result; percentage alone is insufficient |
| First ROM boot may take several minutes | No measured time or failure threshold |
| NetHunter install takes about 25 minutes | No installation log; treat this as an anecdotal duration |
| `ro2rw --check` is used after conversion | No output proving that the conversion succeeded |
| APT sandbox user is set to root | No original error or explanation for this workaround |
| Reflashing vbmeta is described as a stability fix | No crash report or before/after evidence; data-preservation and stability claims remain unverified |

No additional fixes have been invented from these observations. In particular, a generic recovery image or repeated partition flash is not an evidence-backed repair for an unexplained boot failure.

## Evidence to collect in a future authorized device session

Record the exact device model and ROM build, active slot, selected image paths and hashes, original download URLs, recovery version, kernel version/commit, Magisk version, RO2RW check output, and NetHunter install result. For the adapter, record model, chipset/USB ID, required modules, interface detection, and a basic connection result on an authorized network.

Keep serial numbers, IMEI, unlock tokens, account details, network names, MAC addresses, private IP addresses, and personal notifications out of public screenshots and logs. Capture errors and successful results separately so failures do not disappear from the project record.

## Reproduction blockers

1. Exact stock restore package and recovery/vbmeta image selection are not documented.
2. The generated `_b` super image and conversion logs are missing.
3. The NetHunter ZIP modification has no recorded member path or resulting checksum.
4. Compatibility among the OxygenOS-12-named NetHunter package, Android 13 ROM, and kernel is not independently verified.
5. The external adapter and concrete test results are unspecified.

These gaps do not prevent publishing a clearly labeled historical project record. They prevent presenting it as a tested, complete installation recipe.
