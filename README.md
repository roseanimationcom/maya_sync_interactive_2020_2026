@echo off
REM Maya interactive shelf sync helper (2020-2026) - corrected numbering
REM Usage: Run as administrator

:: -------- CONFIG --------
set "SYNC_SHELVES=1"   REM 1 to sync shelves
set "SYNC_ICONS=1"     REM 1 to sync icons
:: -------------------------

setlocal enabledelayedexpansion

:: Define available Maya versions
set "MAYA_VERSIONS=2020 2021 2022 2023 2024 2025 2026"
set IDX=1
echo Available Maya versions:
for %%V in (%MAYA_VERSIONS%) do (
    echo !IDX! - %%V
    set /A IDX+=1
)

:: Ask user which version is installed (source)
set /P CHOICE=Select your installed Maya version [1-7]:
set IDX=1
for %%V in (%MAYA_VERSIONS%) do (
    if "!IDX!"=="%CHOICE%" set "SRC_VER=%%V"
    set /A IDX+=1
)
echo Selected source version: !SRC_VER!

:: Build target versions list (all except source)
set "DST_VERSIONS="
for %%V in (%MAYA_VERSIONS%) do (
    if NOT "%%V"=="!SRC_VER!" (
        set "DST_VERSIONS=!DST_VERSIONS!%%V,"
    )
)
:: Remove trailing comma
set "DST_VERSIONS=!DST_VERSIONS:~0,-1!"

echo Source version: !SRC_VER!
echo Target versions: !DST_VERSIONS!

:: Define source paths
set "SRC_SHELVES=%USERPROFILE%\Documents\maya\!SRC_VER!\prefs\shelves"
set "SRC_ICONS=%USERPROFILE%\Documents\maya\!SRC_VER!\prefs\icons"

:: Elevate to admin if needed
net session >nul 2>&1
if %errorlevel% neq 0 (
    echo Requesting administrator privileges...
    powershell -NoProfile -Command "Start-Process -FilePath '%COMSPEC%' -ArgumentList '/c \"%~f0\"' -Verb RunAs"
    exit /b
)

:: Timestamp for backup
for /f "usebackq" %%T in (`powershell -NoProfile -Command "Get-Date -Format yyyyMMdd_HHmmss"`) do set "TIMESTAMP=%%T"

:: Process each target version
for %%D in (!DST_VERSIONS!) do (
    set "DST_VER=%%D"
    set "DST_PREFS=%USERPROFILE%\Documents\maya\!DST_VER!\prefs"
    set "DST_SHELVES=%USERPROFILE%\Documents\maya\!DST_VER!\prefs\shelves"
    set "DST_ICONS=%USERPROFILE%\Documents\maya\!DST_VER!\prefs\icons"

    echo.
    echo ===============================
    echo Processing target Maya version: !DST_VER!

    if "%SYNC_SHELVES%"=="1" (
        call :backup_and_link "!SRC_SHELVES!" "!DST_SHELVES!" "!DST_PREFS!"
    )
    if "%SYNC_ICONS%"=="1" (
        call :backup_and_link "!SRC_ICONS!" "!DST_ICONS!" "!DST_PREFS!"
    )
)

echo.
echo All done. Check messages above for details.
pause
exit /b 0

:backup_and_link
set "SRC=%~1"
set "DST=%~2"
set "DST_PREFS_PARENT=%~3"

echo.
echo --- Item ---
echo Source: %SRC%
echo Destination: %DST%

if not exist "%SRC%" (
    echo WARNING: Source does not exist: %SRC%
    echo Skipping this item.
    exit /b 0
)

REM Ensure destination prefs parent exists
if not exist "%DST_PREFS_PARENT%" (
    echo Creating destination prefs parent folder: %DST_PREFS_PARENT%
    mkdir "%DST_PREFS_PARENT%"
)

if exist "%DST%" (
    set "BACKUP=%DST%_backup_%TIMESTAMP%"
    echo Target exists. Moving to backup: %BACKUP%
    move "%DST%" "%BACKUP%"
)

echo Creating symbolic link:
echo mklink /D "%DST%" "%SRC%"
mklink /D "%DST%" "%SRC%"
if %errorlevel% neq 0 (
    echo ERROR: mklink failed. Run as Administrator and check paths.
)
echo Link created successfully.
exit /b 0
