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
