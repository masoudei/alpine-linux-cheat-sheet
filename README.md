# Alpine Linux useful commands & terminal cheat sheet

![Alpine Linux useful commands & terminal cheat sheet](/assets/alpine.jpg)

Minimal set of useful commands for Alpine Linux

## Installing a package

To install packages on Alpine Linux, use the syntax:

```sh
apk add package_name
```

For example, to install the nano text editor, run the command:

```sh
apk add nano
```

Additionally, you can install multiple packages in a single command using the syntax:

```sh
apk add package1 package2
```

For example, the command below installs neofetch and vim editor at a go.

```sh
apk add neofetch vim
```

You can confirm if you installed neofetch by running the command:

```sh
neofetch
```

This populates information about the operating system such as OS type, kernel, uptime, and underlying hardware such as CPU and memory.

![This populates information about the operating system such as OS type, kernel, uptime, and underlying hardware such as CPU and memory.](/assets/Neofetch-Show-Linux-Information.png)

To confirm that vim editor is installed, simply run the vim command without any arguments and this will display information about vim.

```sh
vim
```

![vim editor](/assets/Vim-Editor-Info.png)

or

```sh
nano
```

## How to install a specific package version in Alpine

You can set "sticky" versions like this:

```sh
# Both are equal
apk add packagename=1.2.3-suffix
apk add 'packagename<1.2.3-suffix'
```

That will upgrade packages only until the specified version. You can then safely use …

```sh
apk upgrade
```

to upgrade all packages, while packages with versions will remain with their version. To set a minimum version just use …

```sh
apk add "packagename>1.2.3-suffix"
```

In case you can't find a package, while you can see it in the UI for Alpine packages, update your sources/package database:

```sh
apk update
```

The package repository can be found here:

<https://pkgs.alpinelinux.org/packages>
Never pin packages from the "edge" branch of the alpine package repo, as these are in test and may be revoked. (At pkgs.alpinelinux.org/packages, click "edge" and change it to the alpine image version you use, and click "search" again.)

## Uninstalling a package

```sh
apk del neovim
```

## List All Installed Packages

To list all installed packages on Alpine Linux, run the command:

```sh
apk info
```

![List All Installed Packages](/assets/List-All-Installed-Packages-in-Alpine-Linux.png)

## Update Alpine Linux

To update the repositories and package lists on Alpine Linux, run the command

```sh
apk update
```

## Search for an Availability of Packages

Before installing packages, it’s worthwhile to check if the packages have been officially been hosted in the repositories. To do so, use the syntax:

```sh
apk search package_name   
```

For example, to search for a nano package in the repositories, run the command:

```sh
apk search nano
```

## Get a Description of an Installed Package

To get a description of a package in the repositories, about the package pass the -v and -d flags as shown. The option -d is short for description whilst the -v option prints out verbose output.

```sh
apk search -v -d nano
```

## Upgrade Alpine Linux

To upgrade all the packages on Alpine Linux to their latest versions, run the command

```sh
apk upgrade
```

![Upgrade Alpine Linux](/assets/Upgrade-Alpine-Linux.png)

The -a option allows downgrading packages to what's available in the repository. It is generally recommended to always use this option, especially on stable releases, to ensure that any package reverts make it to your device.

```sh
apk upgrade -a
```

To perform a dry run of the upgrade, pass the -s option. This merely runs a simulation and shows the versions that the packages will be upgraded to. It does not upgrade the packages.

```sh
apk -s upgrade
```

![perform a dry run of the upgrade](/assets/Dry-Run-Upgrade-Alpine-Linux.png)

## Hold a Package Upgrade

There are instances where you may want to keep a few packages back from an upgrade. For instance to keep nano in its current version – nano-5.9-r0 – run the command.

```sh
apk add nano=5.9-r0 
```

![Hold Package Upgrade in Alpine Linux](/assets/Hold-Package-Upgrade-Alpine-Linux.png)

This will exempt the nano package from the upgrade as other packages are upgraded to their latest versions.

To later release the package for the upgrade, run:

```sh
apk add 'nano>5.9'
```

