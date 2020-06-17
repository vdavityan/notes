# GNS3 Installation on Linux :octocat: 

## INSTALLATION FROM PACKAGES
Ubuntu-based distributions (64-bit only)
These instructions are for Ubuntu and all distributions based on it (like Linux Mint).

``` 
sudo add-apt-repository ppa:gns3/ppa
sudo apt update                                
sudo apt install gns3-gui gns3-server
```

(when prompted whether non-root users should be allowed to use wireshark and ubridge, select ‘Yes’ both times)

## If_you_want_IOU_support

``` 
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install gns3-iou
``` 

## To_install_Docker-CE_(Xenial_and_newer)

Remove any old versions:
``` 
sudo apt remove docker docker-engine docker.io
``` 
Install the following packages:
``` 
sudo apt-get install apt-transport-https ca-certificates curl \
software-properties-common
``` 
Import the official Docker GPG key:
``` 
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
``` 
Add the appropriate repo:
``` 
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"
``` 
Install Docker-CE:
``` 
sudo apt update
sudo apt install docker-ce
``` 
Finally, add your user to the following groups:
``` 
ubridge libvirt kvm wireshark docker 
``` 
(use “_**sudo usermod -aG group username**_” to add your user to each of those groups)

Restart your user session by logging out and back in, or restarting the system.

## Debian-based distributions (64-bit only)

### FOR DEBIAN JESSIE
Add the following lines to your /etc/apt/sources.list:
``` 
deb http://ppa.launchpad.net/gns3/ppa/ubuntu trusty main
deb-src http://ppa.launchpad.net/gns3/ppa/ubuntu trusty main

sudo apt-get update
sudo apt-get install -y gns3-gui gns3-server
``` 

### FOR DEBIAN STRETCH
Add the following lines to your /etc/apt/sources.list:
``` 
deb http://ppa.launchpad.net/gns3/ppa/ubuntu xenial main
deb-src http://ppa.launchpad.net/gns3/ppa/ubuntu xenial main
``` 
**The python libraries for this are broken, it will not work**

### FOR DEBIAN BUSTER
Refresh your metadata, and install the following packages:
``` 
sudo apt update
sudo apt install -y python3-pip python3-pyqt5 python3-pyqt5.qtsvg \
python3-pyqt5.qtwebsockets \
qemu qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst \
wireshark xtightvncviewer apt-transport-https \
ca-certificates curl gnupg2 software-properties-common
``` 
## Install GNS3 from Pypi:
``` 
pip3 install gns3-server
pip3 install gns3-gui
``` 
We’ll go ahead and install docker next.  Import the Docker GPG key:
``` 
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
``` 
(As of 10/10/2019, Buster requires using the “edge” repo)
``` 
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable edge"
``` 
Refresh your metadata, and install docker:
``` 
sudo apt update
sudo apt install -y docker-ce
``` 
Add the following lines to your /etc/apt/sources.list:
``` 
deb http://ppa.launchpad.net/gns3/ppa/ubuntu bionic main
deb-src http://ppa.launchpad.net/gns3/ppa/ubuntu bionic main 
``` 
Get the GPG key:
``` 
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F88F6D313016330404F710FC9A2FD067A2E3EF7B
``` 
Refresh your metadata, and ONLY install the following two packages!

``` 
sudo apt-get update
sudo apt install dynamips ubridge
``` 
To prevent accidentally installing anything else from that repo (for now), remove or comment out those two lines in your __*/etc/apt/sources.list*__ file:

``` 
#deb http://ppa.launchpad.net/gns3/ppa/ubuntu bionic main
#deb-src http://ppa.launchpad.net/gns3/ppa/ubuntu bionic main
``` 
You can also remove that GPG key, if desired:
``` 
sudo apt-key del F88F6D313016330404F710FC9A2FD067A2E3EF7B
``` 
Add your user to the following groups:
``` 
kvm libvirt docker ubridge wireshark
``` 
(use “_**sudo usermod -aG group your_user**_” to add your user to an existing group)

Restart your user session by logging out and back in, or rebooting the system.


(note: the reason we currently can’t just install all of the packages (except docker-ce) off launchpad, is due to a python issue)


If you have a pre-existing install of Buster, and run into the following error, follow these instructions (shout out to user Pierce Howell for providing them!)


**Fail update installation: No module named 'sip'
Can't import Qt modules: Qt and/or PyQt is probably not installed correctly…**

