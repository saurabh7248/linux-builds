# linux-builds
This repository is used to put up debian builds of linux kernels along with commands, config and pitfalls.

## Download kernel
1. Make a folder(do not skip, you lazy ass ;) )
2. Get kernel source code  
```
git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git --single-branch --depth=1 --branch=v<major-version>.<minor-version>
```
4. Install dependencies using 
```
sudo apt install git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache -y
```
5. Download patch for patch build using
```
wget -c https://cdn.kernel.org/pub/linux/kernel/v6.x/patch-<major-version>.<minor-version>.<patch>.xz
```
6. Extract patch using
```
unxz patch-<major-version>.<minor-version>.<patch>.xz
```
5. cd into extracted directory and copy .config  using cp /boot/config-`uname -r` .config  or use config in this repo
6. Patch patch changes
```
git apply ../patch-<major-version>.<minor-version>.<patch>
```
7. Also patch deb creation changes for uefi-signing of vmlinuz and config fiel copying for dkms module signing.
```
git apply deb-creation.patch
```
8. Accept all new configs with following commands
```
yes '' | make oldconfig
```
9. If you want to edit the config file use the following command
```
make menuconfig
```
10. Copy the signing certificate in certs. And edit CONFIG_SIG_KEY, SYSTEM_TRUSTED_KEYS and SYSTEM_REVOCATION_KEYS, Also configure DEBUG_INFO configuration to disble debug info if not needed.
11. Clean the linux chekout using
```
make clean
```
12. Trigger linux kernel build with :
```
make -j$(( $(nproc) + 1)) bindeb-pkg LOCALVERSION=-custom
```
13. 3 deb files( outside of source code directory) will be created( 4 if dbg is created ).Follow [this](https://lists.kernelnewbies.org/pipermail/kernelnewbies/2016-September/016985.html) to create dbg deb.
14. To update the grub options to pick newer kernel
    ```
    sudo update-grub2
    ```
