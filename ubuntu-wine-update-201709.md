# Upgrading from older 1.8 Version of Wine to Latest 2.0.x Versions in Ubuntu 17.04, 16.10, 16.04.3

## Installing

```shell
wget https://dl.winehq.org/wine-builds/Release.key
sudo apt-key add Release.key
sudo apt-add-repository 'https://dl.winehq.org/wine-builds/ubuntu/'
sudo apt update
sudo apt install --install-recommends -y winehq-stable
sudo apt autoclean
sudo apt autoremove
```

Check for the latest version:

`wine --version`

Should be **`wine-2.0....`**, at the time of writing this it was `wine-2.0.2`.

## Edit the Older DASH entries of Ubuntu

The dash entries are located in `~/.local/share/applications/wine` directory.

More specifically : `~/.local/share/applications/wine/Programs/<Installed Software>`

In this case we are looking at **Diptrace** so
 
`~/.local/share/applications/wine/Programs/DipTrace` 

is the directory we need to find the files to edit.

This directory may contain files like:

```shell
User@Device:~/.local/share/applications/wine/Programs/DipTrace$ ls -1
Component Editor.desktop
DipTrace Launcher.desktop
Pattern Editor.desktop
PCB Layout.desktop
Schematic.desktop
Tutorial.desktop
Uninstall DipTrace.desktop

```

We would take the example of `Schematic.desktop` file.

This might look something like (User might get replaced by your username).

```
[Desktop Entry]
Name=Schematic
Exec=env WINEPREFIX="/home/User/.wine" wine-stable C:\\\\windows\\\\command\\\\start.exe /Unix /home/User/.wine/dosdevices/c:/users/Public/Start\\ Menu/Programs/DipTrace/Schematic.lnk
Type=Application
StartupNotify=true
Path=/home/User/.wine/dosdevices/c:/Program Files (x86)/DipTrace
Icon=20D5_Schematic.0
```

This is the older way of calling up the program to run.

However in the latest version the command is only `wine` not `wine-stable`.

Thus we edit the file `Schematic.desktop` to:

```
[Desktop Entry]
Name=Schematic
Exec=env WINEPREFIX="/home/User/.wine" wine C:\\\\windows\\\\command\\\\start.exe /Unix /home/User/.wine/dosdevices/c:/users/Public/Start\\ Menu/Programs/DipTrace/Schematic.lnk
Type=Application
StartupNotify=true
Path=/home/User/.wine/dosdevices/c:/Program Files (x86)/DipTrace
Icon=20D5_Schematic.0
```

Change in the **third line** of the above ensures that we have the command.

