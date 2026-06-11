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

@echo off
title Schedule CHKDSK and Restart
color 0A

:: Check for Administrator privileges
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo.
    echo ERROR: Please run as Administrator.
    echo.
    pause
    exit /b
)

echo ==========================================
echo Scheduling CHKDSK /R on drive C:
echo ==========================================
echo.

echo Y|chkdsk C: /r

echo.
echo CHKDSK has been scheduled.
echo Restarting now...
echo.

shutdown /r /f /t 0



@echo off
title Repair Phase 2 - Windows Repair
color 0A

:: Check for Administrator privileges
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo.
    echo ERROR: Please run as Administrator.
    echo.
    pause
    exit /b
)

echo ==========================================
echo PHASE 2 - WINDOWS REPAIR
echo ==========================================
echo.

echo.
echo [1/4] Running System File Checker...
echo.
sfc /scannow

echo.
echo [2/4] Running DISM RestoreHealth...
echo.
DISM /Online /Cleanup-Image /RestoreHealth

echo.
echo [3/4] Resetting Winsock...
echo.
netsh winsock reset

echo.
echo [4/4] Updating installed software...
echo.
winget upgrade --all

echo.
echo ==========================================
echo REPAIRS COMPLETE
echo ==========================================
echo.
echo It is recommended to restart the PC now.
echo.

pause
