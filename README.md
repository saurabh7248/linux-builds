# linux-builds
This repository is used to put up debian builds of linux kernels along with commands, config and pitfalls.

## Download kernel
1. Make a folder(do not skip, you lazy ass ;) )
2. Download tarball from https://www.kernel.org/ using wget in the created directory
3. Extract tarball using tar -xvf command
4. Install dependencies using sudo apt install git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache -y
5. cd into extracted directory and copy .config  using cp /boot/config-`uname -r` .config  or use config in this repo
6. yes '' | make oldconfig
7. make menuconfig if you want to edit the config file
8. make clean
9. NUM_THREADS=16 # this can be number of logical processors
10. make -j $NUM_THREADS deb-pkg LOCALVERSION=-custom # this makes a deb package and the file in the repo skips dbg deb file,custom is the nam you want to  give to the kernel
11. 3 deb files( outside of extracted directory) will be created( 4 if dbg is created )(follow this to create dbg https://lists.kernelnewbies.org/pipermail/kernelnewbies/2016-September/016985.html)
12. Install them in the order of firmware,libc,headers and image (and image-dbg).
13. sudo update grub
14. After install if secure exception is given see secure boot options and set to other os instead of windows.
