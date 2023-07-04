# Logitech Swiss German Keyboard Layout

Keyboard layout with some slight deviations from the default Swiss German keyboard layout that comes preinstalled on most systems.

## Characteristics

The most important differences to the default Swiss German keyboard layout are:

- Tilde ("~") is `Alt(Right)-N` instead of `Alt(Right)-^`
- Vertical bar ("|") is `Alt(Right)-1` instead of `Alt(Right)-7`

> There may be more differences that are not listed here and the default Swiss German keyboard layout may differ between systems.

## Origin

I orignally obtained the macOS keyboard layout file ([`LogitechSwissGerman.keylayout`](macos/LogitechSwissGerman.keylayout)) from a source that I can't remember. The Windows file ([`LogSwiss.klc`](windows/LogSwiss.klc)) has been generated from the macOS version, as described in [macOS to Windows conversion](#macos-to-windows-conversion).

## Installation

### macOS

> The following instructions have been tested on macOS 13.4.

On macOS, you can simply copy the `.keylayout` and `.icns` files to `/Library/Keyboard Layouts`:

```bash
cp LogitechSwissGerman.keylayout LogitechSwissGerman.icns /Library/Keyboard\ Layouts
```

> The `.icns` file contains the icon for the keyboard layout and is not strictly necessary.

Then, you should be able to find and activate the _Logitech Swiss German_ keyboard layout as follows:

**_System Preferences > Keyboard > Text Input > Edit... > + > Others > Add_**

### Windows

> The following instructions have been tested on Windows 10.

On Windows, it's necessary to build an installable package from the `.klc` file and then install this package. To do so, proceed as follows:

1. Download and install the [Microsoft Keyboard Layout Creator (MSKLC)](https://www.microsoft.com/en-us/download/details.aspx?id=102134)
1. In MSKLC, go to _File > Load Source File..._ and select the `LogSwiss.klc` file
1. Click on _Project > Build DLL and Setup Package_
   - This creates a directory called `logswiss` in the current working directory
   - There may be a dialog notifying about warnings during the build which can be ignored
1. Open the `logswiss` directory and execute `setup.exe`
   - This installs the keyboard layout as `LogSwiss.dll` to `C:\Windows\System32` and creates the necessary registry entries to make the layout available to the system
   - If `setup.exe` doesn't work for some reason, use `LogSwiss_amd64.msi`

Now, you should be able to find and activate the _Logitech Swiss German_ keyboard layout as follows:

**_Settings > Time & Language > Language > Keyboard > Override for default input method_**

After installation, keep the `logswiss` directory somewhere on the system as the `setup.exe` file allows to cleanly uninstall the keyboard layout. Uninstallation is necessary if you want to make any changes to the layout and then reinstall it because MSKLC refuses to export a keyboard layout with the same name as an already installed one. In this case, first uninstall the existing keyboard layout with `setup.exe`, edit the `.klc` file and then repeat the installation steps above.

> **Note:** the name of a keyboard layout file on Windows may have at most 8 characters (excluding the extension). That's why the name of the file has been shortened to `LogSwiss.klc`.

## macOS to Windows conversion

As mentioned, the Windows keyboard layout file has been generated from the macOS file. This requires converting a `.keylayout` file to a `.klc` file, which can be done as follows:

1. Clone the [adobe-type-tools/keyboard-layouts](https://github.com/adobe-type-tools/keyboard-layouts) repository
1. Change into the repository and execute the following command:
    ```bash
    python mac2winKeyboard.py ../LogitechSwissGerman.keylayout
    ```

The above converts the `.keylayout` file to a `.klc` file named `Logitech.klc`

> The file name is shortened to 8 characters (excluding extension) because Windows key layout files may only have names with at most 8 characters.

For some reason, the `OEM_3` key (§/°) and `OEM_102` key (</>) seem to be swapped on macOS and Windows keyboards. For this reason, the corresponding mappings have bene manually swapped in the generated `.klc` file as follows:

```bash
# Generated
29	OEM_3		SGCap	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
# New
29	OEM_3		SGCap	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
```

```bash
# Generated
56	OEM_102		0	00a7	00b0	-1	-1	2264	2265	// SECTION SIGN, DEGREE SIGN, <none>, <none>, LESS-THAN OR EQUAL TO, GREATER-THAN OR EQUAL TO
# New
56	OEM_102		0	003c	003e	-1	-1	005c	2030	// LESS-THAN SIGN, GREATER-THAN SIGN, <none>, <none>, REVERSE SOLIDUS, PER MILLE SIGN
```
