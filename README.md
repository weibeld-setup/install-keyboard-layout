# Logitech Swiss German Keyboard Layout

Logitech's version of the Swiss German keyboard layout.

## Characteristics

Some differences to the standard Swiss German keyboard layout include:

| Character | Logitech Swiss German | Swiss German    |
|-----------|-----------------------|-----------------|
| `~`       | `OptionRight-N`       | `OptionRight-^` |
| `\|`      | `OptionRight-1`       | `OptionRight-7` |

## Installation on macOS

> Tested on macOS Sonoma 14.2.1

1. Download [`LogitechSwissGerman.keylayout`](macos/LogitechSwissGerman.keylayout) and [`LogitechSwissGerman.icns`](macos/LogitechSwissGerman.icns)
1. Copy both files to `/Library/Keyboard Layouts`:
    ```bash
    sudo cp LogitechSwissGerman.keylayout LogitechSwissGerman.icns /Library/Keyboard\ Layouts
    ```
1. Log out of macOS and log in again
1. Go to **_System Preferences > Keyboard > Text Input > Edit... > [+] > Others_** and add the _Logitech Swiss German_ keyboard layout

## Installation on Windows

> Tested on Windows 10.

To install the keyboard layout, you must build a setup package from the [`LogSwiss.klc`](windows/LogSwiss.klc) file and then install this package:

1. Download and install the [Microsoft Keyboard Layout Creator (MSKLC)](https://www.microsoft.com/en-us/download/details.aspx?id=102134)
1. Download the [`LogSwiss.klc`](windows/LogSwiss.klc) file
1. In MSKLC, go to _**File > Load Source File...**_ and select the `LogSwiss.klc` file
1. Click on _**Project > Build DLL and Setup Package**_
   > This creates a directory called `logswiss` in the current working directory. There may be a dialog notifying about warnings during the build which can be ignored.
1. Run `setup.exe` in the `logswiss` directory
   >  This installs the keyboard layout as `LogSwiss.dll` to `C:\Windows\System32` and creates the necessary registry entries to make the layout available to the system. If `setup.exe` doesn't work for some reason, try executing `LogSwiss_amd64.msi`.
1. In Windows, go to **_Settings > Time & Language > Language > Keyboard > Override for default input method_** and activate the _Logitech Swiss German_ keyboard layout

> **Important:** after installation, keep the `logswiss` directory somewhere on the system. This is because the contained `setup.exe` file allows to cleanly uninstall the keyboard layout. Uninstallation is necessary if you want to make changes to the keyboard layout (i.e. by editing the `.klc` file and rebuilding the DLL and setup package as described above) and then reapply it. The reason that the keyboard layout must be uninstalled before making changes is that MSKLC refuses to export a keyboard layout with the same name as an already installed keyboard layout.

> **Note:** the name of a keyboard layout file on Windows may have at most 8 characters (excluding the extension). That's why the name of the file has been shortened to `LogSwiss.klc`.

## Sources

### macOS

The macOS `LogitechSwissGerman.keylayout` file has been obtained as-is from an unknown source.

### Windows

The Windows `LogSwiss.klc` file has been generated from the `LogitechSwissGerman.keylayout` file as follows:

1. Download the [`LogitechSwissGerman.keylayout`](macos/LogitechSwissGerman.keylayout) file
1. Clone the [adobe-type-tools/keyboard-layouts](https://github.com/adobe-type-tools/keyboard-layouts) repository
1. Execute the following command in the root directory of the cloned repository:
    ```bash
    python mac2winKeyboard.py ../LogitechSwissGerman.keylayout
    ```
    > This creates a file named `Logitech.klc` in the current working directory.
1. Do the manual adaptations on the `Logitech.klc` file as described below
1. Rename the file to `LogSwiss.klc`

#### Adaptations

The `OEM_3` key (containing `§ `, `/`, and `°`) and the `OEM_102` key (containing `<`, `/`, and `>`) seem to be swapped on macOS and Windows keyboards. For this reason, do the following adaptations in the generated `Logitech.klc` file:

1. Change the following line:
   ```bash
   29	OEM_3		SGCap	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
   ```
   to:
   ```bash
   29	OEM_3		SGCap	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
   ```
1. Change the following line:
   ```bash
   56	OEM_102		0	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
   ```
   to:
   ```bash
   56	OEM_102		0	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
   ```
