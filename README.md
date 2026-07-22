# 📂 Smart File Organizer

A lightweight Python script that automatically organizes messy folders (like `Downloads` or `Desktop`) by sorting files into category folders based on their extensions — with an optional live-watch mode that organizes new files the moment they appear.

## ✨ Features

- **Automatic categorization** — sorts files into `Belgeler` (Documents), `Resimler` (Images), `Arsivler` (Archives), `Muzik` (Music), `Videolar` (Videos), `Programlar` (Programs), and `Kod` (Code) based on extension
- **Custom rules** — bring your own category/extension mapping via a JSON file
- **Safe renaming** — automatically appends `(1)`, `(2)`, etc. to avoid overwriting files with the same name
- **Dry-run mode** — preview exactly what would happen before anything is moved
- **Live watch mode** — uses [watchdog](https://pypi.org/project/watchdog/) to monitor a folder and organize new files instantly as they're added
- **Logging** — every action is recorded to `organizer.log` inside the target folder

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- (Optional, for `--watch` mode) the `watchdog` package

### Installation

```bash
git clone https://github.com/eliftrk99/organizer.git
cd organizer
pip install watchdog   # only needed if you want live-watch mode
```

## 🛠️ Usage

**One-time cleanup of a folder:**

```bash
python organizer.py --path "/Users/you/Downloads"
```

**Preview without moving anything:**

```bash
python organizer.py --path "/Users/you/Downloads" --dry-run
```

**Continuously watch a folder and organize new files as they arrive:**

```bash
python organizer.py --path "/Users/you/Downloads" --watch
```

**Use your own category rules:**

```bash
python organizer.py --path "/Users/you/Downloads" --rules my_rules.json
```

### CLI Options

| Flag              | Description                                                        |
|-------------------|----------------------------------------------------------------------|
| `--path`          | **(required)** Path to the folder to organize                     |
| `--rules`         | Path to a JSON file with custom category → extension mappings      |
| `--other-folder`  | Folder name for unmatched extensions (default: `Diger`)            |
| `--dry-run`       | Show what would happen without actually moving files                |
| `--watch`         | Keep running and organize new files as soon as they appear (requires `watchdog`) |

## 📋 Default Categories

| Category      | Extensions                                                        |
|---------------|---------------------------------------------------------------------|
| Belgeler      | `.pdf` `.docx` `.doc` `.txt` `.xlsx` `.xls` `.pptx` `.ppt` `.csv` `.odt` `.md` |
| Resimler      | `.jpg` `.jpeg` `.png` `.gif` `.svg` `.webp` `.bmp` `.tiff` `.heic`  |
| Arsivler      | `.zip` `.rar` `.7z` `.tar` `.gz` `.iso`                              |
| Muzik         | `.mp3` `.wav` `.flac` `.aac` `.m4a`                                  |
| Videolar      | `.mp4` `.mov` `.avi` `.mkv` `.webm`                                  |
| Programlar    | `.exe` `.msi` `.dmg` `.pkg` `.deb` `.apk`                            |
| Kod           | `.py` `.js` `.html` `.css` `.json` `.java` `.cpp` `.c` `.ipynb` `.sh` |

Anything that doesn't match goes into the `Diger` (Other) folder by default.

### Custom rules example

Create a JSON file to override the defaults:

```json
{
  "Documents": [".pdf", ".docx", ".txt"],
  "Images": [".jpg", ".png", ".gif"],
  "Archives": [".zip", ".rar"]
}
```

Then run:

```bash
python organizer.py --path "/Users/you/Downloads" --rules my_rules.json
```

## 🗂️ How It Works

```
[Target Folder]
      │
      ▼ (read extension, e.g. .pdf)
[Rules Lookup] → matches .pdf → "Belgeler" folder
      │
      ▼
[shutil.move()] moves the file, creating the folder if needed
```

In `--watch` mode, the script first organizes any existing files, then keeps running in the background and organizes each new file as soon as it's fully written to disk.

## 📝 Logging

Every run creates/appends to `organizer.log` inside the target folder, recording each file that was moved (or would be moved, in dry-run mode) along with a timestamp.

## ⚠️ Notes

- `--watch` mode requires the `watchdog` package. If it isn't installed, the script will print an install hint and exit.
- Files are matched by extension only (case-insensitive); the script does not inspect file contents.
- Only the top-level of the target folder is scanned — subfolders are left untouched.

## 📄 License

MIT — feel free to use, modify, and share.
