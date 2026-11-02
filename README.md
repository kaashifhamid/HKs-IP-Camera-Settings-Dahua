<!-- Images to upload: docs/logo.png, docs/screenshots/light-mode.png, docs/screenshots/dark-mode.png, docs/screenshots/settings.png -->

<p align="center">
  <img src="docs/logo.png" alt="HK's IP Camera Settings (Dahua) logo" width="120">
</p>

<h1 align="center">HK's IP Camera Settings <sub>(Dahua)</sub></h1>

<p align="center">
  <b>A Windows tool for Dahua IP cameras.</b><br>
  Find every camera on your network, log in to all of them at once, check their settings,
  take snapshots, sync their clocks and export everything to Excel.
</p>

<p align="center">
  <a href="https://github.com/kaashifhamid/HKs-IP-Camera-Settings-Dahua/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/kaashifhamid/HKs-IP-Camera-Settings-Dahua?label=latest&color=1f3864"></a>
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows%2010%20%2F%2011%20(x64)-2b4c87">
  <a href="https://github.com/kaashifhamid/HKs-IP-Camera-Settings-Dahua/releases"><img alt="Downloads" src="https://img.shields.io/github/downloads/kaashifhamid/HKs-IP-Camera-Settings-Dahua/total?color=22c55e"></a>
</p>

---

## ⬇️ Download