## Run various package repair strategies

e.g. running failed installation scripts again:

```sh
apk fix
apk fix name-of-package  # will also reinstall name-of-package
```

## Listing installed package versions

(add | grep package-name to filter for a specific package):

```sh
apk info -vv
```

## Installing build dependencies

```sh
sudo apk add build-base install-makedepends
install-makedepends neovim
```

The repositories are stored in /etc/apk/repositories as one repository url per line. There's also /etc/apk/world which is a list of packages that are explicitly installed. It's possible to add/remove packages from this list and then running apk fix to apply those changes. The Alpine Linux wiki has a page comparing apk to other popular distros, with many helpful examples.

New installations of postmarketOS let apk ask for confirmation by default. If you don't want this, delete /etc/apk/interactive (pmaports!3444).

## Service management

The service manager in postmarketOS is OpenRC. The basic service management is done with the familiar rc-service command.

Note Tip: rc-service is also symlinked to service which is shorter to type.

```sh
rc-service networkmanager status
rc-service networkmanager start
rc-service networkmanager stop
rc-service networkmanager restart
```

To enable or disable services on boot you use the rc-update command.

```sh
List the services that are added to a runlevel
$ rc-update
             bootmisc | boot                                   
              chronyd |      default                           
                 dbus |      default                           
                devfs |                                 sysinit
                dmesg |                                 sysinit
                  gdm |      default                           
              haveged |      default                           
             hostname | boot                                   
              hwclock | boot                                   
            killprocs |                        shutdown        


Start NetworkManager on boot (in the default runlevel)
$ rc-update add networkmanager default

Stop NetworkManager starting on boot
$ rc-update del networkmanager default
```

## Allow sshd login with password

How to enable SSH root login on Alpine Linux

On Alpine Linux, root SSH access using passwords is disabled by default. The following tutorial shows you how to enable password-based root login via SSH when using openssh. (I have not tested whether root access is enabled when installing Alpine Linux using dropbear instead of openssh)

First, open the SSH config file using

```sh
vi /etc/ssh/sshd_config
```

or

```sh
nano /etc/ssh/sshd_config
```

Press I in order to activate vi editing mode.

Remove the # at the beginning of the line and change prohibit-password to yes:

Now look for this line and uncomment:

```sh
#PasswordAuthentication yes
```

Now save and exit by pressing Esc and then pressing :wq and Enter.

After that, restart openssh using

```sh
service sshd restart
```

or

```sh
rc-service sshd restart
```

Now you can login as root using the password.

## Setup the Repositories 📗

Update you repositories under /etc/apk/repositories to include community, edge community and testing.

```file
#/media/cdrom/apks
http://dl-cdn.alpinelinux.org/alpine/v3.17/main
http://dl-cdn.alpinelinux.org/alpine/v3.17/community
#http://dl-cdn.alpinelinux.org/alpine/edge/main
http://dl-cdn.alpinelinux.org/alpine/edge/community
http://dl-cdn.alpinelinux.org/alpine/edge/testing
```

## Install Sudo

By default, Alpine Linux does not have the `sudo` package installed, as it uses `doas` instead. You can also change user to root by executing :

```sh
su
```

or

```sh
su root
```

or

```sh
doas -s
```

by the way if you still want to have `sudo` package just install this package by :

```sh
apk add sudo
```

## Add User

You can add user to Apline Linux by running :

```sh
adduser [username]
```

Example :

```sh
adduser mike
```

then enter desired password and confirmation.

also you can add user to root group that is called wheel in alpine

```sh
adduser [username] [group]
```

Example :

```sh
adduser mike wheel
```

## Alpine Linux setup-proxy

Remove proxy from alpine linux bash profile

```sh
cd /etc/profile.d/
nano proxy.sh 
```

Comment proxy Env Vars or remove them :

```file
# this file was generated with and might get overwritten by setup-proxy

#export http_proxy=http://192.168.1.100:10811
#export https_proxy=http://192.168.1.100:10811
#export ftp_proxy=http://192.168.1.100:10811
export no_proxy=localhost
```
