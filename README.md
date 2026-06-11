# Windows Repair Toolkit

Windows Repair Toolkit is a simple two-phase batch script designed to automate several built-in Windows maintenance and repair tasks. The goal is to help users quickly perform common troubleshooting procedures that can resolve system corruption, disk errors, networking issues, and software-related problems without manually entering commands.

## Features

### Phase 1 – Disk Check & Repair

* Verifies Administrator privileges
* Schedules a full `CHKDSK /R` scan on the system drive
* Automatically restarts the computer
* Repairs file system errors and scans for bad sectors during boot

### Phase 2 – System Repair & Maintenance

* Runs System File Checker (`SFC /SCANNOW`)
* Repairs the Windows component store using DISM
* Resets the Windows Winsock networking catalog
* Updates installed applications through Winget

## Purpose

This toolkit combines several trusted Windows repair utilities into an easy-to-use workflow. It is intended to help resolve issues such as:

* System file corruption
* File system errors
* Bad sectors on storage drives
* Windows image corruption
* Network stack problems
* Outdated software installations

## Requirements

* Windows 10 or Windows 11
* Administrator privileges
* Internet connection recommended for DISM and Winget operations

## Usage

1. Run **Phase 1** as Administrator.
2. Allow the computer to restart and complete the CHKDSK scan.
3. Once Windows has booted normally, run **Phase 2** as Administrator.
4. Restart the computer after Phase 2 completes for best results.

## Disclaimer

This tool uses built-in Windows commands and does not make permanent system modifications beyond the repairs performed by those utilities. While it may help resolve issues that contribute to poor system performance, it is primarily intended as a repair and maintenance tool rather than a direct PC optimization utility.
