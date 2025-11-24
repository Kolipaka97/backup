#  Bash Backup Utility

A flexible and configurable Bash script for backing up files and directories with support for compression, restoration, exclusion patterns, and backup rotation. Ideal for developers and sysadmins who want a simple CLI-based backup solution.

Project Overview

This project provides a **command-line backup solution** that:
- Creates timestamped backups of files or directories.
- Supports compression into `.tar.gz` archives.
- Allows restoring backups to a target directory.
- Lists available backups for quick reference.
- Skips unnecessary files using exclusion patterns.
- Maintains logs and checksum files for integrity verification.
- Automatically rotates old backups to save space.

---
Requirements

- **Operating System:** Linux/Unix (tested on Bash shell)
- **Dependencies:**  
  - `bash` (default shell)  
  - `tar` (for compression)  
  - `cp` (for file copying)  
  - `sha256sum` (for checksum generation)  
  - `tee`, `ls`, `find`, `awk` (standard utilities)

---

How to Use
Make it executable
chmod +x backup.sh

Run a full backup
./backup.sh

 Default Folder Structure
project-folder/
---

 backup.sh          # The main script
 backup.log         # Detailed log of all backup runs
 backup.txt         # Output log for current session
 backup.conf        # Optional configuration file
 backups/           # Backup storage folder

---


## Features

-  Backup files or folders with optional compression (`.tar.gz`)
-  Restore backups to any target directory
-  Backup files modified in the last X days
-  Automatically rotate old backups (keep only latest N)
-  Generate SHA256 checksums for integrity
-  Configurable via `backup.conf`
-  Dry-run mode for safe preview
-  List all available backups

---

Available Options
Option	Description
--compress	     Compress the backup into a .tar.gz archive
--dry-run	     Preview what files will be backed up (no changes made)
--recent Xd	     Backup only files modified in the last X days (e.g., 
--list	         List all available backups
--restore <file> Restore a backup file
--to <path>	     Destination folder for restore
--help	         Show usage information


## Usage

```bash
./backup.sh [options] [file1 file2 ...]

Configerationa:

Create a backup.conf file in the script directory to override defaults

BACKUP_DESTINATION="/custom/backup/path"
EXCLUDE_PATTERNS=".git node_modules .cache temp"
DAILY_KEEP=7
WEEKLY_KEEP=4
MONTHLY_KEEP=3

 Backup Behavior
- If no files are specified, defaults to backing up all files in ./data/
- Excludes patterns defined in EXCLUDE_PATTERNS
- Stores backups in ./backups/ by default
- Keeps only the latest 5 backups (configurable via KEEP_COUNT)
- Logs are saved to backup.log and backup.txt

----

How It Works

- Setup Paths
- Creates directories for backups, logs, and configuration.
- Uses Asia/Kolkata timezone for timestamps.
- Configuration
- Loads settings from backup.conf if available.

- Modes of Operation
- Normal Backup Mode: Copies files into a timestamped folder.
- List Mode: Displays available backups.
- Restore Mode: Restores files from a backup archive or folder.
- Dry-Run Mode: Shows what would be backed up without copying.
- Recent Mode: Backs up files modified in the last X days

- Compression
- If --compress is enabled, backups are archived into .tar.gz.

- Logging
- All actions are logged in backup.log.
- Output is also saved in backup.txt.

Cleanup Policy
Automatically deletes older backups beyond the configured KEEP_COUNT. Customize this in backup.conf.


Output Example:
The data will stored in the backup.log file

[Mon, Nov 24, 2025 10:49:48 AM]  Starting backup...
[Mon, Nov 24, 2025 10:49:48 AM]  No files specified — defaulting to all files in data/
[Mon, Nov 24, 2025 10:49:48 AM]  Backed up: data/Predicto_Project_Report_Simple (1).pdf
[Mon, Nov 24, 2025 10:49:48 AM]  Checksum file created: 2025-11-24_10-49-48.sha256
[Mon, Nov 24, 2025 10:49:48 AM]  Removed old backup: 2025-11-09_20-38-27.sha256
[Mon, Nov 24, 2025 10:49:48 AM]  Removed old backup: 2025-11-09_20-34-04.sha256
[Mon, Nov 24, 2025 10:49:49 AM]  Backup process completed successfully.
[Mon, Nov 24, 2025 10:49:49 AM]  Backup saved at: backups/2025-11-24_10-49-48
[Mon, Nov 24, 2025 10:49:49 AM]  Log file: backup.log

