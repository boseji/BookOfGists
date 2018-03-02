# OpenOCD Debugger Software

## Links:

  * Details about OpenOCD command:<br>
  https://linux.die.net/man/1/openocd
  
  * Flasing Cortex-M3 using [ARM-USB-OCD-H](https://www.olimex.com/Products/ARM/JTAG/ARM-USB-OCD-H/) JTAG adapter from Olimex:<br>
  https://jacobmossberg.se/posts/2017/01/10/using-openocd-flash-arm-cortex-m3.html
  
  * Another OpenOCD based Tutorial:<br>
  https://balau82.wordpress.com/2013/08/14/flashing-the-stm32-p152-board-with-openocd/

  * STM32 Mass Erase using OpenOCD:<br>
  https://www.lxrobotics.com/erase-the-complete-flash-memory-on-a-stm32-with-openocd
  
  * STM32F0 debugging and build with OpenOCD:<br>
  http://www.hertaville.com/stm32f0discovery-command-line-ide.html
  
## Command line with OpenOCD

These commands assume that the folling variables are initialized

  * `OPENOCD` this variable stores the path to the OpenOCD folder<br>
  *e.g.* `C:\OpenOCD-20170821\bin` on a windows PC and possibly <br>
  `/usr/bin/` on Linux
  
  * `OPENOCD_SCRIPT` this variable stores the path to the OpenOCD scripts folder<br>
  *e.g.* `C:\OpenOCD-20170821\share\openocd\scripts` on a windows PC and possibly <br>
  `/usr/lib/openocd/scripts` on Linux

Based on these assumptions we now start writing the Commands.

### Know your Processor

On Windows:

```batch
%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg -c "init" -c "flash probe 0" -c "exit 0x0"
```

Let's look at what esential parts form this command:

  * Invocation of OpenOCD:<br>
  `%OPENOCD%\openocd` this actually executes the `openocd.exe` on *Windows* or the Linux executable.
  
  * Programmer / Debugger Sepecification:<br>
  This is also knon as the interface specification. In the above example we have a **CMSIS-DAP** interface. <br>
  `-f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg` This part is where its specified<br>
  This can be of various depending on the type of programmer or debugger available with the target.<br>
  To explore more see the directory `%OPENOCD_SCRIPT%\interface` folder or `$OPENOCD_SCRIPT/interface` on Linux<br>  
  Also note that not all the interfaces support the **JTAG** and **SWD** together.<br>
  In our case CMSIS-DAP or DAPLink support both based on the ARM Hardware Interface Circuit chip used.
  
  * Process / MCU Specification:<br>
  This details the actual microcontroller type as well as memory interface.<br>
  ` -f %OPENOCD_SCRIPT%\target\nrf51.cfg ` Here we are specifcing the **nRF51822** as type of MCU
  
  * Command Parameter List: <br>
  This gives the exact sequence in which commands can be sent to the debugger as command arguments. 
  In case no commands are specified then the OpenOCD enters the **Daemon mode** where we can connect through *telnet*.<br>
  `-c "init" -c "flash probe 0" -c "exit 0x0"` This the command list from the above command.

Now, Let's look at the sequence of commands-parameter-list sent for **Know your Processor** command:

  * Initialize the Processor:<br>
  `-c "init"` This command would begin debugger execution
  
  * Check what type of Flash is attached:<br>
  `-c "flash probe 0"` This command would print details about thhe size, Chip and type of memory.
  
  * Terminate the Commandline after executing the Command Parameter sequence:
  `-c "exit 0x0"` This would prevent the OpenOCD from going into the **Daemon Mode** 
  and exit after completion of the Command Parameter List.

### Halt your processor

```batch
SET CMD1="%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg"
%CMD1% -c "init" -c "halt" -c "exit 0x0"
```

### Reset you processor

```batch
SET CMD1="%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg"
%CMD1% -c "init" -c "reset" -c "exit 0x0"
```

### Mass Erase your Processor

```batch
SET CMD1="%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg"
%CMD1% -c "init" -c "halt" -c "nrf51 mass_erase 0" -c "exit 0x0"
```

Since here our target is nRF51822 so we use the `nrf51` specific commands.

Similar commands are available for `stm32` etc.

Command format is `<Specific Processor Commands> mass_erase <Bank No>`

### Sector Erase

```batch
SET CMD1="%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg"
%CMD1% -c "init" -c "halt" -c "flash probe 0" -c "flash erase_sector 0 0 5" -c "exit 0x0"
```

The Flash Erase command is generic and does not need a special Processor part.

Here is the command Format `flash erase_sector <Bank No> <Start Sector> <End Sector>`

In the above case from Bank 0 we are erasing Sectors 0 through 5.

One needs to know the actual number of sectors the memory is devided in else you might not be able to use this command properly.

### Read DWORD from memory

```batch
SET CMD1="%OPENOCD%\openocd -f %OPENOCD_SCRIPT%\interface\cmsis-dap.cfg -f %OPENOCD_SCRIPT%\target\nrf51.cfg"
%CMD1% -c "init" -c "halt" -c "flash probe 0" -c "mdw 0x200 5" -c "exit 0x0"
```

This would read double words from the memory of the traget.

The format is `mdw <Start Memory address in Hex> <Number of DWORDs to display>`

This command alows read from the entire memory space in the chip.

Note that kepping the chip in *halt* state helps as the values would not change abruptly.


