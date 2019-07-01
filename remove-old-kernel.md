# Removing old kernels
## List all kernels
dpkg -l | tail -n +6 | grep -E 'linux-image-[0-9]+' | grep -Fv $(uname -r)

This command list all kernels excluding current one.

## Removing kernel
sudo dpkg --purge PACKAGE

Taken from http://ubuntuhandbook.org/index.php/2016/05/remove-old-kernels-ubuntu-16-04/
