# Keyboard Layout

![macOS](https://img.shields.io/badge/macOS-lightgreen?logo=apple&labelColor=green&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-lightblue?logo=windows&labelColor=blue&logoColor=white)

Custom keyboard layout based on the Logitech Swiss German keyboard layout.

## How to use it?


## What is it?


## Features

Differences between this keyboard layout, the Logitech Swiss German keyboard layout, and the macOS native Swiss German keyboard layout:

| Char  | Description | Custom                  | Logitech Swiss German | Native Swiss German |
|:-----:|-------------|-------------------------|-----------------------|---------------------|
| \|    |             | _**Opt-1**_             | _Opt-1_               | _Opt-7_             |
| –     | en-dash     | _**Opt-[-]**_           | _Opt-[-]_             | _Opt-[-]_           |
| —     | em-dash     | _**Ctrl-Opt-[-]**_      | —                     | —                   |
| āēīōū |             | _**Opt-[AEIOU]**_       | —                     | —                   |
| ĀĒĪŌŪ |             | _**Opt-Shift-[AEIOU]**_ | —                     | —                   |
| €     |             | _**Opt-R**_             | _Opt-E_               | _Opt-E_             |

## Installation

### macOS

1. Download [`CustomSwissGerman.keylayout`](https://raw.githubusercontent.com/weibeld-personalisation/custom-swiss-german-keyboard-layout/main/CustomSwissGerman.keylayout) and [`CustomSwissGerman.icns`](https://raw.githubusercontent.com/weibeld-personalisation/custom-swiss-german-keyboard-layout/main/CustomSwissGerman.icns)
1. Copy both files to `/Library/Keyboard Layouts`:
    ```bash
    sudo cp CustomSwissGerman.* /Library/Keyboard\ Layouts
    ```
1. Log out of macOS and log in again
1. Go to **_System Preferences > Keyboard > Text Input > Edit... > [+] > Others_**
1. Locate and add the the **Custom Swiss German** keyboard layout

### Windows

> Tested on Windows 10.

To install the keyboard layout, you must build a setup package from the [`CustomSw.klc`](CustomSw.klc) file and then install this package:

1. Download and install the [Microsoft Keyboard Layout Creator (MSKLC)](https://www.microsoft.com/en-us/download/details.aspx?id=102134)
1. Download the [`CustomSw.klc`](CustomSw.klc) file
1. In MSKLC, go to _**File > Load Source File...**_ and select the `CustomSw.klc` file
1. Click on _**Project > Build DLL and Setup Package**_
   > This creates a directory called `customsw` in the current working directory. There may be a dialog notifying about warnings during the build which can be ignored.
1. Run `setup.exe` in the `customsw` directory
   >  This installs the keyboard layout as `CustomSw.dll` to `C:\Windows\System32` and creates the necessary registry entries to make the layout available to the system. If `setup.exe` doesn't work for some reason, try executing `CustomSw_amd64.msi`.
1. In Windows, go to **_Settings > Time & Language > Language > Keyboard > Override for default input method_** and activate the **Custom Swiss German** keyboard layout

> **Important:** after installation, keep the `customsw` directory somewhere on the system. This is because the contained `setup.exe` file allows to cleanly uninstall the keyboard layout. Uninstallation is necessary if you want to make changes to the keyboard layout (i.e. by editing the `.klc` file and rebuilding the DLL and setup package as described above) and then reapply it. The reason that the keyboard layout must be uninstalled before making changes is that MSKLC refuses to export a keyboard layout with the same name as an already installed keyboard layout.

> **Note:** the name of a keyboard layout file on Windows may have at most 8 characters (excluding the extension). That's why the name of the file has been shortened to `CustomSw.klc`.

## Notes

1. The original [Logitech Swiss German](LogitechSwissGerman.keylayout) keyboard layout has been obtained from [Ukelele](https://software.sil.org/ukelele).
   - The keyboard layout files that ship with Ukelele can be retrieved by downloading the [Ukelele DMG file](https://software.sil.org/ukelele/#downloads), double-clicking on it, and going to `Resources/Standard Keyboards`. The Logitech Swiss German keyboard layout is located in `Logitech Keyboard Layouts.bundle`.
1. The [Custom Swiss German](CustomSwissGerman.keylayout) keyboard layout for macOS has been created with Ukelele based on the [Logitech Swiss German](LogitechSwissGerman.keylayout) keyboard layout extracted from Ukelele.
1. The Custom Swiss German keyboard layout for Windows ([`CustomSw.klc`](CustomSw.klc)) has been generated from the corresponding macOS version as explained in [Windows Keyboard Layout File Generation](#windows-keyboard-layout-file-generation) below.

## Windows Keyboard Layout File Generation

To generate the [`CustomSw.klc`](CustomSw.klc) file from the [`CustomSwissGerman.keylayout`](CustomSwissGerman.keylayout) file, proceed as follows:

1. Clone [adobe-type-tools/keyboard-layouts](https://github.com/adobe-type-tools/keyboard-layouts):
   ```bash
   git clone https://github.com/adobe-type-tools/keyboard-layouts
   ```
1. Execute the following command:
    ```bash
    python3 mac2winKeyboard.py CustomSwissGerman.keylayout
    ```
    > This should create or overwrite the `CustomSw.klc` file in the same directory as the `CustomSwissGerman.keylayout` file.
1. Do the following manual fixes in the `CustomSw.klc` file:
   1. Change the line:
      ```bash
      29  OEM_3		SGCap	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
      ```
      to:
      ```bash
      29  OEM_3		SGCap	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
      ```
   1. Change the line:
      ```bash
      56  OEM_102		0	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
      ```
      to:
      ```bash
      56  OEM_102		0	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
      ```
   > The above fixes are necessary because the `OEM_3` key (`§ `/`/`/`°`) and `OEM_102` key (`<`/`/`/`>`) seem to be swapped on macOS keyboards with respect to Windows keyboards.
