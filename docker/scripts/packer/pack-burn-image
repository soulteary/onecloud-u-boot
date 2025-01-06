#!/bin/bash

echo "Generate eMMC burn image"

rm -rf pack
mkdir -p pack

cp build/ddr_init.bin pack/DDR.USB
cp build/u-boot-comp.bin pack/UBOOT_COMP.USB

cat <<EOF >pack/aml_sdc_burn.ini
[common]
erase_bootloader    = 1
erase_flash         = 0
reboot              = 0

;package will filled by sdacard burning tool
[burn_ex]
package     = aml_upgrade_package.img
;media       =
EOF

cat <<EOF >pack/platform.conf
Platform:0x0801
BinPara:0xd9010000
DDRLoad:0xd9000000
DDRRun:0xd9000030
Uboot_down:0x200000
Uboot_decomp:0xd9000030
EOF

cp build/u-boot.bin pack/bootloader.img
echo -n "sha1sum $(sha1sum pack/bootloader.img | awk '{print $1}')" >pack/bootloader.VERIFY

cp build/resource.img pack/resource.img
echo -n "sha1sum $(sha1sum pack/resource.img | awk '{print $1}')" >pack/resource.VERIFY

cat <<EOF >pack/commands.txt
USB:DDR:normal:DDR.USB
USB:UBOOT_COMP:normal:UBOOT_COMP.USB

ini:aml_sdc_burn:normal:aml_sdc_burn.ini
conf:platform:normal:platform.conf

PARTITION:bootloader:normal:bootloader.img
VERIFY:bootloader:normal:bootloader.VERIFY

PARTITION:resource:normal:resource.img
VERIFY:resource:normal:resource.VERIFY

# PARTITION:boot:normal:boot.img
# VERIFY:boot:normal:boot.VERIFY
# PARTITION:rootfs:normal:rootfs.img
# VERIFY:rootfs:normal:rootfs.VERIFY
EOF

AmlImg pack build/eMMC.burn.img pack/
