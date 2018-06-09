# Installing AVRISP mkII with Libusb-win32 on Windows 10

Its common that one needs to use a programmer from the *Arduino IDE*, but on
Windows 10 things are not as simple. Many times the drivers installed are not
correct hence the `avrdude` tool in the *Arduino IDE* refuses to work.

Here we would look at a way to make that possible.

The idea would be first to get the **Libusb-Win32** driver installed which is 
essential for the `avrdude` to work.

**Note:** In case you have AtmelStudio installed this might have difficultly,
due to the presence of the *Jungo WinUSB* driver. We would look into moving
around that. 

Let's assume that on the PC we have the following situation:

  1. No drivers installed for **AVRISP mkII**
  2. We have Admin access
  3. We have internet available

## Getting Libusb-win32

The **libusb-win32** package available from [Sourceforge](https://sourceforge.net/projects/libusb-win32/).

https://sourceforge.net/projects/libusb-win32/

We just need to download the latest available release version of the package.

Currently it is `libusb-win32-bin-1.2.6.0.zip` as on June 2018.

Make sure to get something on the similar lines.

If you do not find, just go the **Files** section of the [Sourceforge](https://sourceforge.net/projects/libusb-win32/files/) and go to the `libusb-win32-releases` folder. Select the latest version folder and 
download the file similar to the above.


## Setting Up Driver

Next, *unarchive / unzip* the Zip file `libusb-win32-bin-1.2.6.0.zip` as on June 2018.

Inside would be a directory structure similar to :

```shell
bin/
examples/
include/
lib/
AUTHORS.txt
COPYING_GPL.txt
COPYING_LGPL.txt
installer_license.txt
libusb-win32-changelog-1.2.6.0.txt
README.txt
```

### 1. Open **`bin`** directory.

### 2. There in execute the `inf-wizard.exe` after connecting the **AVRISP mkII**.

![FirstPage](https://github.com/boseji/BookOfGists/raw/master/images/libusb-win32-avrispmkii-page1.png)

### 3. Press the *Next* button.

### 4. Select the **AVRISP mkII** in the list

![SecondPage](https://github.com/boseji/BookOfGists/raw/master/images/libusb-win32-avrispmkii-page2.png)

The Ids would be -

`VendorID: 0x03EB`

`ProductID: 0x2104`
 
Click *Next* to start the Ini creation process

### 5. Device Configuration

**Do Not Touch this**

![ThirdPage](https://github.com/boseji/BookOfGists/raw/master/images/libusb-win32-avrispmkii-page3.png)

Click *Next* to Proceed.

### 6. Save the INI file

Next it would ask to save an INI file with the name `AVRISP_mkII.inf`

Create A directory Named `AVRISP_mkII-Driver`.

Navigate to this directory and then Click on *Save*.

Next Click on Done to continue.

### 7. The Driver Directory

It should look something like 

```shell
 amd64/
 ia64/
 license/
 x86/
 AVRISP_mkII.inf
 installer_x64.exe
 installer_x86.exe
```

## Disable the Driver Signing check

We would be disabling the Driver signing check temporarily.

 1. Hold **Shift** and select *Resart* from the *Start menu*. 
 This would enter the advance setup mode.
 
 2. In Advanced Setup mode : Select **Troubleshoot** -> **Advance Options**
 
 3. In **Advanced Options** click on **See more recovery options**
 
 4. Next **Startup Settings** - This would reboot the PC and come to another
 reboot screen displaying options with function keys to select them.
 
 5. Press **F7** this would select to **Disable Driver Signature Enforcement** -
 Again the system would reboot into normal windows.
 
 6. Now Open the *Device Manager* : Right click the **AVRISP mkII** under *Other devices* and select Update driver.
 
 7. Select the Directory Location where we earlier stored the generated driver.
 
 8. Windows 10 would show ***Warning*** about driver having no signature. 
 Don't worry its not a problem just select **Install Anyway...**
 
 9. Now you should be able to see **libusb-win32 devices** under which the 
 **AVRISP mkII** is present in *Device Manager*
 
 10. We need to restore the Driver signing. Open an **Administrator Command Prompt**.
 
 11. In **Administrator Command Prompt** Type Command :
 `BCDEDIT /set nointegritychecks OFF`
 This would re-Enable the **Disable Driver Signature Enforcement**
 
 12. Reboot the PC normally.

**Note:** The Easy way to the enable and disable **Driver Signature Enforcement**
Are 2 commands for an *Administrator Command Prompt*:

  1. To **disable** device driver signing, type `BCDEDIT /set nointegritychecks ON` then press **Enter**
  2. To **enable** device driver signing, type `BCDEDIT /set nointegritychecks OFF` then press **Enter**

## Finally
 
This completes the Installation of the Driver.

Now we can try and check in the *Arduino IDE* if the `avrdude` interface works with
**AVRISP mkII**

*The [generated Driver is included here](https://github.com/boseji/BookOfGists/raw/master/files/AVRISP_mkII-Driver.zip).*

