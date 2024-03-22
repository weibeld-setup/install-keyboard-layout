# Keyboard Layout

![macOS](https://raw.githubusercontent.com/weibeld-setup/.github/main/badges/macos.svg)
![Windows](https://raw.githubusercontent.com/weibeld-setup/.github/main/badges/windows.svg)

Custom keyboard layout based on the Logitech Swiss German keyboard layout.

## Installation

### macOS

![macOS](https://raw.githubusercontent.com/weibeld-setup/.github/main/badges/macos.svg)

1. Download [`CustomSwissGerman.keylayout`](https://raw.githubusercontent.com/weibeld-setup/install-keyboard-layout/main/macos/CustomSwissGerman.keylayout) and [`CustomSwissGerman.icns`](https://raw.githubusercontent.com/weibeld-setup/install-keyboard-layout/main/macos/CustomSwissGerman.icns)
1. Copy both files to `/Library/Keyboard Layouts`:
    ```bash
    sudo cp CustomSwissGerman.* /Library/Keyboard\ Layouts
    ```
1. Log out of macOS and log in again
1. Go to **_System Preferences > Keyboard > Text Input > Edit... > [+] > Others_**
1. Locate the **Custom Swiss German** keyboard layout in the list and add it

### Windows

![Windows](https://raw.githubusercontent.com/weibeld-setup/.github/main/badges/windows.svg)

> Tested on Windows 10.

To install the keyboard layout, you must build a setup package from the [`CustomSw.klc`](windows/CustomSw.klc) file and then install this package:

1. Download and install the [Microsoft Keyboard Layout Creator (MSKLC)](https://www.microsoft.com/en-us/download/details.aspx?id=102134)
1. Download the [`CustomSw.klc`](windows/CustomSw.klc) file
1. In MSKLC, go to _**File > Load Source File...**_ and select the `CustomSw.klc` file
1. Click on _**Project > Build DLL and Setup Package**_
   > This creates a directory called `customsw` in the current working directory. There may be a dialog notifying about warnings during the build which can be ignored.
1. Run `setup.exe` in the `customsw` directory
   >  This installs the keyboard layout as `CustomSw.dll` to `C:\Windows\System32` and creates the necessary registry entries to make the layout available to the system. If `setup.exe` doesn't work for some reason, try executing `CustomSw_amd64.msi`.
1. In Windows, go to **_Settings > Time & Language > Language > Keyboard > Override for default input method_** and activate the **Custom Swiss German** keyboard layout

> **Important:** after the installation, keep the `customsw` directory somewhere on the system. This is because the contained `setup.exe` file allows to cleanly uninstall the keyboard layout. Uninstallation is necessary if you want to make changes to the keyboard layout (i.e. by editing the `.klc` file and rebuilding the DLL and setup package as described above) and then reapply it. The reason that the keyboard layout must be uninstalled before making changes is that MSKLC refuses to export a keyboard layout with the same name as an already installed keyboard layout.

## Notes

### Keyboard layout

The keyboard layout is based on the Logitech Swiss German layout. The differences to the Logitech Swiss German layout, and also the native macOS Swiss German layout, are listed below:

| Character               | Custom              | Logitech Swiss German | macOS Swiss German |
|:-----------------------:|---------------------|-----------------------|---------------------|
| `|`                     | `Opt-1`             | `Opt-1`               | `Opt-7`             |
| `–` (en dash)           | `Opt-[-]`           | `Opt-[-]`             | `Opt-[-]`           |
| `—` (em dash)           | `Ctrl-Opt-[-]`      | —                     | —                   |
| `ā`, `ē`, `ī`, `ō`, `ū` | `Opt-[AEIOU]`       | —                     | —                   |
| `Ā`, `Ē`, `Ī`, `Ō`, `Ū` | `Opt-Shift-[AEIOU]` | —                     | —                   |
| `€`                     | `Opt-R`             | `Opt-E`               | `Opt-E`             |

> As can be seen, the custom keyboard layout mainly adds combinations for typing vowels with macrons (e.g `ā`) as they are used, for example, in Japanese romanisation systems.

### File origins

**[`LogitechSwissGerman.keylayout`](macos/LogitechSwissGerman.keylayout):**

Obtained from [Ukelele](https://software.sil.org/ukelele) by opening its [DMG file](https://software.sil.org/ukelele/#downloads) and navigating to _**Resources/Standard Keyboards**_. The Logitech Swiss German keyboard layout is located in the _**Logitech Keyboard Layouts.bundle**_ file.

**[`CustomSwissGerman.keylayout`](macos/CustomSwissGerman.keylayout)**

Created with Ukelele by using the `LogitechSwissGerman.keylayout` as a base.

**[`CustomSw.klc`](windows/CustomSw.klc)**

Generated from `LogitechSwissGerman.keylayout` as explained in [Windows keyboard layout file generation](#windows-keyboard-layout-file-generation) below.

### Windows keyboard layout file generation

The following explains how to generate the `CustomSw.klc` file from `CustomSwissGerman.keylayout`:

1. Clone [adobe-type-tools/keyboard-layouts](https://github.com/adobe-type-tools/keyboard-layouts):
   ```bash
   git clone https://github.com/adobe-type-tools/keyboard-layouts
   ```
1. Execute the following command:
    ```bash
    python3 mac2winKeyboard.py CustomSwissGerman.keylayout
    ```
1. Do the following manual fixes in the created `CustomSw.klc` file:
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
    The above fixes are necessary because the `OEM_3` key (`§ `/`/`/`°`) and `OEM_102` key (`<`/`/`/`>`) seem to be swapped on macOS keyboards with respect to Windows keyboards.
is 
> **Note:** keyboard layout file names on Windows can have at most 8 characters (excluding the extension). That's why the name of the output file is shortened to `CustomSw.klc`.