Start by uninstalling gns3 and gns3-server (Do not remove the ppa’s):
``` 
sudo apt purge --autoremove gns3-server gns3-gui
``` 
Create a symbolic link for python 3.5 using python3.7:
``` 
sudo ln -s /usr/bin/python3.7 /usr/bin/python3.5
``` 
Install python-pip and python3-pip to use the gns3 from source:
``` 
sudo apt install python-pip python3-pip
``` 
Install from PyPi as listed:
``` 
sudo pip3 install gns3-server
sudo pip3 install gns3-gui
``` 
Once installed, additional dependencies such as QtSvg, qtwebsockets, and dynamips will be required in order for the application to run.
``` 
sudo apt install python3-pyqt5.QtSvg python3-pyqt5.qtwebsockets dynamips
``` 

Finally, attempt to start gns3 from the command line. If you receive no output errors and the application does not start, try to reboot your machine.


**Note:** _You may not have to reboot your machine. The reboot worked in my case after much trial and error. You may also have to add the shortcut as it may not automatically populate in your applications menu.  _



## For Docker support (Jessie and Stretch)

Uninstall older versions:
``` 
sudo apt-get remove docker docker-engine docker.io
``` 
Install these dependencies, if not already present:
``` 
sudo apt-get install  apt-transport-https ca-certificates \
     curl gnupg2 software-properties-common telnet
``` 
Import the official Docker GPG key:
``` 
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
``` 
Add the appropriate repository:
``` 
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable"
``` 
Install docker:
``` 
sudo apt-get update
sudo apt-get install docker-ce
``` 
If any permissions errors are encountered, ensure your user belongs to the following groups:
``` 
ubridge docker wireshark kvm libvirt
``` 
If your user doesn’t belong to any of the above groups, use the following command to add it to them:

``` 
sudo usermod -aG group username
``` 
Log out and back in to restart your user session



## Arch Linux
Note that this package is maintained by a third-party.


Run the following commands in a terminal:
``` 
yaourt -S gns3-gui gns3-server
``` 
## Fedora
Note that this package is maintained by a third-party.


Starting with Fedora 24 packages are available in official repositories.
``` 
dnf install gns3-server gns3-gui wireshark wireshark-qt
``` 
**WARNING**
_21/12/16 the installation is broken: https://bugzilla.redhat.com/show_bug.cgi?id=1406711_



### For_QEMU/KVM_guests
``` 
dnf install qemu-kvm qemu-system-x86
``` 
For_Dynamips_emulation,_packages_are_still_under_review,_however_you_can_use_this_copr_repository
``` 
dnf copr enable athmane/gns3-extra
dnf install vpcs dynamips
``` 
### For_Docker_support


Uninstall previous versions:
``` 
sudo dnf remove docker \
                 docker-client \
                 docker-client-latest \
                 docker-common \
                 docker-latest \
                 docker-latest-logrotate \
                 docker-logrotate \
                 docker-selinux \
                 docker-engine-selinux \
                 docker-engine
``` 
Set up the repository -
``` 
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
   --add-repo \
   https://download.docker.com/linux/fedora/docker-ce.repo
```    
**NOTE** There currently is NOT a release in the stable channel, so you’ll need to use the edge channel for now:
``` 
sudo dnf config-manager --set-enabled docker-ce-edge
``` 
Install Docker -
``` 
sudo dnf install docker-ce
``` 
**NOTE** there currently appears to be an issue with docker containers from the GNS3 Marketplace (even after adding your user account to the docker group). Will update this doc accordingly.



### For IOU support with Fedora
``` 
https://gns3.com/discussions/use-iou-images-in-64-bit-linux
``` 


## Gentoo
Note that this package is maintained by a third-party.


Run the following commands in a terminal:
``` 
emerge --ask net-misc/gns3-gui
``` 


**NOTE**
Kernel support for KVM and virtio recommended (required for certain Qemu VMs)

## Sabayon
Note that this package is maintained by a third-party.


Run the following commands in a terminal:
``` 
equo i net-misc/gns3-gui net-misc/gns3-converter net-misc/gns3-server
``` 
**NOTE**  The available packages are very out-of-date.
Kernel support for KVM and virtio recommended (required for certain Qemu VMs)
Docker does not seem to work properly with the packages provided via Entropy.

## Alpine Linux
Note that this package is maintained by a third-party.


GNS3 is in testing repository of "edge" version.
``` 
apk update && apk add gns3-gui gns3-server
``` 
## OpenSuse 
Note that this package is maintained by a third-party.

``` 
http://software.opensuse.org/download.html?project=home%3ADremor%3Agns3&package=gns3-gui
``` 


## INSTALLATION FROM SOURCE
### Source archive
The GNS3 source archive can be downloaded from here


Qemu is not provided you need to install it by hand.

Installation from PyPi
PyPi is the python package index. A repository for pure python package.
``` 
pip3 install gns3-gui
pip3 install gns3-server
``` 
You will also need to install all the dependencies by hand (qemu, iouyap, ubridge, dynamips...).
