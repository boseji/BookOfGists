# Reading QR-Code from Command Line via `zbarimg`

Many times we need a reliable way to infer the data hidden into the Barcode.

In the modern days is the time for QR-Code.

There is a easy to use utility provided by [**Zbar**](http://zbar.sourceforge.net/) folks.

**Zbar** is an opensource project which provides an easy way to **read** or **create** barcodes.

http://zbar.sourceforge.net/

They support EAN-13/UPC-A, UPC-E, EAN-8, Code 128, Code 39, Interleaved 2 of 5 and QR Code.

We would be interested in the *QR-Code*.

The Idea is the we want to scan a ***QR Code*** as a picture 
and then let **Zbar** figure out what is contained in that picture.


## 1. Just install the tools

```shell
sudo apt-get install zbar-tools
```

Or for Windows they have an Installation package:

http://sourceforge.net/projects/zbar/files/zbar/0.10/zbar-0.10-setup.exe/download

For better insight refer to 

http://zbar.sourceforge.net/download.html

and 

https://launchpad.net/ubuntu/xenial/+package/zbar-tools

## 2. Command-line Use

```shell
zbarimg QR-code.png
```

The data would be displayed instantly in the commandline.
