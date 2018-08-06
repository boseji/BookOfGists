# ESP8266 Unofficial Windows Development Environment : a Restart in 2018

Dated : 06th August 2018

It has been long time since I worked on ESP8266 and that too native SDK from Esperssif.
With the rusty knowhow and lack on info online, I had a mixed start again.
Hope that the experieince shared here might be helpful for many users,
who wish to do ESP8266 native SDK development on Windows Platform.
But are afraid that most of the working tools for ESP8266 are linux friendly.

>*Not to worry you are in good company.*

Most of Windows users like myself first try to see if there are any docker alternatives.

But sadly if you try to build the toolchain on your own you can land-up deep into troubled waters.
Its not impossible but takes lots of bandwidth, space and *hair ripping* to get a working setup.

The I found an old repository I had worked on long back fixing some issues:

https://github.com/boseji/esp8266-devkit

Well its a fork of the original work:

https://github.com/CHERTS/esp8266-devkit

[Mr. Mikhail Grigorev](https://programs74.ru/index-en.html) 
has been kind enough to provide us with packaged installer.

However the story was not as stright forward as walk in the park.

It took actually effort of 2 days to get a basic program 
running on Wemos D1 Mini that hosts the ESP-12e Module of ESP8266.

Here is how you can avoid suffering and have a quick running setup.

## Getting All Main stuff downloaded

There are 3 main pieces of software that needs to be installed in your windows machine.

### \#1 [Unofficial DevKit for ESP8266 v2.2.1](https://programs74.ru/udkew-en.html)

![Icon image of the Unofficial DevKit for ESP8266 v2.2.1][Icon-image1]

[**Espressif-ESP8266-DevKit-v2.2.1-x86.exe**](http://dl.programs74.ru/get.php?file=EspressifESP8266DevKit)

**MD5: ca6ba36c2dc9c3821c1e631d1d7c8580**

From the website released nearly 2 years ago but still works great if used correctly.

Links:
 * https://programs74.ru/udkew-en.html
 * http://dl.programs74.ru/get.php?file=EspressifESP8266DevKit

This can be installed without any fear of corruption or damage to your already delicate Windows machine.

Just that this would create `C:\Espressif` directory where all would get installed.

We would revisit this directory when we finish the **MinGW** installation and to upgrade the **SDK** from Espressif.

### \#2 [MinGW Setup](https://sourceforge.net/projects/mingw/files/Installer/)

[![Image of MinGW Installer](https://github.com/boseji/BookOfGists/raw/master/images/ESP8266-unoff-sdk-win-1.png)](
https://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download)

This is the special saus needed to do makefile stuff and provide the tools needed to emulate Linux tools.

For more experienced user **WARNING :** ***Please do not add the `c:\MinGW` etc to path***.
The toolchain discussed here would intelligently include stuff needed to work with tools inside `MinGW` / `Msys`.

Links:
 * https://sourceforge.net/projects/mingw/files/Installer/
 * https://sourceforge.net/projects/mingw/files/latest/download?source=files
 * https://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download
 
#### **In the MinGW setup**

Make sure to select the following choices to install:

![Image of options to install in MinGW](
https://github.com/boseji/BookOfGists/raw/master/images/ESP8266-unoff-sdk-win-3.png)

Next 

![Image of Install Depencency location MinGW Installer](
https://github.com/boseji/BookOfGists/raw/master/images/ESP8266-unoff-sdk-win-4.png)

**NOTE :** *In my case I had already installed the MinGW tools so the `Apply Changes` 
is not highlighted. In a fresh installation this must get enabled*

Just press the `Apply Changes` begin the installation.

### \#3 [Eclipse IDE](https://www.eclipse.org/downloads/packages/)

Choose the respective Windows architectures X86 = 32-bit X64 = 64-bit for

![CDT Icon](https://www.eclipse.org/downloads/images/cdt.png)
**Eclipse IDE for C/C++ Developers**

Links:
 * https://www.eclipse.org/downloads/packages/
 * Windows 32-bit : 
 http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/photon/R/eclipse-cpp-photon-R-win32.zip
 * Windows 64-bit:
 http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/photon/R/eclipse-cpp-photon-R-win32-x86_64.zip

Just choose which ever install comes latest do not worry about version of **Eclipse CDT** or flavor.

Make sure to setup the *Workspace* directory as descibed or as you desire while opening the Eclipse IDE for the first time.

### \#3.5 Missing JRE !!! **ERROR**

Yep, I too missed this Not to worry ...

You can [*Search*](https://www.google.com/search?q=java+jre) 
or directly go to [**Oracle JAVA SE runtime**](
http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)

Check the `Accept License Agreement` and then click on correct version of Windows architecture.

Make sure this is alrgned with your **Eclipse** architechture that was downloaded earlier.

Links:
 * http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html
 * Windows 32-bit:
 http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jre-8u181-windows-i586.exe
 * Windows 64-bit:
 http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jre-8u181-windows-x64.exe


[Icon-image1]: https://github.com/boseji/BookOfGists/raw/master/images/ESP8266-unoff-sdk-win-2.png
  
