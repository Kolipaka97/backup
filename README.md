#  Bash Backup Utility

A flexible and configurable Bash script for backing up files and directories with support for compression, restoration, exclusion patterns, and backup rotation. Ideal for developers and sysadmins who want a simple CLI-based backup solution.

How to Use
Make it executable
chmod +x backup.sh

Run a full backup
./backup.sh

 Default Folder Structure
project-folder/

 backup.sh          # The main script
 backup.log         # Detailed log of all backup runs
 backup.txt         # Output log for current session
 backup.conf        # Optional configuration file
 backups/           # Backup storage folder

---

bash-backup/
│
├── backup.sh        # Main backup script
├── backup.conf      # Optional config file
├── backups/         # Backup storage directory
├── backup.log       # Log file
├── backup.txt       # Output capture
└── README.md        # Documentation


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

 Restore Example

./backup.sh --restore backups/backup_2025-11-09_22-00-00.tar.gz --to /restore/path

List Backups:
./backup.sh --list

Dry Run Example
./backup.sh --dry-run file1.txt file2.txt

 Checksum Verification
Each backup includes a .sha256 file for integrity checks:
sha256sum -c backup_YYYY-MM-DD_HH-MM-SS.tar.gz.sha256

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

