# Securing SSH on Ubuntu - Works for 16.04, 16.10, 17.04,17.10 - Tested

Some of us like myself do our primary hardware development on Windows PCs.
However when it come to writing a [`Go`](https://golang.org) server or a [`Rust`](https://www.rust-lang.org) program we head over to linux PC.

Most of the development work on a linux PC is via the terminal. Hence having a good remote terminal is essential.
As you can't always have a VM instances running your linux. It become difficult over time.

The best is to have either a headless PC or an old Laptop that would act as our Linux box.

In my case I had an old **Celeron Core 2 Duo** a.k.a `i386`.

Since the loss of `Windows XP` the laptop was running **Ubuntu 16.04**.

There were several tutotials on the Net telling about how to configure an SSH server
however every time I tried them some thing was wrong or some thing did not work.

Hence I decided to document my journey to have a good secure SSH server running on my old Laptop.

Fortunately the same process also worked on my Workstation running a storage server on **Ubuntu 17.10**.

Lets Begin !

## Steps that we are going to take

 1. Installation of SSH on Ubuntu
 2. Setup of Public/Private Key in Windows/Linux - With compatibility in mind
 3. Configuration for hardened SSH on Ubuntu
 4. Integration of Keys with Windows/Linux
 5. Setup for Connecting Windows to SSHD on Linux
 
## Install SSH on Ubuntu

The package `ssh` is same name used for a cluster of packages that would be installed by giving the command:

```shell
sudo apt install ssh
```

Here is a List of packages that would be installed : 

https://packages.ubuntu.com/search?keywords=ssh&searchon=names&suite=all&section=all

You can look at the individual distrubtions in the [link](https://packages.ubuntu.com/search?keywords=ssh&searchon=names&suite=all&section=all) above.

Typically there are few important ones to note:

 1. `openssh-client` = This gives all the importat features needed in linux to connect to other SSH servers and generate keys .etc.
 2. `openssh-server` = A.K.A **`SSHD`** The famous SSH server for Linux. This enables your linux box to become a server 
 and provide ssh terminal connection features from windows.

With these two installed its most likely that your **SSH Server** is already running.

### **WARNING** Your service is UNSAFE ! - Just after install

So first stop the server with the command:

```shell
sudo service ssh stop
```

To check if the SSH is running then type the following command:

```shell
ps -e|grep ssh
```

If this shows an empty list then you have successfuly turned off your unsafe ssh server.
