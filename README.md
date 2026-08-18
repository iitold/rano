# RANO - RAN Optimize

Desktop tool for analyzing radio network CM (Configuration Management) data: it reads export files from Ericsson Network Manager (ENM), Huawei MML, CSV/TXT, Excel... then stores them in a database, lets you filter by MO type and export to CSV/TXT/XLSX.

**Links:**
- Website: <https://ranoptimize.web.app>
- GitHub: <https://github.com/iitold/rano>
- Latest release: <https://github.com/iitold/rano/releases>

## Install

1. Download **`rano.exe`** from [Releases](https://github.com/iitold/rano/releases) (latest version).
2. Put `rano.exe` in any folder you like (e.g. `C:\RANO`, Desktop...). **No installation needed** - just run it.
3. Note: the folder must be writable by your user account (avoid `Program Files`, otherwise the app cannot self-update).

## Usage

Open `rano.exe`. The interface has these tabs:

- **Parse**: add data files/folders (Add Files / Add Folder), choose a parser (leave default *Auto-detect*), press **Parse**. A progress bar shows while running; you can cancel.
- **Records / MO**: browse parsed data and select the MO types you want to export.
- **Export**: export to **CSV / TXT / XLSX** into a folder you choose.
- **License**: view the Machine ID, activate a key, or use the trial.
- **About**: version info, check for updates.

Compressed inputs `.gz`, `.zip` are supported (decompressed automatically). Typical ENM data files are named `enm*`; Huawei MML files are `.xml`/`.txt`.

## License

RANO is licensed per machine.

1. Open the **License** tab and click **Copy** to get your **Machine ID** (unique per machine).
2. Send the Machine ID (with your name/company if you like) to the vendor by email: **doancongthanh92@gmail.com**.
3. Receive the key from the vendor, paste it into the **License key** field and click **Activate**.

Or use the trial: click **Start Trial** (7 days, once per machine), or **Request License** to ask the vendor for an official key.

> Note: a machine that used up its trial needs an official key. When online, the app re-verifies the license with the server; if the license is revoked or expired, the app asks you to activate again.

## Updates

The app updates itself through GitHub. When a new version is available:

1. Open the **About** tab -> click **Check for updates**.
2. If a new version exists -> click **Download vX and restart**.
3. The app downloads, verifies the SHA-256 checksum, replaces itself and restarts.

You can also download manually from [Releases](https://github.com/iitold/rano/releases).

## Release history

Every release on [Releases](https://github.com/iitold/rano/releases) has **Release notes** describing the changes (and includes the `CHANGELOG.md`). Recent versions:

- **v0.3.0 (18/08/2026)** — Auto-update, encrypted license with online check, website/releases links in About.
- **v0.2.0** — Renamed Operator -> Company, split contact info into separate fields.

## Generated files

- License file: `rano_license.json` (next to `rano.exe`, encrypted per machine).
- Temporary parse database: stored in the OS temp folder (cleaned up automatically).
- Logs: `logs\` folder next to `rano.exe`.

## Support

For any question about installation, licensing or bugs, contact: **doancongthanh92@gmail.com**.
