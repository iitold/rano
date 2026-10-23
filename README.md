# RANO — RAN Optimize (v1.2.0)

**RANO (RAN Optimize)** is a standalone, high-performance Windows desktop application and CLI toolkit built in Rust (`egui` / `eframe`) for **Radio Access Network (RAN) Configuration Management (CM), Performance Management (PM), Baseline Auditing, ENM Scripting (ECLI/SFTP), 5G/LTE Call Trace Analysis, and Automated Dump Pipelines**.

> **Tài liệu Hướng dẫn Sử dụng (Tiếng Việt, có ảnh minh họa từng bước):** [**RANO_User_Guide.pdf**](./RANO_User_Guide.pdf)

---

## Key Capabilities at a Glance

| Module (Tab) | Core Functionality | Supported Formats / Protocols |
| :--- | :--- | :--- |
| **1. Parse** | Multi-vendor streaming CM/PM parser into SQLite WAL database with per-MO filtering and formatted export | Ericsson CM XML, ENM `cmedit` Tab/Log, 3GPP PM XML, Huawei CM XML & MML, Nokia RAML2 XML, ZTE CM XML & XLS/XLSX, Generic CSV/TSV, `.gz` / `.zip` |
| **2. Baseline** | Multi-technology parameter auditing (`4G LTE`, `5G NR`, `4G ENDC`, `3G WCDMA`), custom rule manager, and `cmedit set` remediation script generator | Built-in & custom `.txt` / `.tsv` / `.csv` rule profiles (`bl/`), audits against Parse DB, Folder, Files, Excel, or SQLite DB |
| **3. ECLI** | Multi-session Ericsson ENM terminal (`enmscripting` / `cmedit` / `alarm` / `pm` / `mobatch`), split-pane dual console, Command Builder, and Saved Quick Commands | HTTPS / SSH / SOCKS5 Proxy to Ericsson ENM, 5 terminal color themes, drag-and-drop workspace upload |
| **4. SFTP** | Dual-pane file commander (Local Computer `<->` Remote ENM Server) sharing ECLI credentials & Global Proxy | SSH/SFTP over Direct or SOCKS5/HTTP Proxy, drag-and-drop upload, recursive transfer |
| **5. Trace** | **5G EN-DC & CU-CP** CellTrace/UETRace KPI analyzer & CR generator, **3GPP Protocol & Flow** analyzer (ASN.1 tree, UE Perf, TA Polar Radar, VoLTE Drops, Hex PDU Decoder), **DCG Archive Diagnostics**, and **XML Translations** | `.bin`, `.bin.gz`, `.pcap`, `.pcapng`, `.cap`, `.log`, `.txt`, `*_dcgm.zip`, `.tar.gz`, Ericsson Event XMLs |
| **6. Auto** | Scheduled or on-demand multi-ENM `cmedit` bulk fetch -> streaming parse -> merged export with live S-curve pipeline visualization | Multi-profile ENM SSH/ECLI fetch -> `.csv`, `.txt`, `.xlsx`, or `.db` (SQLite) |
| **7. KPI** | 4G LTE / 5G NR / 3G WCDMA PM statsfile KPI engine with Local & Remote ENM SFTP fetch, customizable formulas, Trend Charts, and **PRACH Distance Distribution** | 3GPP / Ericsson PM `statsfile.xml` / `.xml.gz`, custom formulas (`kpi_formulas.json`) |
| **8. About** | Per-machine license activation & 7-day trial, system diagnostics, and one-click GitHub SHA-256 verified self-update | `rano_license.json` (AES-GCM encrypted), GitHub Releases OTA update |

---

## Global Header & Workspace Controls

Across all tabs, RANO provides three persistent workspace controls:

1. **Global Proxy Switch (Top-Right Header):**
   - Click the **sliding switch** to toggle between **Direct Network** (`Proxy: OFF`) and **Global Proxy** (`Proxy: 127.0.0.1:1080` / `socks5://...`).
   - Click the **Gear icon** (or right-click the pill) to open **Global Proxy Settings** and configure your `SOCKS5` or `HTTP` proxy URL (`e.g. socks5://127.0.0.1:1080` or `127.0.0.1:4920`).
   - Automatically applies to **ECLI**, **SFTP**, **Auto**, **KPI Remote Fetch**, and **Trace 5G SFTP Collector**.
