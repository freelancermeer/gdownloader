# gdownloader

A resilient, production-ready bulk Google Drive folder downloader and Quality Control (QC) verification tool built with Python.

---

## Features

- **Automated Anti-Blocking:** Injects modern desktop User-Agents to prevent Google's 403 Forbidden / bot detection on direct downloads.
- **Smart Cookie Validation:** Automatically checks if `cookies.txt` has valid active tokens or causes a redirect to Google ServiceLogin, gracefully falling back to public mode if cookies are stale.
- **Smart Skip & Resume:** Detects completed folders in milliseconds to prevent re-downloading gigabytes of existing data.
- **Built-in Quality Control (QC):** Validates downloaded content, verifying videos (`.mp4`), thumbnails, and text metadata.
- **Zero Extra Files:** Single worker script design configurable via command-line flags and parameters.
- **Cross-Platform UTF-8 Support:** Reconfigures Windows terminal output to eliminate encoding crashes.

---

## Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/muhammadmeeroffice-blip/gdownloader.git
   cd gdownloader
   ```

2. **Install required dependencies:**
   ```bash
   pip install gdown requests beautifulsoup4
   ```

3. **Prepare your links:**
   Add Google Drive folder URLs to `links.txt` (one per line):
   ```text
   https://drive.google.com/drive/folders/1PsWVz_rZXYS6jKez2S5PDf1NEefwn0nS
   https://drive.google.com/drive/folders/1c8OxxA_Yk8MATzzcnjkkBum-0d3KBPFg
   ```

---

## Usage

### 1. Download Folders (Default)
Run the script to download all folders defined in `links.txt`:
```bash
python gdownloader.py
```
*(Folders that are already downloaded will automatically be skipped in seconds).*

### 2. Run Quality Control (QC) Report
Verify all downloaded folders without downloading:
```bash
python gdownloader.py --qc
```

---

## Command-Line Flags & Parameters

| Flag | Argument | Default | Description |
|---|---|---|---|
| `--qc` | None | `False` | Run Quality Control verification report on downloaded folders. |
| `--qc-count` | `INT` | `All` | Limit the QC report to check only the first `N` links. |
| `--limit` | `INT` | `None` | Process only up to `N` folders from `links.txt`. |
| `--start` | `INT` | `1` | Start processing from folder index `N` (1-based index). |
| `--no-skip` | None | `False` | Disable smart skip; re-checks and re-downloads existing folders. |
| `--wait` | `INT` | `5` | Wait time in seconds between consecutive folder downloads. |
| `--retries` | `INT` | `5` | Maximum retry attempts per folder on failure. |
| `--force-cookies` | None | `False` | Force using `cookies.txt` even if validation detects a login redirect. |
| `--quiet` | None | `False` | Suppress download progress bar output. |

---

## Examples

- **Process folders 16 through 35:**
  ```bash
  python gdownloader.py --start 16 --limit 20
  ```

- **Run QC on the first 10 folders:**
  ```bash
  python gdownloader.py --qc --qc-count 10
  ```

- **Custom wait time and retries:**
  ```bash
  python gdownloader.py --wait 10 --retries 8
  ```

---

## Project Structure

```text
gdownloader/
├── Download Data/       # Destination folder for downloaded content
│   └── .gitkeep        # Keeps folder tracked in Git
├── .gitignore          # Ignores sensitive cookies, logs, and heavy files
├── gdownloader.py      # Unified downloader & QC worker script
├── links.txt           # Input list of Google Drive folder URLs
└── README.md           # Documentation and usage guide
```
