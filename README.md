# V1.34 rocminstall.py Utility to install ROCm releases. Supports Ubuntu/Debian, CentOS/RHEL 7/8, SLES15 installation
#### NOTE: Install dkms, kernel headers, gcc packages on OS BEFORE installing ROCm Kernel

## Section CentOS Pre-Install Steps
**CentOS 7/8 Preparing System for ROCm Kernel:**
```
  sudo yum clean all
  sudo yum update (this would update kernel to latest. Do this before installing kernel-headers.)
  sudo yum install kernel-headers  (OR sudo yum install kernel-headers-`uname -r` )
  sudo yum install gcc
  sudo yum install gcc-c++
  sudo yum install -y epel-release
  sudo yum install dkms
  sudo yum install kernel-devel (OR sudo yum install kernel-devel-`uname -r` )
  sudo yum install python3
  sudo reboot (for above updates to take effect)
```
#### To Update ROCm Kernel (rock-dkms rock-dkms-firmware) Packages:
**CentOS/RHEL rpm Commands to UNINSTALL rock-dkms rock-dkms-firmware packages FIRST before install**
```
  sudo rpm -evh rock-dkms rock-dkms-firmware (OR sudo rpm -evh --nodeps rock-dkms rock-dkms-firmware)
  sudo reboot
```

## Section: Ubuntu Pre-Install Steps
**Ubuntu 18 HWE Preparing System for ROCm Kernel**
```
   sudo apt-get install --install-recommends linux-generic-hwe-18.04  (Do this before installing linux-headers.)
   sudo reboot
   sudo apt install dkms
   sudo apt install gcc
   sudo apt install g++
   sudo apt upgrade (this may be needed for kernel updates to correspond with headers, etc.)
   sudo reboot (for above upgrade to take effect)
   sudo apt install linux-headers ( OR sudo apt install linux-headers-`uname -r` )
   sudo apt install linux-tools ( OR sudo apt install linux-tools-`uname -r` )
   sudo reboot
```
#### To Update ROCm Kernel (rock-dkms rock-dkms-firmware) Packages:
**Ubuntu dpkg Commands to UNINSTALL ROCm rock-dkms rock-dkms-firmware packages FIRST before install**
```
  sudo dpkg -r --force-all rock-dkms rock-dkms-firmware
  sudo dpkg --purge --force-all rock-dkms rock-dkms-firmware
  sudo reboot
```
#### NOTE: On SLES15, the script uses zypper and requires user interaction

## Section: Install ROCm
#### Download Python3 rocminstall.py script:
***wget -O rocminstall.py --no-check-certificate https://raw.githubusercontent.com/amddcgpuce/rocminstaller/master/rocminstall.py***

```
Example: Install ROCm 4.0, including kernel components (assumes dkms, kernel header, gcc preinstalled)
  wget -O rocminstall.py --no-check-certificate https://raw.githubusercontent.com/amddcgpuce/rocminstaller/master/rocminstall.py

Example: Install ROCm, excluding pre-built miopenkernels (faster install)
  sudo python3 ./rocminstall.py --rev 4.0 --nomiopenkernels

Example: Install ROCm including pre-built miopenkernels
  sudo python3 ./rocminstall.py --rev 4.0

Example: Install ROCm 4.0 development packages in container (no kernel components)
  sudo python3 ./rocminstall.py --rev 4.0 --nokernel

```
#### Usage
```
usage: rocminstall.py [-h] [--rev REVSTRING] [--destdir DESTDIR] [--list]
                      [--repourl REPOURL] [--baseurl BASEURL] [--nokernel]
                      [--justkernel] [--justrdc] [--nomiopenkernels]

[V1.34]rocminstall.py: utility to download and install ROCm packages for
specified rev (dkms, kernel headers must be installed, requires sudo
privilege)

optional arguments:
  -h, --help         show this help message and exit
  --rev REVSTRING    specifies ROCm release repo to use as in
                     http://repo.radeon.com/rocm/{apt, yum, zyp,
                     centos8}/<REV> Example: --rev 3.5 for ROCm 3.5 repo
                     http://repo.radeon.com/rocm/{apt, yum, zyp, centos8}/3.5,
                     or --rev 3.3 for ROCm 3.3 repo
                     http://repo.radeon.com/rocm/{apt, yum, zyp}/3.3
  --destdir DESTDIR  specify directory where to download RPM before
                     installation. Default: current directory --destdir /tmp
                     to use /tmp directory
  --list             just list the packages that will be installed -- do not
                     download or install
  --repourl REPOURL  specify ROCm repo URL to use from where to download
                     packages Example: --repourl http://compute-
                     artifactory/build/xyz
  --baseurl BASEURL  specify early access ROCm repo URL to use from where to
                     download packages Example: --baseurl
                     http://repo.radeon.com/rocm/private/apt_3.6-priv/
  --nokernel         do not install rock kernel packages, for example, used to
                     install ROCm in docker
  --justkernel       ONLY install rock kernel packages of specified version -
                     undefined behavior if --nokernel also specified
  --justrdc          ONLY install ROCm Radeon Data Center Monitor tool -
                     attempts to install rdcX.Y.Z package corresponding to rev
  --nomiopenkernels  do not install pre-built miopenkernels packages - saves
                     space and installation time

```

# ROCm Uninstall Utility
## NOTE: Does not uninstall ROCm Kernel packages (rock-dkms rock-dkms-firmware) 
#### To uninstall ROCm kernel
**CentOS/RHEL rpm Commands to UNINSTALL rock-dkms rock-dkms-firmware packages ONLY**
```
  sudo rpm -evh rock-dkms rock-dkms-firmware (OR sudo rpm -evh --nodeps rock-dkms rock-dkms-firmware)
  sudo reboot
```
**Ubuntu dpkg Commands to UNINSTALL rock-dkms rock-dkms-firmware packages ONLY**
```
  sudo dpkg -r --force-all rock-dkms rock-dkms-firmware
  sudo dpkg --purge --force-all rock-dkms rock-dkms-firmware
  sudo reboot
```

### Steps to Run rocmuninstall.sh Script
#### Download the rocmuninstall.sh shell script:
***wget -O rocmuninstall.sh --no-check-certificate https://raw.githubusercontent.com/amddcgpuce/rocminstaller/master/rocmuninstall.sh***
#### NOTE: User will be prompted with list of selected packages to confirm uninstallation.

```
Examples
# Download the rocmuninstall.sh script
 wget -O rocmuninstall.sh --no-check-certificate https://raw.githubusercontent.com/amddcgpuce/rocminstaller/master/rocmuninstall.sh

 Example: To uninstall ROCm 4.1 packages use (requires sudo):
    sudo sh ./rocmuninstall.sh 4.1.0 

 Example: To uninstall all ROCm packages use (requires sudo):
    sudo sh ./rocmuninstall.sh all 
```
#### Usage of rocmuninstall.sh script
```
=== ROCm Uninstall Utility V1.0 ===
Mon May  3 15:39:50 PDT 2021
Usage: sudo sh ./rocmuninstall.sh <X.Y.Z>|all, where <X.Y.Z> is the ROCm version to uninstall
 To uninstall all ROCm packages (except kernel) use 'all' option
 Example: To uninstall ROCm 4.1 packages use:
    sudo sh ./rocmuninstall.sh 4.1.0 
 Example: To uninstall all ROCm packages use:
    sudo sh ./rocmuninstall.sh all 
 User will be prompted with list of selected packages to confirm uninstallation.
```