2. **Interactive Table Headers (`Sort`, `Resize`, and `Fit`):**
   - **Click any column header** to sort ascending (`^`), click again to sort descending (`v`).
   - **Drag column dividers** (`|`) to resize individual columns, or **double-click** a divider to reset column widths.
   - **Click the `Fit` button** on the right end of any table header to auto-expand all columns to fit their longest visible data values.
3. **Collapsible Process Panel & Status Bar (Bottom):**
   - Displays live progress bars, streaming log output, record counts, active database file, and `Collapse` / `Expand` / `Clear` controls.

---

## Detailed Module Guide & Basic Settings

### 1. Parse Tab — Multi-Vendor CM/PM Parser & Exporter
1. **Input Files:** Click **+ Add Files...** or **+ Add Folder...** to queue raw exports (`.xml`, `.csv`, `.txt`, `.gz`, `.zip`, `.xlsx`, `.xls`, `.log`). RANO automatically detects the vendor parser next to each filename.
2. **Output Folder:** Click **Browse...** to choose where exported files will be written (defaults to the application directory).
3. **Parser Settings:**
   - **Parser:** Keep **Auto-detect (recommended)** or force a specific parser from the dropdown.
   - **Save parsed records to database:** Keep checked to stream records into the local SQLite WAL database (`%TEMP%\rano_scratch\rano.db`) for large-file deduplication and **Baseline** auditing.
   - *Note:* If input files mix multiple vendors (e.g. Ericsson CM + Huawei CM), RANO prompts you to select which vendor group to parse in the current run.
4. **Run & MO Filter:** Click **Parse**. Once complete, the right panel displays the **MO types** grid with record counts, search box, pagination, and `All` / `None` checkboxes.
5. **Export:** Click **Export CSV**, **Export TXT**, or **Export XLSX** to export the selected MO types.

### 2. Baseline Tab — Parameter Audit & Remediation
1. **Baseline Profiles (Left Sidebar):**
   - Toggle standard rule profiles: **4G LTE**, **5G NR**, **4G ENDC**, **3G WCDMA** (loaded from the `bl/` folder).
   - Click **+ File** to load custom `.txt` / `.tsv` / `.csv` rule files, or click **Manage Rules** to inspect, search, add, edit, enable/disable, or export baseline rules interactively.
2. **Audit Data Source:**
   - By default, audits the data currently loaded in the **Parse** SQLite database.
   - Under **Direct Import (Already Parsed)**, you can directly load a **Folder**, **Files**, **Excel (.xlsx)**, or **SQLite DB (`.db`)** without re-running the Parse tab.
3. **Run & Remediate:**
   - Click **Check Baseline**.
   - Review the 4 summary cards (**Total Checked**, **Compliant**, **Discrepancies**, **Affected Nodes**), filter by `Discrepancies only`, `Profile`, `Node`, `MO`, or keyword.
   - Click **Copy** next to any parameter to copy its ready-to-run Ericsson ENM `cmedit set` remediation command, or export results via **Export CSV**, **Export Excel**, or **Export cmedit**.

### 3. ECLI Tab — Ericsson ENM Terminal & Command Builder
1. **ENM Profiles (Left Sidebar):**
   - Click **+** to add an ENM server profile (`Name`, `ENM URL / Host`, `Username`, `Password`).
   - Double-click a profile (or right-click -> **Connect**) to open an interactive ENM session tab. Multiple concurrent ENM sessions are supported.
2. **Split View & Multi-Target Execution:**
   - Click **Split View** when 2+ sessions are connected to view two ENM terminals side by side and send commands to **Primary**, **Split**, or **Both** sessions simultaneously.
