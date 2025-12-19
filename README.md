# PharmApp Backup Tool (GUI) — v1.3

A lightweight, profile-based backup utility for Windows (and buildable for macOS) designed for **incremental backups** with clear **audit artifacts** (log + JSON summary + CSV history) per run.

This project focuses on practical, reliable backups for large, messy knowledge bases (QA/QC/R&D files, PDFs, Office docs, media, design/statistical project files, etc.) while staying simple to operate for non-technical users.

---

## Download (Windows EXE)

The Windows build is available here:

```text
https://github.com/nghiencuuthuoc/PharmAppBackup/blob/main/PharmAppBackup_v1.3.exe
```

Notes:
- If GitHub shows a file preview page, use **Download** / **Download raw file**.
- Windows may show SmartScreen/Defender warnings for new EXEs. Verify you downloaded from the official repository before running.

---

## Screenshot

Add your screenshot to the repo (example path):

```text
docs/screenshot.png
```

Then reference it here:

```md
![PharmApp Backup Tool GUI](docs/screenshot.png)
```

---

## Key Features

- **Profiles**: switch between backup configurations quickly.
- **Multiple Jobs per Profile**: each job is a `Source -> Target` route.
- **Job Override**: per-job **groups / excludes / verify** can override profile defaults.
- **Auto Backup**: run all enabled jobs every *N* minutes (sequential execution).
- **Manual Backup**:
  - Run selected job
  - Run all enabled jobs in the profile
- **Incremental Copy**:
  - Copy **new** files
  - Copy **updated** files (mtime newer than target)
  - Skip up-to-date files
- **Safety**:
  - Blocks invalid configurations (Target == Source or Target inside Source).
- **Robust Copy**:
  - Atomic replace using `*.part` + `os.replace`
  - Retry on transient errors/locks
- **Audit Artifacts (per job run, saved inside Target)**:
  - `backup_YYYYMMDD_HHMMSS.log`
  - `backup_summary_YYYYMMDD_HHMMSS.json`
  - `backup_history.csv` (append-only)

---

## Concepts

### Profile
A container for:
- Default settings (interval, groups, exclude patterns, verify, etc.)
- A list of Jobs

### Job
One backup route:
- `Source` folder
- `Target` folder
- `Enabled` flag
- Optional `Note`

### Override
When enabled on a Job:
- The job uses its own groups / excludes / verify
- Otherwise it inherits profile defaults

---

## Usage Guide (Quick)

1. **Select or create a Profile**
   - Use `New`, `Save`, `Delete`, `Export`, `Import`
2. **Add Jobs**
   - Set `Source` and `Target`
   - Add a `Note` for traceability
   - Click `Add Job`
3. **Enable/Disable Jobs**
   - Select a row → `Toggle Enabled`
4. **Configure Profile Defaults**
   - Interval (minutes)
   - Exclude dirs (comma-separated)
   - Exclude patterns (comma-separated globs)
   - File groups (All groups or selected groups)
5. **Optional: Set Job Override**
   - Select job → `Edit Override…`
   - Enable override and adjust job-specific settings
6. Run:
   - `Run Selected Job` for testing
   - `Run All Enabled Jobs` for a full manual run
   - `Start Auto Backup` for periodic runs

---

## Output Files (in Target folder)

Each job run produces:

- **Session Log**
  - `backup_YYYYMMDD_HHMMSS.log`
  - Detailed file-level actions and errors
- **JSON Summary**
  - `backup_summary_YYYYMMDD_HHMMSS.json`
  - Run metadata and totals (copied/skipped/errors)
- **CSV History**
  - `backup_history.csv`
  - Append-only timeline of runs (good for Excel / reporting)

---

## File Groups (Highlights)

The tool supports common groups (documents, archives, audio, video, pictures, code, others) plus:

- **Graphics**: Corel / Adobe / Affinity / design formats (e.g., `.psd`, `.ai`, `.cdr`, `.indd`, …)
- **Stats**: Minitab / SAS / JMP formats (e.g., `.mtw`, `.mpj`, `.sas`, `.jmp`, …)

---

## Build From Source

### Prerequisites
- Python 3.10+ (recommended)
- PyInstaller

```bash
pip install pyinstaller
```

### Windows: One-file EXE (recommended)

```bash
pyinstaller --noconfirm --clean --onefile --windowed --name PharmAppBackup --icon nct_logo.ico --add-data "nct_logo.png;." --add-data "nct_logo.ico;." pharmapp_backup_gui_v1_3_profiles_job_override_theme_footer_icons.py
```

**Windows note:** `--add-data` uses `;` between `SRC;DEST`.

### macOS: Build .app (windowed)

```bash
pyinstaller --noconfirm --clean --windowed --name PharmAppBackup --add-data "nct_logo.png:." pharmapp_backup_gui_v1_3_profiles_job_override_theme_footer_icons.py
```

**macOS/Linux note:** `--add-data` uses `:` between `SRC:DEST`.

macOS app icon:
- Preferred: `--icon nct_logo.icns`
- If you only have `.png`, convert to `.icns` for best dock/icon quality.

---

## Assets

Expected icons:

```text
./nct_logo.ico   # Windows EXE icon (PyInstaller --icon)
./nct_logo.png   # Window icon (Tk iconphoto)
```

---

## Data & Privacy

- The app runs locally.
- Logs and summaries are saved in the selected Target folder for auditability.
- No telemetry is sent.

---

## Roadmap (Suggested)

- Job-level “merge override” (override adds/extends, not replaces)
- Dry-run / Scan-only mode (preview size and file count)
- Optional concurrency controls (2–3 parallel workers with I/O throttling)
- Retention policy for logs/summaries (keep last N days)

---

## Donate / Support

- Donate NCT: `https://www.nghiencuuthuoc.com/p/donate.html`
- Donate PharmApp: `https://www.pharmapp.vn/Donate`

(Links are also available in the app footer.)

---

## License

Add your preferred license here (e.g., MIT). If you have not chosen yet, create `LICENSE` later and update this section.
