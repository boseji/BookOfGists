# Flash bootloader for Cheap STM32F103C8T boards (BluePill board)

![Cheap STM32 Board](http://s1.bild.me/bilder/110417/88150221486874340.jpg)

Original - <https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Flashing-Bootloader-for-BluePill-Boards>

~~These boards are low cost and very well supported by **[STM32 Arduino](https://github.com/rogerclarkmelbourne/Arduino_STM32)**~~

Officially Supported now at **[STM32duino](https://github.com/stm32duino/Arduino_Core_STM32)**

One needs to flash the Bootloader to make this board work with modified **MAPLE booloader**.

For that here are the Steps:

## 1. Get the Required Bootloader File

Here is a **[link](https://github.com/rogerclarkmelbourne/STM32duino-bootloader/tree/master/binaries)** to all the binary files 
available in precompiled form.

The specific file needs to be selected.

### Selection Citeria

If you have the above board most linkly the **LED is on PIN PC13** then the file to use is ***[generic_boot20_pc13.bin](https://github.com/rogerclarkmelbourne/STM32duino-bootloader/blob/master/binaries/generic_boot20_pc13.bin)***

Note this file also needs a **USB Disconnect** pin connected to **Pin PB9**

### Circuit for USB Connection

### Fast Fix

Figure shows the modification needed for the `R10` resistance that comes populated on the board.

![USB Resistor Mod](http://s1.bild.me/bilder/110417/2401149STM32_Blue_Pill_bottom.jpg)

One needs to replace this to a **1.5k** resistance.

The Board would still need reset button press some times in case the USB Serial is not getting detected.

### Another Alternative

In case you can't affort the fancy circuit shown above, then use a **Pull up** of **1.5k** on **Pin PA12**

This is the **USB D+** line of the MCU.

Here is a picture showing the MOD

![Modification for USB](http://s1.bild.me/bilder/110417/3203170USB-Jumper-MOD.png)

Though this would work, but the USB reset some times fails with this MOD.

One needs to disconnect the board from USB and then reconnect to recover some times.

### More Professional Mod

Figure shows the **circuit needed** for ***USB Connection to STM32F103C8T boards***:

![USB](http://s1.bild.me/bilder/110417/9787895Usb-connfor-STM32F103C8T-Cheap-boards.JPG)


## 2. Setting up the Firmware

First connect the board using the **ST-LINK** programmer.

Copy the firmware file **generic_boot20_pc13.bin** or what ever in your case to :

`(Arduino)\hardware\Arduino_STM32-master\tools\win` - **Windows**

In case of windows this would correct in case of linux use the specific directory.

`(Arduino)\hardware\Arduino_STM32-master\tools\linux64` - **Linux 64-bit**

`(Arduino)\hardware\Arduino_STM32-master\tools\linux` - **Linux 32-bit**

Here the `(Arduino)` is the default folder where the Arduino Sketches are stored.

### Windows Command

In case of **Windows** we run the command:

`stlink\ST-LINK_CLI.exe -c SWD -P generic_boot20_pc13.bin 0x8000000 -Rst -Run`

To Flash the board. Here `generic_boot20_pc13.bin` needs to be replaced by the respective bin file selected earlier.

### Linux Command

`./stlink/st-flash write generic_boot20_pc13.bin 0x8000000`

`./upload-reset`

To Flash the board. Here `generic_boot20_pc13.bin` needs to be replaced by the respective bin file selected earlier.
The last command would reset the board to begin execution.

*Note:* The new `write` keyword in the latest version of [texane/stlink](https://github.com/texane/stlink).


## Further Reading

https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/stm32duino-bootloader

https://github.com/rogerclarkmelbourne/STM32duino-bootloader

https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Bootloader