3. **Command Builder & Quick Commands:**
   - Click **Builder** to generate structured `cmedit get`, `cmedit set`, `alarm`, `pm`, or `mobatch` commands, or click any saved template under **Quick Commands** (`ecli_commands.json`).
   - Customize terminal appearance using the **Theme** selector (`Catppuccin`, `Midnight`, `OneDark`, `Solarized`, `Paper`) and **Keyword Highlight** toggle.

### 4. SFTP Tab — Dual-Pane Local `<->` ENM File Commander
1. **Session Selection:** Choose an ENM profile from the **Session** dropdown and click **Connect** (uses the Global Proxy setting automatically).
2. **Dual-Pane Navigation:**
   - **Left Pane (Local Computer):** Quick jump to drives (`C:\`), `Output/`, `Browse...`, `Explorer`, or `+ Folder`.
   - **Right Pane (Remote Server - ENM SFTP):** Quick jump to `Home (~)`, `PM`, `Export`, `/tmp`, `+ Folder`, or `+ File`.
3. **Transfer:** Select files/folders on either side and click **Upload ->** or **<- Download** (or drag and drop files from Windows Explorer directly onto the Remote Server pane).

### 5. Trace Tab — 5G EN-DC, 3GPP Protocol Flow, DCG & XML Translations
Organized into 4 module groups in the left navigation sidebar:
1. **5G EN-DC & CU-CP:**
   - **Overview & Event KPIs:** Select a folder/files of 5G CellTrace / UE Trace logs and click **Scan & Parse 5G** (or **Demo 5G Trace**). Displays 8 KPI cards, Top Failing Cells, Cause Breakdown, Release Distribution, and a 16-column per-cell KPI table.
   - **CR Script Generator:** Enter Node ID, Scanner ID, PLMN, and UE Trace IDs to generate ENM `cmedit` CR activation/deactivation scripts.
   - **ENM SFTP Collector:** Search and download CellTrace/UETRace files directly from ENM over SFTP.
2. **3GPP Protocol & Flow:**
   - Select `.bin`, `.pcap`, `.pcapng`, `.cap`, `.log`, `.txt`, or `.gz` trace files and click **Parse File** (or **Demo Data**).
   - **LTNG Flow (3GPP ASN.1):** Filter packets (`e.g. type == S1AP and msg contains InitialUEMessage`), click **Follow UE**, and click any packet to inspect its **ASN.1 Decode Tree** and **16-Byte Hex Dump** side by side.
   - **Simple Summary & PM Counters:** High-level message timeline and aggregated `EVENT_PARAM_*` / `pm*` counters.
   - **UE Performance:** Dual-panel vector charts (**DL/UL Throughput & HARQ BLER** + **RF Link Adaptation PRB/MCS/SINR**) with a 13-column table.
   - **Timing Advance (TA):** **3-Sector Polar Azimuth Radar**, **UE Propagation Distance Trajectory**, and **Range Ring Histogram** (`1 TA = 78.125 m`).
   - **VoLTE Drops & Baseband / SAF Filter:** QCI-1 abnormal release root-cause table and baseband subsystem trace filter.
   - **Hex PDU Decoder:** Paste raw hex bytes (`00 01 00 1A...`), pick a protocol (`S1AP`, `X2AP`, `NGAP`, `XNAP`, `F1AP`, `E1AP`, `RRC`, `CPP`, `HiCap`), and click **Decode Hex PDU** (or **Load Sample S1AP**).
3. **DCG Diagnostics:**
   - Load an Ericsson support archive (`*_dcgm.zip`, `.tar.gz`, or extracted folder) and click **Inspect DCG** to examine **Node Summary & Health** (4 status cards + properties table), **Node Logs** (filtered by severity), **MO Browser**, **PMD Crashes**, and **Archive Files & Grep**.
4. **XML Translations:**
   - Point to a folder of Ericsson event XML definitions and click **Load XML Translations** to enrich PM Events, Baseband Traces, and Interface Signals.

### 6. Auto Tab — Scheduled Multi-ENM Dump Pipelines
1. Click **+ New Job** to create an automated dump pipeline (`auto_jobs.json`).
2. Select target **ENM Profiles**, enter the `cmedit` command(s) to execute, choose the merged **Export Format** (`CSV`, `TXT`, `XLSX`, or `SQLite .db`), **Output Folder**, and **Schedule Interval** (or run manually via **Run now**).
3. Monitor live execution across all ENM sessions in the interactive S-shaped pipeline graph.

### 7. KPI Tab — 4G/5G PM Statsfile Analyzer & PRACH Distribution
1. **Data Source Mode:**
   - **Local Files / Folder:** Select local `statsfile.xml` / `.xml.gz` files or folders and click **Calculate KPIs**.
   - **Remote ENM Fetch:** Select an ENM SFTP profile, enter target **Node Name(s)** and **ROP Time Window**, and click **Fetch & Calculate**.
2. **Aggregation & Views:**
   - Switch between **Cell Level** and **Node Level**, and filter by Technology (`All`, `4G LTE`, `5G NR`, `3G`), Date/ROP, Node, or Cell in the left filter sidebar.
   - Switch between **Table View**, **Trend Charts**, and **PRACH Distribution** (analyzes `pmPropagationDelay` / `pmPrachPropagationDelay` counters to compute cell overshooting distance percentiles).
   - Click **Columns** to toggle visible KPI columns, **Formulas** to customize KPI expressions, or **Export Excel** to save a formatted `.xlsx` report.

### 8. About Tab — License Activation & Auto-Update
1. **License Activation:**
   - Copy your **Machine ID** (`About -> License -> Copy`) and send it to the administrator, or click **Start Trial** to request a one-time 7-day trial.
   - Paste your key (`RANO-XXXX-...`) and click **Activate**.
2. **OTA Auto-Update:**
   - Click **Check for updates** under `About -> Update` to download, SHA-256 verify, replace, and restart into the latest release automatically.

---

## CLI Usage (Headless Mode)

Run `rano.exe` without arguments to launch the GUI, or pass arguments for headless batch processing:

```text
rano [options] <input files or folders...>

Options:
  --out <dir>      Export directory (default: first input file's folder)
  --format <fmt>   Export format: csv | txt | xlsx  (default: csv)
  --parser <name>  Force a parser (auto-detect by default)
  --no-db          Keep results in memory instead of the SQLite database
  --vendor <name>  With mixed inputs, parse this vendor group (e.g. huawei,
                   'ericsson cm', '3gpp pm'). Default: vendor of the first file
  --machine-id     Print this machine's ID (for licensing) and exit
  --activate <key> Activate with a license key, then exit
  --help, -h       Show this help
```

### CLI Examples

```powershell
rano cmdata.xml
rano logs\ --format xlsx --out out\
rano dump_dir\ --vendor "ericsson cm" --format csv --out export\
rano --machine-id
rano --activate RANO-ABCD-EFGH-IJKL
```

---

## Configuration & Data Files

| File / Path | Purpose |
| :--- | :--- |
| `rano_license.json` | Encrypted per-machine license & trial state (next to `rano.exe`) |
| `ecli_profiles.json` | Saved ENM server connection profiles (shared by ECLI, SFTP, Auto, KPI, Trace) |
| `ecli_commands.json` | Saved ECLI quick commands and templates |
| `ecli_ui.json` | UI preferences, terminal theme, and **Global Proxy** configuration |
| `auto_jobs.json` | Scheduled & on-demand Auto pipeline job definitions |
| `kpi_formulas.json` | User-customized KPI formulas and thresholds |
| `bl\*.txt` / `*.csv` | Standard & custom Baseline parameter audit rule files (`4G`, `5G`, `ENDC`, `3G`) |
| `%TEMP%\rano_scratch\rano.db` | Temporary SQLite WAL database used during streaming Parse & Baseline audits |
| `logs\rano_<date>.log` | Daily rotating application diagnostic logs |

---

## Build from Source

Requirements: Rust stable toolchain (`edition = 2024`).

```powershell
cargo build --release
cargo test --release
```

Output binary: `target\release\rano.exe` (standalone single-file executable with LTO and stripped symbols).
