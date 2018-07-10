# Convert Raw disk Image file into Virtual Box `.vdi`

Working with **[DietPi][DietPi]**, we found going back and forth between 
the [Raspberry Pi][RPi] and PC very time consuming.
Fortuantely **[DietPi](https://dietpi.com/#download)** provides a PC distribution 
with *x86_x64* configuration.
Though they had both **VM Ware** and **Virtual Box** 
files but they did not work for us.
So we decided to download the 
**Native PC for BIOS/CSM (BETA)** option.

We later found out this is just a `.img` file, 
we could not directly use it with **Virtual Box**

With virtual box one gets a handy command [`VBoxManage`][VBM], we seek to use that.
This same command is used by famous ***[docker][docker]*** 
to create the older windows runtime *VM*.

## Using [`VBoxManage`][VBM] sub-command [`convertfromraw`][VBMCR]

We would be converting the RAW image file 
`DietPi_v6.2_NativePC-BIOS-x86_64-Stretch.img` to a `.vdi` file.

Also a bonus is that the `convertfromraw` command directly 
converts to **VDI** format by default.
Though it also supports **VMDK** and **VHD** formats also.

### In Windows:

In a terminal window remap the **Path** using the following:

```dos
PATH=%PATH%;"C:\Program Files\Oracle\VirtualBox"
```

This would allow the use of the `VBoxManage` tool any where.

In our case in the directory where the RAW image is available.

Now to Convert the Image we feed the following command

```dos
J:\ > VBoxManage convertfromraw DietPi_v6.2_NativePC-BIOS-x86_64-Stretch.img dietpi_disk.vdi

Converting from raw image file="DietPi_v6.2_NativePC-BIOS-x86_64-Stretch.img" to file="dietpi_disk.vdi"...
Creating dynamic image with size 839909376 bytes (801MB)...
```

We are done with the conversion.

### In Linux:

Depending on the type of your linux installation and distro the **VirtualBox** can be located in the 
`/usr/share/virtualbox`
Sometimes its tools are directly mapped or one need to map them.

In case you need to map them to the current terminal path then use the following:

```shell
export PATH=$PATH:/usr/share/virtualbox
```

**Note: You might need to change the virtual box path as per 
your distro / installation.**

Rest of the procedure is the same

```shell
$ VBoxManage convertfromraw DietPi_v6.2_NativePC-BIOS-x86_64-Stretch.img dietpi_disk.vdi

```

### Notice the Size ***800MB*** only !

Next we need to fix this.

-------------------------


# Extending the Size of an Virtual Box `.vdi` file

As we saw earlier we are getting only **800MB** disk file. 
That would hardly be enough even for the lean distribution 
like [DietPi][DietPi].

We need to increase the Size of this `.vdi` file.

## Using [`VBoxManage`][VBM] sub-command [`modifymedium`][VBMMM]

**Note : In order to execute this command either in Windows or on Linux make 
sure to follow the pervious path configuration step.**

The detault type is **disk** or **hdd** for this command.

We would use the `--resize` option of the `modifymedium` command.
The `--resize` option takes the storage space in ***megabytes***.

Hence if we want to increase the size to lets say ***8GB*** then 
we use the command with `--resize 8192`

Well Ideally we could use ***8000MB*** for **8GB** but in order to 
make the sector alignment correct we use a *`2^n`* value.

Here is the full command and its output:

```shell
$ VBoxManage modifymedium dietpi_disk.vdi --resize 8192

0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%

```

Now the though the `dietpi_disk.vdi` file not increase in size but its made as 
*Sparse VM disk*. 
Hence, as there is demand for space then it would increase size accordingly.

[DietPi]: https://dietpi.com/
[RPi]: https://www.raspberrypi.org/
[VBM]: https://www.virtualbox.org/manual/ch08.html
[VBMCR]: https://www.virtualbox.org/manual/ch08.html#idm5688
[VBMMM]: https://www.virtualbox.org/manual/ch08.html#vboxmanage-modifyvdi
[docker]: https://www.docker.com/
