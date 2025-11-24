#  Bash Backup Utility

A flexible and configurable Bash script for backing up files and directories with support for compression, restoration, exclusion patterns, and backup rotation. Ideal for developers and sysadmins who want a simple CLI-based backup solution.

---
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

 backup.sh     
 # The main script
 backup.log    
 # Detailed log of all backup runs
 backup.txt    
 # Output log for current session
 backup.conf    
 # Optional configuration file
 backups/      
 # Backup storage folder

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


Backup
A compact README for the backup utility included in this repository. It documents purpose, requirements, configuration, usage, scheduling, restore, logging, and contribution notes.

Overview
This project provides a simple backup utility to copy or archive specified files and directories to a target storage location (local path, network share, or cloud mount). Features include incremental/archive modes, retention policy, logging, and optional encryption.

Features
Full and incremental backup modes
Configurable source(s) and destination
Retention and rotation rules
Optional compression and encryption
Logging and exit codes for automation
Restore commands and verification
Requirements
Operating system: Windows, macOS, or Linux
Shell or runtime: Bash / PowerShell / Python (depending on provided script)
Disk space on destination >= expected backup size
(Optional) gzip, tar, zip, openssl/openssl-compatible tool for compression/encryption
Installation
Clone the repository:
git clone <repo-url>
cd <repo-dir>
Ensure the runtime and optional tools are installed (e.g., Python 3, tar, gzip).
Make scripts executable if needed:
chmod +x ./backup.sh
Configuration
Edit the configuration file or environment variables before running:

config.yaml or .env (example):
sources:
   - /path/to/dir
   - /path/to/file
destination: /path/to/backup/location
mode: incremental      # full | incremental
retain: 7              # number of backups to keep
compress: true
encrypt: false
encryption_key: ""
Alternatively use CLI flags:
./backup.sh --source /data --dest /backups --mode incremental --retain 14
Usage
Run a backup manually:

Shell script:
./backup.sh
Python:
python backup.py --config config.yaml
PowerShell:
.\backup.ps1 -Config .\config.json
Exit codes:

0: success
1: configuration error
2: runtime error (I/O)
3: verification failed
Logs are written to logs/backup-YYYYMMDD.log by default.

Scheduling
Linux/macOS (cron):
# daily at 02:00
0 2 * * * /path/to/repo/backup.sh >> /path/to/repo/logs/backup.log 2>&1
Windows (Task Scheduler): create a task that runs the script or PowerShell command on the desired schedule.
Restore
To restore a backup:

Identify backup archive or folder in the destination (timestamped).
For compressed archives:
tar -xzvf backup-YYYYMMDD.tar.gz -C /restore/path
For encrypted archives, decrypt first then extract:
openssl enc -d -aes-256-cbc -in backup.enc -out backup.tar.gz -pass pass:YOUR_KEY
tar -xzvf backup.tar.gz -C /restore/path
Verify permissions and ownership after restore if required.

Testing
Run the script on a small test dataset and verify contents in the destination.
Use dry-run or verification flags if provided:
./backup.sh --dry-run
----
Troubleshooting
Check logs in logs/ for detailed errors.
Ensure destination has sufficient space and correct permissions.
Confirm network mounts are available before running scheduled tasks.
----
Limitations
Rotation Currently supports daily cleanup; weekly/monthly cleanup to be added.
Designed for Linux/Unix environments; Windows users must use WSL or Git Bash
Limited error handling; unexpected system issues (e.g., permission errors, disk full) may cause incomplete backups
 Restore works for `.tar.gz` archives or plain directories, but does not handle complex restore scenarios (e.g., partial restore, permissions).
----

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

