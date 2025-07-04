## Description
This is a fork of the [PocketPD](https://github.com/CentyLab/PocketPD) from CentyLab.
The goal of this fork is to trail minor usability and saftey improvents in the state machine.

Changes include:
+ If no PPS support, then transition from CAPDISPLAY to MENU to allow immediate PDO voltage selection
+ If transitioning to MENU then turn off output to improve safety 
+ If in MENU, then a short encoder press will transition states to improve usability 
+ If in BOOT or CAPDISPLAY, encoder rotation will transition states to MENU
+ Long press duration has been reduced to 1.5 seconds
+ '*' has been added to version number to indicate non-official release


## Links
* [PocketPD Hackaday's Project](https://hackaday.io/project/194295-pocketpd-usb-c-portable-bench-power-supply)

## System flow chart (not up-to-date)

```mermaid
flowchart LR
    A[Boot up] --> |0.5s|B[Obtain charger capability]
    B --> |1.5s|C[Display capabilities]
    C --> |3s|D[Normal operation]
    D --> |Long Press V/I|E[Menu]
    B --> |Short Press any button|D
    C --> |Short Press any button|D
    E --> |Long Press V/I|D
    E --> |Short Press Encoder|D
```

## Operational manual
If your charger support PPS (Programable Power Supply) mode, the charger will first enter BOOT screen.

<p align="center" width="100%">
    <img width="40%" src="media/bootscreen.jpg">
</p>

The system will then display the available profile from the charger.

<p align="center" width="100%">
    <img width="40%" src="media/ppsprofile.jpg">
</p>
After 3 seconds, the system will enter operating mode. If PPS mode exist, the system will request 5V @ 1A

<p align="center" width="100%">
    <img width="40%" src="media/normalpps.jpg">
</p>


In NORMAL state:
+ Turning the encoder to increase/decrease voltage/current
+ Short press encoder to change increment from fine to coarse
+ Short press Volt/Amp button to switch between adjusting Voltage or Current
+ Short press On/Off button to enable output
+ Long press Volt/Amp button to enter MENU

In MENU state:
+ Turning the encoder to select profile
+ Short press encoder to activate profile
+ Long press Volt/Amp button to return to normal operation and cancel profile change

<p align="center" width="100%">
    <img width="40%" src="media/ppsmenu.jpg">
</p>

Example when select 5V @ 3A profile 

<p align="center" width="100%">
    <img width="40%" src="media/normalpdo5V.jpg">
</p>
<br>

**Note**: If your charger doesn't support PPS profile, PocketPD will directly boot into the Menu state. Once you select a PDO voltage your menu will looks like this:

<p align="center" width="100%">
    <img width="40%" src="media/pdoprofile.jpg">
</p>


## Compile the code
+ You will need [VSCode](https://code.visualstudio.com/download) with [Platform IO extension](https://docs.platformio.org/en/latest/integration/ide/vscode.html#installation).

+ Before letting Platform IO pulling the pico-sdk files. Follow [Important steps for Windows users, before installing](https://arduino-pico.readthedocs.io/en/latest/platformio.html#important-steps-for-windows-users-before-installing)
Else you will encounter:

```
VCSBaseException: VCS: Could not process command ['git', 'clone', '--recursive', 'https://github.com/earlephilhower/arduino-pico.git', 'C:\\Users\\keylo\\.platformio\\.cache\\tmp\\pkg-installing-iypaogfn']
```

+ Go to PlatformIO extension -> Pico -> General -> Build

+ Output of the build process will be in .pio/build/pico/


## How to flash new firmware
Note: Firmware at and before `0.9.5` is only for `PocketPD HW1.0`

Step 1: Select the correct hardware version from [PocketPD's Firmware Releases](https://github.com/CentyLab/PocketPD/releases)

+ HW1.0: Also known as "Limited edition". Download `firmware_xx_HW1.0.uf2`
+ HW1.1: Our standard production version. Download `firmware_xx_HW1.1.uf2`

Step 2: Mount PocketPD as a drive in your computer

For MacBook user:
+ Method 1: (Easy)
    + Short the BOOT pads at the back of the device with a tweezer in `HW1.0` or hold the BOOT button in `HW1.1`.
    + Use a USB-A -> USB-C adapter, then use a USB-A -> USB-C cable to connect PocketPD to computer. PocketPD should pop up as `RPI_RP2` drive.
+ Method 2: (Intermediate)
    + Use a USB-A -> USB-C adapter, then use a USB-A -> USB-C cable to connect PocketPD to computer. No drive will popup.
    + Use any serial monitor, and start a Serial port with 1200 Baudrate. PocketPD should pop up as `RPI_RP2` drive.

For Windows user: 
+ Method 1: (Easy)
    + Short the BOOT pads at the back of the device with a tweezer in `HW1.0` or hold the BOOT button in `HW1.1`.
    + Use any USB cable to connect PocketPD to computer. PocketPD should pop up as `RPI_RP2` drive.
+ Method 2: (Intermediate)
    + Use any USB cable to connect PocketPD to computer. No drive will pop-up.
    + Open [Putty](https://www.putty.org/) and open a Serial port with 1200 Baudrate. PocketPD should pop up as `RPI_RP2` drive.

Step 3: Drag and drop the `.uf2` file into the drive


If you build the firmware directly from VSCode, the `.uf2` file will be in `.pio/build/pico/`

Detail guide [How to upload new firmware to PocketPD](https://github.com/CentyLab/PocketPD/wiki/How-to-upload-new-firmware-to-PocketPD)