Current version: **1.29.0**. Go to the **[Releases page](https://github.com/kaashifhamid/HKs-IP-Camera-Settings-Dahua/releases/latest)** and download one of the two files:

| File | Best for | Notes |
|---|---|---|
| **`HK_IP_Camera_Settings_Setup_1.29.0_x64.exe`** | Normal use | Installer. Installs for your user only, **no administrator rights needed**. Adds a Start Menu entry and an optional desktop shortcut. |
| **`HK_IP_Camera_Settings_Portable_1.29.0_x64.zip`** | USB stick / no installation | Unzip anywhere and run `HK_IP_Camera_Settings.exe`. Logs and settings stay in a `data` folder next to the program. |

> **Requirements:** Windows 10 or 11, 64-bit. Your PC must be on the same network (or a routed network) as the cameras.
> No Python or other software is needed – everything is included.

### First start: Windows SmartScreen

The program is **not yet code-signed**, so Windows may show *"Windows protected your PC"*.
Click **More info → Run anyway**. You can confirm that the download is genuine by comparing its checksum (see [Verify your download](#-verify-your-download)).

---

## ✨ What it does

**1. Finds the cameras on the right network**
- Lists every network adapter of your PC (tick box, **adapter name** and **IP address**) and scans **only the networks you tick**. The range that is scanned is the network of the adapter; to scan a custom range, add it under **Extra ranges** in **⚙ Settings**, for example `192.168.1.50-192.168.1.120`.
- Virtual / VPN adapters are unticked by default. Networks up to **/16** (for example /23 or /22 subnets) are scanned **in full**. A larger network is narrowed to the /24 around your PC and a warning below the adapter table says so; you can add more ranges (and change the scanned ports) under **⚙ Settings**. Before a scan that is expected to take long (about 45 seconds or more), the program shows the estimated time and asks for confirmation, and a **Stop scan** button ends it at any time, keeping the devices found so far.

**2. One login for all cameras**
- Enter the username and password once – the app logs in to all devices found.
- **Lockout protection:** a camera that rejected the password is not tried again with the same credentials, so a typo cannot lock your cameras.
- Different password on one camera? Tick just that camera, enter its password and fetch again.

**3. Details before login, and only cameras in the table**
- **Scan** also uses Dahua's own network search (the same method as Dahua's search tools). Without any password, every device that answers shows its **model, serial number, firmware version, MAC address, device name and class** right after the scan.
- Only cameras (fixed, dome, PTZ, thermal and traffic/ITC) are shown. NVRs/DVRs, switches, door stations, access controllers and other brands are **hidden from the table**, whether they were recognised by the search or after login. They are listed with the reason in the Excel *Scan Info* sheet and the log.
- Devices that rejected the password stay in the table, with their details, so you can enter their own password and fetch again.
- Devices found by the network search outside the scanned range (for example a camera left on a wrong subnet) are shown and marked *other subnet*.

**4. Detailed camera information**

| Group | Fields |
|---|---|
| Identity | Camera name, device name, model, device class, serial number, firmware version, build date, hardware version, vendor. Optional columns (switch them on in Settings): **SD card slots**, **processor (SoC)**, **update serial** (firmware family code) |
| Network | IP address, MAC address, DHCP, subnet mask, gateway, network adapter it was found on, **SNMP v3** (Yes / No / Unknown – whether the camera offers SNMP v3, not whether it is enabled; *No* = the camera's firmware has no SNMP section), **P2P Enabled** (Yes / No / Not supported / Unknown – whether the camera's P2P / cloud service is switched on; optional *P2P Server*: the P2P server address stored in the camera), and the optional **SIRA Reference No** (see 12) |
| Streams | **Main + Sub 1–3:** resolution, FPS, bitrate, codec, bitrate mode, GOP, profile |
| Image | **WDR mode and WDR % (day / night)**, smart codec, day/night mode, exposure mode, anti-flicker, brightness, contrast, saturation |
| Clock | **Live device date & time**, difference to your PC, **Clock Status** (OK / wrong time zone / never set / off by ...), time zone, NTP on/off, NTP server, **NTP port**, NTP tolerance (the NTP server and port columns are optional: *Settings → Show NTP details*, off by default) |

**5. Clock check and time sync** – the **Clock Status** column shows at a glance which cameras have a wrong clock: *Wrong time zone* (the time itself is right), *Zone differs from PC*, *Never set* (factory date) or *Off by ...*. **Tick clock issues** ticks only those cameras, ready for **Sync PC Time**. Cameras that reject the password still show their clock (read without a password through ONVIF), but can only be synchronised after a login. Sync: one click sets the clock of all ticked cameras to your PC time and **verifies** the result. Tick **Also set time zone** next to the button to set each camera's time zone to your PC's as well (read back to confirm; the old and new zone are logged). Cameras that keep a wrong time (for example because NTP is enabled) are reported with the reason.

**6. Snapshots** – tick the cameras and click **Snapshot**. Every camera saves a JPEG with the **camera name in the bottom-right corner**, named like `2026-10-09 095822 AK0B69APAG01456.jpg` (date, time, serial number).

**7. Open web interface** – click **🌐 Open** in any row (or select several cameras and use **Open Web**) to open the camera's own web page in your browser.

**8. Excel export** – a **Summary** sheet (site name, counts of cameras, SNMP v3 Yes/No, P2P, NTP off, clock issues, cameras whose **device name / camera name is still the factory default**, failed logins, and a model table with the **firmware versions and the oldest build per model**), an **Issues** sheet (one line per problem per camera - clock status, NTP switched off, default device / camera name, firmware that differs from the other cameras of the same model, model not in the SIRA list, DHCP on, failed logins - with a suggested action; the matching cells in the Cameras sheet are marked **amber**), a **Cameras** sheet with one row per camera channel (IP Address and Model are the first two columns and stay frozen while you scroll; the Channel column is hidden - unhide it in Excel if you need it), and a *Scan Info* sheet that documents the scan (devices that were not included are listed with their **model and serial number**). Enter a **Site name** next to the login fields: it goes into the file name and the Summary. Choose whether to export all fields or only the visible ones.

**9. Edit a camera's names** – select one camera and click **Edit Camera...** (or right-click → *Edit camera name...*) to change its **on-screen camera name** and its **device name**. Each change is read back from the camera to confirm it, and the old and new values are written to the log.

**10. Stream settings in bulk** – tick any number of cameras and click **Stream Settings...** to change the **resolution, FPS, bitrate, codec, CBR/VBR and GOP** of the **main stream and sub streams 1–3** at once.
- A **preview** compares your values with what each camera supports (resolutions, maximum FPS, bitrate range) and shows exactly what will change. Values a camera cannot take are marked in red with the reason, and nothing is written until you click **Apply**.
- After writing, every value is **read back** from the camera; if a camera adjusts a value, you see what it stored instead.
- **Undo last change** restores the previous values.

**11. NTP settings in bulk** – tick any number of verified cameras and click **NTP Settings...** to set the **NTP server** and **NTP port** of all of them at once, and (optionally) switch **NTP on** where it is off. Leave the server or port empty to keep each camera's own value. It works like Stream Settings: **Preview** shows what will change per camera, **Apply** writes it and reads every value back to confirm, and **Undo last change** restores the previous values. The time zone is not changed. Cameras whose firmware has no NTP section are listed as not possible.

**12. SIRA reference number (optional)** – *Settings → Show SIRA reference number* adds a **SIRA Reference No** column (and puts it in the Excel file). *Settings → Refresh SIRA list* downloads the Dubai SIRA lists for **IPC and NVR** (model and reference number) from Dahua, shows the time of the last synchronisation and saves the list on the PC, so the column works **offline** afterwards. The NVR list is **stored for now** and is not shown in the Cameras table. Models are compared **without a leading `DH-` or `DHI-`** and ignoring capitals (`DH-IPC-HFW3649T-ZS-IL` = `IPC-HFW3649T-ZS-IL`, `DHI-NVR2216-16P-I2` = `NVR2216-16P-I2`); a model with several references shows all of them. Five model families are **built in as the same model** (the SIRA list only has the version with the *P*, for example `IPC-HDBW2231RP-ZS`, while the camera reports `IPC-HDBW2231R-ZS`): `IPC-HDBW2231RP-ZS`/`R-ZS`, `IPC-HDW2239TP-AS-LED-S2`/`T-AS-LED-S2`, `IPC-HFW2239MP-AS-LED-B-S2`/`M-AS-LED-B-S2`, `IPC-HFW3441MP-AS-SFC-I2`/`M-AS-SFC-I2` and `IPC-HDBW4431RP-ZS`/`R-ZS`; with or without `DH-` all four spellings show the same reference (taken from the downloaded list). Apart from these sets, only exact model matches are used: a variant such as `…-S4` is **not** given the reference of its base model and shows *Not in SIRA list*. The list is also **updated automatically in the background every time the program starts** (the window never waits for it; with no internet the saved list is used). *Settings → Update the SIRA list automatically when the app starts* switches this off, and shows the result of the last automatic sync next to the *Refresh SIRA list* button. Each of the two lists (IPC, NVR) is refreshed on its own: a list that cannot be downloaded keeps its **last successful download** and the other one is still updated (a message says which one was kept). If the SIRA address changes or the server does not answer as expected, the old list is kept and the log contains the line `SIRA refresh FAILED` with the address, HTTP status and the start of the reply.

**13. Compare scans** – *Compare Scans...* compares an **earlier Excel file** exported by this program with the scan in the window (or with another Excel file) and lists the cameras that are **new**, **no longer found**, **replaced** (a different device on the same IP address) and **changed**, with what changed (IP address, names, firmware, NTP, P2P, time zone, DHCP, SIRA reference, main / sub stream resolution, FPS, bitrate and codec). Cameras are matched by **serial number**, so a camera that got a new IP address is still the same camera. *Export comparison to Excel...* saves the result. Only fields that exist in both files are compared.

**14. Rename cameras in bulk** – tick cameras and click **Rename Cameras...** (in the **Bulk changes** box). Either (a) *Save template for the ticked cameras...* writes an Excel file with the current names - edit the yellow **Camera Name** / **Device Name** cells you want to change, save it and click *Load names file...* (a CSV or Excel file with the columns *IP Address* or *Serial Number*, *Channel* for multi-channel devices, *Camera Name* and / or *Device Name* works too; empty cells are left unchanged); or (b) tick *Set the device name = the camera name* to replace device names that are still the default (for example the serial number) with the camera name. **Preview** lists every change, **Apply** writes it and reads each name back to confirm, and **Undo last rename** restores the previous names. The camera name is the title of the video channel; the device name is the name of the device itself.

**15. Security** – (a) **Read-only mode** (switch in the window header): no request that changes a camera is sent, the buttons that change cameras are greyed out and a red banner is shown; the choice is remembered. (b) **Unencrypted logins**: if a camera only accepts plain HTTP with Basic authentication, the password would travel unencrypted - the program asks first (send anyway / skip those cameras / stop). When a camera answers on both HTTP and HTTPS, **HTTPS is used**. The *Login Method* column (Settings → Table columns → *Security* group) shows how each camera was logged in to. (c) **Security checks** in the *Issues* sheet (category *Security*): firmware build older than 24 months, P2P (cloud access) switched on, SSH on, UPnP on, HTTPS off, camera managed over plain HTTP; the read-only columns *SSH Enabled*, *UPnP Enabled* and *HTTPS Enabled* show the values (*Not supported* = the firmware has no such section, *Unknown* = no usable answer; the log lines `SECURITY check` show what the camera answered). (d) **Diagnostic bundle**: *Settings → General → Anonymise the bundle* (on by default) replaces IP addresses (the network structure stays readable), MAC addresses (manufacturer part kept), serial numbers, camera / device names, user names, the Windows user folder, the PC name and the site name in file names. The key to the replacements is saved **next to the bundle** as `…_KEEP-PRIVATE_key.csv` - keep it, do not send it. (e) **Log retention**: *Settings → General → Delete log files older than N days* (default 30; 0 = never). The program never tries default passwords, because that could lock accounts.

**16. LLDP and SSH on / off in bulk** – the read-only column **LLDP Enabled** (*Settings → Table columns → Security*; *Yes / No / Not supported / Unknown*) shows whether LLDP (Link Layer Discovery Protocol, which lets network switches see the camera's name, model and port) is switched on. **LLDP / SSH...** (second button row) switches **LLDP or SSH on or off** for all ticked cameras (choose the service, then On or Off; *SSH Enabled* is the matching column; switching SSH **on** asks for a confirmation - keep SSH off in normal operation): **Preview** reads the current value of each camera, **Apply** writes it and reads it back to confirm, **Undo last change** restores the previous values. It uses the camera web page's own method (RPC2 `configManager`, settings `LLDPGlobal` and `SSHD`, LLDP confirmed on cameras with V2.820, V2.840 and V3.146 firmware) and falls back to the CGI way. The camera's whole settings table is read and written back with only the on/off value changed (other values such as the SSH port are kept). Cameras that have no such setting are listed as not possible. Blocked in read-only mode.

**17. Audio check (microphone and speaker)** – read-only columns in *Settings → Table columns → Audio* (the first three are shown by default on a new installation): **Audio Input (Mic)** (*Yes / No / Unknown* – does the camera report an audio input), **Mic Status** (*On / Off (audio disabled) / Off (volume 0) / n/a*; *Audio on (mic unconfirmed)* when the camera does not say whether it has a microphone), **Speaker Status** (*On / Off (volume 0) / Not supported / Unknown*), plus **Mic Volume** and **Speaker Volume**. *Off (audio disabled)* means audio is switched off in the video stream settings of the camera. The **Issues** sheet of the Excel report lists microphones and speakers that are off (category *Audio*). Note: Dahua cameras report an *audio input*; the camera does not tell in a uniform way whether it is the built-in microphone or an external Line-In, so *Yes* means "has an audio input". Details of what each camera answered are written to the log (lines starting with *AUDIO check*).

**18. Comfort**
- **Settings page** to choose which fields are shown in the table, and to set **extra scan ranges** and **ports**.
- A clean layout with the compact adapter table **beside** the Scan & login section (username, password, site name and the scan buttons), a large device table, larger entry fields and rows, and only **Scan + Fetch** highlighted.
- **Filter box** above the device table to find cameras by IP, model, name or status. Hidden rows are never ticked or used by actions.
- **Collapsible log** at the bottom (click **Expand**); everything is still saved to the log file.
- The device table has the **Status** and **Web** columns at the end.
- **Dark mode** with an animated switch.
- Tick boxes with *Select all / Deselect all*, resizable columns, centered table.
- **Everything is logged** to a daily log file, also after the app is closed – including every failed request with the camera's reply, a session ID, information about the PC, and a crash log. A **diagnostic bundle** (logs + settings + PC information, no passwords) can be saved from *Settings* in one click, and *Detailed logging* can record every request when a problem is being investigated.

**19. Update check (optional, off by default)** - *Settings → General → Updates → Check for a new version when the app starts*. When switched on, the program asks GitHub **once at start** (in the background, the window never waits for it) for the latest published version. If it is newer than the one you run, a small **↑ v1.xx.x available** link appears in the window header (click it to open the download page) and the version is also shown in the window title and the log. **Check now** in the same place does the check on request, even when the automatic check is off, and shows the result under the button. Nothing is downloaded or installed by the program - you install the new version yourself from the Releases page. If the check fails (no internet, GitHub busy), nothing is shown except a line in the log and the text under *Check now*.

**20. Column presets** - the list **Column preset** (second row above the table) switches the table between saved sets of columns in one click: *Default*, *Audit*, *Network*, *Streams*, *Security*, *Clock* and *Everything*. Each one keeps **Status, Camera Name and Model**. *Settings → Table columns → Preset* has **Save as...**, **Rename** and **Delete** for your own presets (they are saved at once; the built-in ones cannot be changed - save a copy under your own name instead). The list shows **Custom** when the ticked columns match no preset. The Excel export (*only the visible fields*) follows the columns you have chosen.

**21. Copy rows** - **Ctrl+C** copies the **highlighted rows** (or, when none are highlighted, the **ticked** rows) as tab-separated text with a header row, which pastes straight into Excel cells; **Ctrl+A** highlights every row shown. The **right-click menu** also has *Copy selected rows*, *Copy ticked rows*, *Copy all shown rows*, *Copy this column* (one value per line) and **Copy as table (email / Teams)**, which puts a formatted table on the clipboard so it pastes as a real table into an email or a Teams message (Windows; Notepad and Excel get the same data). The copy contains the IP address and the columns you see, in the order shown, the **full** Status text, and only the rows that are shown (rows hidden by the filter are never copied). Right-clicking a row that is already highlighted keeps the whole highlight; *Edit camera name...* needs a single highlighted row.

**22. Retry on timeout and live progress** - while **Login & Fetch** runs, the **Status** column shows what each camera is doing: *Waiting*, *Logging in*, *Reading settings*, *Retrying (1 of 1)*. A **read** that times out or is reset is tried again after a short pause with a longer wait (*Settings → General → Login and fetch → Retries on timeout*: 0, 1 or 2, default 1; 0 = no retry). **Nothing that changes a camera is ever retried, the login call itself is not retried automatically, and a wrong password is never retried**, so the lockout protection is untouched. A camera that does not answer the login in time is shown as **Timed out** (before: *Error*). If some reads still failed after the retry, the camera is shown in **amber** as *OK (n reads timed out - some fields may be empty)* and appears in the Excel **Issues** sheet, so a half-empty row is never mistaken for a complete one. The button **Retry timed-out (n)** above the table reads just these cameras again (not the ones that rejected the password). After two reads in a row fail even after the retry, the program stops retrying for that camera, so one hanging camera cannot slow the whole run down.

---

## 🖼️ Screenshots

| Light mode | Dark mode |
|---|---|
| ![Light mode](docs/screenshots/light-mode.png) | ![Dark mode](docs/screenshots/dark-mode.png) |

Switch between the two with the sun / moon switch in the top-right corner of the window.

**Settings page** – choose which fields are shown in the device table, whether the Excel export uses only the visible fields, where snapshots are saved, and the extra scan ranges and ports:

<p align="center">
  <img src="docs/screenshots/settings.png" alt="Settings page" width="800">
</p>

---

## 🚀 Quick start

1. **Install** (or unzip the portable version) and start **HK's IP Camera Settings**.
2. In **① Network adapters**, tick the adapter that is connected to your camera network.
3. In **② Scan & login settings**, enter the camera **username** and **password**. The default ports `80, 8080, 443` fit most cameras; change them (or add extra ranges) in **⚙ Settings**.
4. Click **Scan + Fetch**. The cameras appear in the table (green = verified IP camera, red = needs attention – click the row for the reason; NVRs, switches and other non-camera devices are not shown).
5. Use the buttons (the **Scan & login**, **Ticked cameras** and **Bulk changes** boxes at the top):

| Button | What it does |
|---|---|
| **Scan** | Searches the network and shows model, serial number, firmware and MAC address of the cameras found (no login) |
| **Login & Fetch** | Logs in to the ticked devices and reads their details |
| **Scan + Fetch** | Both steps in one go |
| **📷 Snapshot** | Saves a JPEG of every ticked camera |
| **🌐 Open Web** | Opens the web pages of the ticked / selected cameras |
| **🕒 Sync PC Time** | Sets the ticked cameras' clock to your PC time (tick **Also set time zone** to set the time zone too) |
| **Tick clock issues** (above the table) | Ticks only the verified cameras with a wrong clock or time zone |
| **Export Excel** | Saves the ticked cameras to an `.xlsx` file, with a Summary sheet and the site name |
| **Edit Camera...** | Changes the camera name and device name of one camera |
| **Stream Settings...** | Changes resolution, FPS, bitrate, codec, CBR/VBR and GOP of the main / sub streams of the ticked cameras (with preview and undo) |

**Tips**
- Tick boxes decide what is exported, photographed or synchronised.
- **Double-click a cell** to copy only that cell's text to the clipboard (for example a model, serial number or MAC address); the copied text is shown under the table. *Settings → Double-click a cell copies its text* switches this off, and then a double-click opens the camera's web page as before. Click **🌐 Open**, the *Web* column or right-click → *Open web interface* to open the web page at any time.
- **Ctrl+C** copies the highlighted rows (or the ticked rows) for Excel; **Ctrl+A** highlights all rows; **right-click** for *Copy ... rows*, *Copy this column* and *Copy as table (email / Teams)*.
- **Column preset** (second row above the table) switches the columns between *Audit*, *Network*, *Streams*, *Security*, *Clock* and your own sets.
- Drag the column borders to resize columns; open **⚙ Settings** to hide fields you don't need.

---

## ⚙️ Settings, logs and portable mode

| Item | Location |
|---|---|
| Logs (daily files, kept 90 days) | `%APPDATA%\HK_IP_Camera_Settings\logs` – open it with the **📁 Logs** button |
| Settings | `%APPDATA%\HK_IP_Camera_Settings\settings.json` |
| Crash log | `%APPDATA%\HK_IP_Camera_Settings\logs\crash.log` |
| Diagnostic bundle | **⚙ Settings → Save diagnostic bundle...** (a zip with logs, settings without the user name, and PC information – no passwords) |
| Portable version | the `data` folder next to `HK_IP_Camera_Settings.exe` (because of the included `portable.txt`) |

The installer's uninstaller **keeps** your logs and settings.

---

## 🔒 Privacy and safety

- The program talks **only to the cameras on your own network** (it sends a local search message to find them; it does not leave your network). It makes **two** kinds of internet connection, and nothing else: (1) the download of the public SIRA list from Dahua's SIRA server (see 12), and (2) the **optional, off-by-default update check** (see 19), a single request to GitHub for the latest published version. Nothing about your cameras, network or PC is sent in either, and the program has no telemetry. As with any website you visit, the server sees the public IP address of your connection, and the update request also carries the program name and version number. The automatic SIRA download and the update check can each be switched off in *Settings*, and the program works fully without internet.
- The **password is never saved** – not in the settings file and not in the log.
- The program only **reads** from your cameras, except for the actions you start yourself: **Sync PC Time** (changes the camera clock and, if ticked, its time zone), **Edit Camera** (changes names) and **Stream Settings** (changes stream parameters, after a preview and a confirmation, with read-back and undo). Every change is written to the log with the old and new value.
- Logs and diagnostic bundles contain device information (IP, model, serial number, firmware) but **never passwords**. Keep that in mind before sharing them.

---

## ❓ Troubleshooting

| Problem | What to try |
|---|---|
| A camera shows **Login failed – Wrong username/password** | That camera uses a different password. Tick only that camera, enter its credentials and click **Login & Fetch**. |
| The **SIRA Reference No** column says *SIRA list not loaded* / *Not in SIRA list* | *Not loaded*: open *Settings* and click *Refresh SIRA list* (internet needed; it also happens automatically at every start). *Not in SIRA list*: the exact model is not in the list. If *Refresh SIRA list* fails, open the log and send the line `SIRA refresh FAILED`. |
| A camera shows **Login failed (3 failed logins this session)** | The program counts the wrong logins per camera in this run. From the third wrong login it asks before trying again, because many Dahua cameras lock the account for about 30 minutes after a few wrong passwords. The count starts again when the program is restarted; it does not know about attempts made by other PCs or by the NVR. |
| A camera shows **Access refused (HTTP 403)** | The account is most likely **locked** after failed logins. Wait about 30 minutes or restart the camera, then try again. |
| The scan of a large network is slow | A /16 network (65,534 addresses) takes several minutes with three ports. Use **Stop scan** to end it early, remove ports you do not need under **⚙ Settings**, or scan only the ranges you need. |
| No cameras found | Make sure the right adapter is ticked, the PC and cameras are in the same subnet (or add a range under *Extra ranges* in **⚙ Settings**), and the ports match the cameras' HTTP port. |
| **SNMP v3** shows *No* | The camera's firmware has no SNMP section (the camera answers "Bad Request"). Some models can get a custom firmware with SNMP. |
| **SNMP v3** shows *Unknown* | The camera did not answer the SNMP request or returned something unexpected. Open the log, find the line `SNMP v3 check <ip>` and send it with the camera model so the detection can be extended. |
| **P2P Enabled** shows *Unknown* or *Not supported* | *Not supported* = the camera's firmware has no P2P section. *Unknown* = the camera did not give a usable answer. Open the log, find the lines `P2P check <ip>` and send them with the camera model so the detection can be extended. |
| A device is missing from the table | NVRs/DVRs, switches, door stations and non-Dahua devices are not shown as cameras. They appear with the reason in the Excel *Scan Info* sheet and in the log. |
| Model / serial / firmware are empty after **Scan** | The camera did not answer the network search (Wi-Fi isolation, another subnet, or a firewall that blocks UDP). Windows may ask whether the program may use the network: allow it on **private** networks. Login & Fetch still reads everything with the password. |
| **Clock Status** says *Wrong time zone* | The camera's time itself is right but its zone is not (new cameras often have GMT+08:00). Tick **Also set time zone** and run **Sync PC Time**. |
| The program does not open a second window | By design only one window runs at a time; a second start brings the open window to the front. |
| *Sync PC Time*: **clock still off** | The camera accepted the new time but its clock did not move. The message in the log tells the likely reason: **NTP enabled** on the camera (check the NTP server / time zone), or the camera is kept in time by an **NVR / recorder** (check the NVR's time-sync option). Run *Sync PC Time* again for those cameras. |
| *Sync PC Time* with **Also set time zone**: "time zone not available" / "not changed" | The PC's UTC offset is not in the camera's time zone list, or the camera kept its old zone. Set it in the camera's web page. Daylight-saving changes are not handled by the camera setting. |
| *Stream Settings*: a value is marked red or "adjusted" | Red = the camera does not support that value (resolution list, maximum FPS and bitrate range are checked per camera) – the reason is shown. "Adjusted" = the camera accepted the change but stored a different value; the table shows what it kept. Cameras recorded by an NVR may be controlled by the NVR – try one camera first. |
| Excel export or the snapshot folder window does not open | Some PCs cannot show the standard Windows file dialog. The app then uses its own dialog, or saves automatically to `Documents\HK IP Camera Settings` (Excel) or the snapshot folder from *Settings*, and says so in the log. |
| Snapshot has a different size than the main stream | The snapshot comes from the camera's snapshot function; some models use another resolution. |
| The update check says **Could not check** | *Settings → General → Updates* shows the reason: no internet, a proxy or firewall that blocks github.com, GitHub limiting requests from a shared network (try again later), or no release published yet. The details are in the log (line `Update check failed`). |
| The Status says **Timed out** | The camera did not answer the login in time. It may be busy, restarting, or the network blocks the web port. Click **Retry timed-out** (above the table) after a moment; it reads only those cameras again. |
| The Status says **OK (n reads timed out - some fields may be empty)** | The camera logged in, but some settings did not arrive even after the retry (amber row). Click **Retry timed-out**; if it happens again, raise *Retries on timeout* in Settings or check the network and the load on the camera. The log names the reads that failed (line `timed out even after the retry`). |
| **Copy as table** pastes tab-spaced text, not a table | The table format needs Windows and a program that accepts formatted text (Outlook, Teams, Word, Excel). The status line under the table says when the program had to fall back to plain text. |
| Windows warns when starting | See [First start: Windows SmartScreen](#first-start-windows-smartscreen). |

---

## 🔐 Verify your download

Compare the SHA-256 checksum of the file you downloaded with the values below (PowerShell):

```powershell
Get-FileHash .\HK_IP_Camera_Settings_Setup_1.29.0_x64.exe -Algorithm SHA256
```

**Version 1.29.0**

| File | SHA-256 |
|---|---|
| `HK_IP_Camera_Settings_Setup_1.29.0_x64.exe` | `0153c0b482853e329674c6ea13dcf4fb6b82e7dc94a1901eb2e7f0c1da088a58` |
| `HK_IP_Camera_Settings_Portable_1.29.0_x64.zip` | `1f46f99f81e209b5a91eb90903e25fdb927cc56cfd91e9d1eadcc22249404025` |

---

## 📝 Release notes

| Version | Highlights |
|---|---|
| **1.29.0** | **New:** the **HK logo** - the new sticker logo replaces the old blue camera icon as window / taskbar icon (sizes 16 to 128 px) and as the logo in the window header (sharper than before). No other change. |
| **1.28.0** | **New:** live progress per camera in the Status column during Login & Fetch (*Waiting, Logging in, Reading settings, Retrying*); a read that times out is tried again (*Settings → General → Login and fetch → Retries on timeout*, 0-2, default 1) - never a change to a camera, never the login call, never a wrong password; **Timed out** is now its own result; incomplete readings are shown in amber and in the Issues sheet; button **Retry timed-out (n)**. |
| **1.27.0** | **New:** **Ctrl+C** copies the highlighted (or ticked) rows as tab-separated text, **Ctrl+A** highlights all rows; right-click: *Copy selected / ticked / all shown rows*, *Copy this column*, **Copy as table (email / Teams)**. **Changed:** right-clicking a highlighted row keeps the whole highlight. |
| **1.26.0** | **New:** **column presets** - *Default, Audit, Network, Streams, Security, Clock, Everything* and your own (*Settings → Table columns → Preset*: Save as, Rename, Delete); the list is next to the table. |
| **1.25.0** | **New:** optional **update check** (*Settings → General → Updates*, off by default): once at start the program asks GitHub for the latest published version and shows a small link in the header (and in the window title) when a newer one exists; **Check now** does it on request. Nothing is downloaded or installed, and nothing about your PC or cameras is sent. |
| **1.24.0** | **New:** double-click a cell copies only that cell's text to the clipboard (*Settings → Double-click a cell copies its text*, on by default; off = a double-click opens the web page as before). |
| **1.23.0** | **New:** the **NVR SIRA list** is downloaded together with the IPC list and stored (not shown in the table yet); `DHI-` is ignored like `DH-` when comparing models (`DHI-NVR2216-16P-I2` = `NVR2216-16P-I2`); each list keeps its last successful download when it cannot be refreshed; the SIRA requests use the form-urlencoded Content-Type of the SIRA web page (the old type is tried if that is refused). |
| **1.22.0** | **Removed:** the lights check (columns *White Light*, *IR Light*, *Light Type* and *AI Mix Light* and the *Lights* group in Settings > Table columns). |
| **1.21.2** | **Changed:** cameras with old firmware that only have the old *Lighting* setting (e.g. IPC-HDBW5442E-ZE) are shown as *IR only (old firmware)* instead of *Unknown*. |
| **1.21.1** | **Fixed:** the lights check showed a white light for cameras that only list an empty white-light entry (e.g. DH-IPC-HDW8441X-3D); they now show *IR only*. |
| **1.21.0** | **New:** lights check - columns *White Light*, *IR Light*, *Light Type* and *AI Mix Light* (Settings > Table columns > Lights). |
| **1.20.0** | **New:** audio check - columns *Audio Input (Mic)*, *Mic Status*, *Speaker Status*, *Mic Volume*, *Speaker Volume* (Settings > Table columns > Audio) and *Audio* entries in the Issues sheet for microphones / speakers that are switched off. |
| **1.19.0** | **Changed:** more compact top of the window. *Scan & login* is smaller (the *Show* tickbox is replaced by an **eye icon inside the password box**), and the **Ticked cameras** and **Bulk changes** buttons now sit beside it in one row with the network adapters, instead of in two bars; the progress bar and the *Stop scan* button moved to the window header. The *Devices & cameras* table gets the freed space. |
| **1.18.1** | **Changed:** new app logo and program icon. |
| **1.18.0** | **Removed:** the firmware upgrade option (button, dialog and all related code). **Changed:** the network adapters table is compact (tick box, adapter, IP address) and sits **beside** the Scan & login section; the Devices & cameras table uses the freed space. |
| **1.17.0** | **New:** **SSH** on / off in bulk - the LLDP dialog became **LLDP / SSH...**; *SSH Enabled* is now read with the web page's method as well. |
| **1.16.0** | **New:** **LLDP** - column *LLDP Enabled* and **LLDP Settings...** to switch LLDP on or off for the ticked cameras (preview, read-back check, undo). |
| **1.15.1** | **Changed:** *HTTPS Enabled* shows *Yes* when the program itself is logged in over HTTPS (some firmware has no HTTPS setting section); a device that answers Dahua's network search but not the web login now says so instead of *Unreachable: ReadTimeout*. |
| **1.15.0** | **Fixed:** the program could not be restored after being minimised while a dialog was open (dialogs are no longer modal). **New / changed:** Settings is one page - *General* on the left, *Table columns* on the right as collapsible groups (tick a whole group with its checkbox); the checkbox in the header of the adapter and device tables shows *none / some / all* (☐ / ⊟ / ☑); refreshed look (colours, buttons, table header) in light and dark mode. |
| **1.13.1** | **Changed:** in *Settings → General* the *Show SIRA reference number* tickbox is now on the left and *Show NTP server / port columns* on the right. |
| **1.13.0** | **New:** **Read-only mode** (header switch); a warning before a password would be sent unencrypted (plain HTTP + Basic) and HTTPS preferred when available; *Login Method*, *SSH Enabled*, *UPnP Enabled*, *HTTPS Enabled* columns and **Security** checks in the Issues sheet (old firmware build, P2P on, SSH / UPnP on, HTTPS off, plain HTTP); **anonymised diagnostic bundle** with a separate private key file; **log retention** (default 30 days). **Changed:** Settings has two tabs (*Table columns*, *General*). |
| **1.12.0** | **New:** Excel **Issues** sheet with amber marks in the Cameras sheet; **Compare Scans...** (earlier Excel file against the scan in the window or another file: new / removed / replaced / changed cameras, exportable); **Rename Cameras...** (camera and device names in bulk from an Excel / CSV file or device name = camera name; preview, read-back check, undo). **Changed:** the bulk actions (Stream Settings, NTP Settings, Rename Cameras) are in a second button row; on small screens the window starts maximised. |
| **1.11.2** | **New:** five model families are built in as "same model" sets for the SIRA reference (with / without `DH-`, with / without the *P*), so all four spellings of each show the same reference. |
| **1.11.1** | **New:** the SIRA list is updated automatically in the background every time the program starts (never delays the window; a failure keeps the saved list and is logged); *Settings* has a tickbox to switch this off and shows the result of the last automatic sync. |
| **1.11.0** | **New:** optional **SIRA Reference No** column with *Refresh SIRA list* in Settings (offline list, `DH-` ignored, failures logged with details); optional **P2P Server** column (*P2P Online* removed - the cameras do not report it); Excel *Summary* counts cameras with a default device name / camera name. **Changed:** in the Excel file Model is column B, columns A-B are frozen, the Channel column is hidden and the Summary model table has no *SNMP v3 No* column; the Settings window keeps its buttons and General box visible on small screens. |
| **1.10.0** | **New:** **NTP Settings...** - set the NTP server, NTP port and NTP on/off of all ticked cameras at once (preview, read-back check, undo); new *NTP Port* column and a *Show NTP details* option (off by default). **Changed:** Main and Sub1 resolution / FPS / bitrate are switched on once after the update; failed logins show the camera's model and serial number (table, log, Excel *Scan Info*), the wrong logins per camera are counted and the program asks before a further attempt that could lock the account; cameras that simply lack a feature are logged as information instead of warnings; `crash.log` only keeps real crashes; Excel *Summary* shows firmware versions and the oldest build per model; the window can no longer be made narrower than the button bar. |
| **1.9.1** | **New:** **P2P status** - new column *P2P Enabled* (Yes / No / Not supported / Unknown: is the camera's P2P / cloud service switched on) and optional column *P2P Online*; P2P counts on the Excel *Summary* sheet. Read-only: nothing is changed on the cameras, and the raw P2P reply is written to the log. |
| **1.9.0** | **New:** networks up to /16 are scanned in full (larger ones are narrowed to the /24 around the PC, now with a visible warning and a *narrowed* mark in the adapter table); **time estimate and confirmation** before long scans; **Stop scan** button. |
| **1.8.0** | **New:** **Clock Status** column and **Tick clock issues** button; the clock of cameras that reject the password is read without a password (ONVIF); **site name** and a **Summary** sheet in the Excel export; optional columns *Processor (SoC)*, *SNMP Enabled*, *SNMP v3 Enabled* and *Update Serial*. The Excel export now uses the live clock values. |
| **1.7.1** | **New:** optional column **SD Card Slots** (how many memory cards the camera supports; hidden by default, switch it on in **⚙ Settings**). |
| **1.7.0** | **New:** Scan uses Dahua's network search, so model, serial number, firmware, MAC address, name and class are shown **without a password**; non-camera devices (NVR, switch, door station ...) are **hidden from the table** and listed on the *Scan Info* sheet; cameras on another subnet are marked; *MAC Address* is shown by default. **Faster start:** no slow Windows system query (also stops the messages in `crash.log` on some PCs); **only one window** at a time. |
| **1.6.2** | **Fix:** *SNMP v3* now shows **No** for cameras whose firmware has no SNMP (they answer "Bad Request" for it) instead of *Unknown*; *Unknown* is kept for cameras that do not answer. Checked against logs from 7 camera models. |
| **1.6.1** | **New:** column **SNMP v3** (Yes / No / Unknown: does the camera offer SNMP v3; the raw SNMP reply is written to the log for checking). **Change:** Start IP / End IP are read-only table columns (custom ranges via *Extra ranges* in Settings). |
| **1.6.0** | **New look:** network adapters in a full-width, evenly spaced table with **Start IP / End IP**; username, password and scan buttons on one line; buttons grouped by purpose; larger entry fields and table rows; **filter box** for the device table; collapsible log. |
| **1.5.1** | **Layout:** network adapters and scan & login side by side, Notes column removed, larger device table, **Status** and **Web** columns moved to the end, extra ranges and ports moved to **⚙ Settings**. **New:** *Sync PC Time* can also set the camera's **time zone** (option next to the button, verified by read-back). |
| **1.5.0** | **New:** edit a camera's name and device name; **bulk stream settings** (resolution, FPS, bitrate, codec, CBR/VBR, GOP for main and sub streams) with capability check, preview, read-back and undo; **more detailed logs** (failed requests with the camera's reply, session ID, PC information, crash log, optional full request trace) and a one-click **diagnostic bundle**. |
| **1.4.3** | **Fixes:** Excel export and snapshot folder now work even when Windows cannot open its file dialog ("Unspecified error"). *Sync PC Time* now reads the clock back several times, retries once and shows the real reason when a camera keeps its old time. |
| **1.4.2** | Native 64-bit program and 64-bit installer, portable x64 package. |
| **1.4.1** | Traffic (ITC) cameras recognised; *Sync PC Time* now verifies the result and explains NTP cameras; new fields: time zone, NTP enabled / server / tolerance. |
| **1.4.0** | New name *HK's IP Camera Settings*; persistent log file; live device clock; *Sync PC Time*; Settings page; per-row *Open*; modern scrollbars; animated dark mode. |
| **1.3.0** | Logo, unified tick boxes, snapshots with camera-name overlay, open web interface. |
| **1.2.0** | New table design, centered columns, lockout protection. |
| **1.1.0** | Discovery by network adapter, only verified Dahua IP cameras exported. |
| **1.0.0** | First release. |

### Planned

- ONVIF check (enabled / disabled) per camera
- Enable / disable P2P in bulk (the P2P status view is available since 1.9.1)
- Code-signed installer

---

## 💬 Feedback

Found a problem or have an idea? Please open an **[Issue](https://github.com/kaashifhamid/HKs-IP-Camera-Settings-Dahua/issues)** and include the version number (shown in the window title) and, if possible, the relevant lines from the log file.

---

## ⚖️ License and notices

© HK. All rights reserved. The program is provided **free of charge, as is, without warranty**. Use it only on networks and devices you are authorised to manage.

Dahua and other product names are trademarks of their respective owners. This is an independent tool.
