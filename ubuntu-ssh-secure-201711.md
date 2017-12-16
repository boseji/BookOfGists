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
 2. Setup of Public/Private Key for SSH
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

## Generate the keys for SSH

Generating the keys is easy :

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Follow the rest from [Github Tutorial](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

### We would Recommend to have a Password for the SSH key.

Next we need to insert this key into the Authorized Keys.

In case you have used the default path for key generation you can do this:

```shell
cat $HOME/.ssh/id_rsa.pub >> .ssh/authorized_keys
```

This would add the currently generated keys to the Authorized Keys register for connection.

In case you have used a diffrent folder or filename for the key then you need to provide the 
**public key version of that name**.

For Example if we have created `/home/user/priv.key` then the public key would be available at `/home/user/priv.key.pub`

It is important that the particular user or the global list of the Authorized keys needs to be updated.
This would allow only a specific private key to be used.
In case the system has multiple ussers then perform the command as:

```shell
cat $HOME/.ssh/id_rsa.pub >> /etc/ssh/global_authorized_keys
```


## Configuration for Basic Security on sshd Server

We need to open the `sshd_config` file.

```shell
sudo nano -c /etc/ssh/sshd_config
```

Here are some of the **Important** Modifications:

```shell

...

# Changing the Default SSH Port helps to make a more secure - Its more like a no-brainer, but make sure to remeber.
Port 1221

...

# Better Logging
LogLevel VERBOSE

...

# Authentication: - Full Enabled

#LoginGraceTime 2m
#PermitRootLogin prohibit-password
PermitRootLogin no
#StrictModes yes
#MaxAuthTries 6
MaxAuthTries 3
#MaxSessions 10
MaxSessions 2
# Make sure to <UserName> is changed to the Respective users you wish to allow. The usernames separated by coma.
AllowUsers <UserName>
PubkeyAuthentication yes

...

AuthorizedKeysFile	.ssh/authorized_keys .ssh/authorized_keys2 /etc/ssh/global_authorized_keys

...

# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no

...

# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
ChallengeResponseAuthentication no

...

#AllowAgentForwarding yes
AllowAgentForwarding no
#AllowTcpForwarding yes
AllowTcpForwarding no
#GatewayPorts no
GatewayPorts no
PermitTunnel no
X11Forwarding yes
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
PrintMotd no
#PrintLastLog yes
#TCPKeepAlive yes
#UseLogin no
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

...

```

You can find a Full file from here [Insert the Links]()

*Once you have this setup ready you can restart the PC to restart the SSHD service.*

## Configuring Windows to Connect using the New Key

On windows the free program [**WinSCP**](https://winscp.net/eng/download.php) is what 
many people use to connect to the remote ssh server.
Also [**Putty**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 
utility along with [**WinSCP**](https://winscp.net/eng/download.php)
program helps to connect to linux SSH terminal.

In order to make both of them work, we need to convert our Private key to a **WinSCP/Putty** acceptable form.

For that we use the tool call `PuTTYgen` its used to convert the binary file to `id_rsa` to a usable `.ppk` file.

  * First copy the `id_rsa` file or where ever you have stored the **Private Key** to the Windows PC.
  
  **NOTE : This is extemely risky step. Make sure to take proper precuaution to securely transfer your private key**
  
  * Next open the `PuTTYgen` tool it would open a Window by the title **PuTTY Key Generator**. 
  We are going to use this to convert the keys.
  
  * Click on the *Load* button and select the `All Files (*.*)` Filter, since the `id_rsa`/Private Key file is not normally visible.
  Select the *Private Key file* and open it.
  
  * In case you have set password on the *Private Key File* it would now ask for the password. Enter the same password you set earlier.
  
  * Finally the Key would be read back into the tool. Check if the `Key Comment` filed says `imported-openssh-key`, this means that the key has been successfully imported.
  
  * Now click *Save private key* button and this would generate a `.ppk` format file that us used by both **PuTTY/WinSCP**
  
  * Save this new format Private Key in a safe location on the Windows PC. 
  This location would later be used when we confirm the **PuTTY/WinSCP** connection.

With the `.ppk` file in place you can delete the `id_rsa` binary version of the Private key from Windows PC.

## Setup Windows to Linux Connection over SSH

### PuTTY

In the **PuTTY** write the `Host Name (or IP address)` and the correct `Port` (configured earlier in the `sshd_config` file).

In the Tag `Connection>SSH>Auth` find the Field `Private key file for authentication:`.

`Browse` for the `.ppk` file we generated in the above steps and select ok.

This sets up the connection, now you can click on `Open` to start the SSH connection.

### WinSCP

In the **WinSCP** Connection dialog set the follow:

  * Protocol SFTP/SCP

  * Hostname and Port number (configured earlier in the `sshd_config` file)

  * Username that we selected in the `sshd_config` file

  * Finally click on the `Advanced` Button

  * This opens an `Advanced Site Settings` Dialog where you can select the Tag `SSH>Authentication`
  
  * Keep the `Allow agent forwarding` as *Unchecked*
  
  * In the `Private key file:` field click on the `...` button the browse for the `.ppk` file generated earlier.
  
  * Click `Ok` to confirm the settings.

  * Finally you can click `Login` to begin the session.

## A Piece of Disclaimer

Though we are talking of secure connection to Linux PC, but method discrbed above is still vulnurable.

One must use hardware key store or HSM modules like [YubiHSM](https://www.yubico.com/products/yubihsm/) for better security.

At any point if the Private key is compromised in the above process due to weak password, no-password or direct Hacking the whole network security to the Linux is compromised.

Hence its recommened to use this technique when using VMs or Older PCs on a Private network not connected to Internet in any way.

*WARNING: This free document / guide is for your convenience and its use is at your own risk. It is available as a reference only, and IS NOT INHERENTLY A SECURE WAY to connect to Linux. The author/providers cannot and do not guarantee the privacy of your data, its security and communication. There are potentially serious security issues with any computer connected to the Internet without the appropriate protection, ranging from viruses, worms and other programs that can damage the user’s computer both ways, to attacks on the computer by unauthorized or unwanted third parties. By following this guide, you acknowledge and knowingly accept the potentially serious risks of accessing your hardware unsecurely over network. It is recommended that users take steps to protect their own computer system, such as installing current anti-virus software and maintaining appropriate firewall protection. You acknowledge and agree that YOUR USE OF THIS DOCUMENT & ABOVE PROCESS IS SOLELY AT YOUR OWN RISK.*
