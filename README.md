# ThingsCore-1 Yocto meta-st Layer
In this repository, the yocto layer for the DreamXLabs ThingsCore-1 SOM is shared.
It is based on the official OpenSTLinux BSP provided by STMicroelectronics and it extends
functionality of the **image-core-minimal** recipe.

# Details
The meta-st-thingcore layer generates the fip.bin file implementing the **TF-A->OPTEE->TF-A->UBoot->Linux**
boot chain by using the device tree sources under /mx and building the patched sources by ST. Also, Uboot and Linux kernel
recipes are added and used to modify sources using artifact files.

Two are the main recipies:
- **thingscore-image-full**: package-rich file system used to be installed on the eMMC rootFSA/B partition
- **thingscore-image-recovery**: compressed root file system for initramfs kernel installed on NAND flassh with minimal packages and linux tools to recover the main eMMC root file system partition.

Packages also include file system overlay recipes that install the necessary kernel modules and firmware/config files for the WF200 tranceiver. 

The end-user is free to modify and add the necessary packages for the end-application. 

Package list:
```python
python3 python3-pip htop nano i2c-tools wireguard-tools iw wpa-supplicant 
iproute2 curl wget gzip tar util-linux-lsblk util-linux-blkid parted 
util-linux-fdisk e2fsprogs openssh 
```
  

# Cloning repositories

```bash
mkdir yocto
cd yocto
git clone https://github.com/patsaoglou/meta-st-thingcore.git
./git-repo/repo init -u https://github.com/STMicroelectronics/oe-manifest.git -b refs/tags/openstlinux-6.1-yocto-mickledore-mpu-v24.06.26
./git-repo/repo sync
```

# Set environment
```bash
MACHINE=thingscore-1-mx DISTRO=openstlinux-weston source layers/meta-st/scripts/envsetup.sh
```

# Build ThingCore full image 
```bash
bitbake thingscore-image-full
```

# Build ThingCore recovery image 
```bash
bitbake thingscore-image-recovery
```

# TODOs
- Add complete image generation for NAND and eMMC, structrured for OTA
- Secure images and secure boot