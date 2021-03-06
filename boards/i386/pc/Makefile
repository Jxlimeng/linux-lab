_BASE    = 1
ARCH     = x86
XARCH    = i386

# qemu-system-i386 -M pc -cpu ?
CPU     ?= qemu32
MEM     ?= 128M
#QEMU    ?= v0.10.6
SMP     ?= 4
LINUX   ?= v5.1

LINUX[LINUX_v2.6.11.12] = 8e63197f
GCC[LINUX_v2.6.11.12] = 4.4
GCC[LINUX_v2.6.12.6] = 4.4
GCC[LINUX_v2.6.21.5] = 4.4
GCC[LINUX_v2.6.24.7] = 4.8
GCC[LINUX_v2.6.34.9] = 4.8

# Configuration for v2.6.10
KERNEL_GIT[LINUX_v2.6.10] = https://gitee.com/tinylab/tglx-linux-history
KERNEL_SRC[LINUX_v2.6.10] = tglx-linux-history
GCC[LINUX_v2.6.10] = 4.4
# Workaround "Bad page state at prep_new_page (in process 'swapper', page c02d1b84)"
MEM[LINUX_v2.6.10] = 80M
# /dev/ram0 boot panic on SMP currently
SMP[LINUX_v2.6.10] = 1
# panic after poweroff while using e1000
NETDEV[LINUX_v2.6.10] = ne2k_pci

# Force change ARCH for kernel version <= 2.6.23
IS_X86 = $(shell cd $(KERNEL_ABS_SRC); git show $(call _v,LINUX,LINUX):arch/$(ARCH)/boot >/dev/null 2>&1; echo $$?)
ifneq ($(IS_X86),0)
  ARCH  := $(XARCH)
endif

BUILDROOT?= 2019.02.2

# Check qemu support:
# $ qemu-system-i386 -net nic,model=?
#   qemu: Supported NIC models: ne2k_pci,i82551,i82557b,i82559er,rtl8139,e1000,pcnet,virtio
#
NETDEV_LIST := ne2k_pci e1000 rtl8139 pcnet
NETDEV  ?= e1000

ROOTDEV_LIST := /dev/hda /dev/ram0 /dev/nfs
ROOTDEV_LIST[LINUX_v2.6.34.9] := /dev/sda /dev/ram0 /dev/nfs
ROOTDEV ?= /dev/ram0

SERIAL  ?= ttyS0
ORIIMG  ?= arch/$(ARCH)/boot/bzImage
#CCPRE   = i686-pc-linux-gnu-
KIMAGE  ?= $(BSP_KERNEL)/$(LINUX)/bzImage
ROOTFS  ?= $(BSP_ROOT)/$(BUILDROOT)/rootfs.cpio.gz
HROOTFS ?= $(BSP_ROOT)/$(BUILDROOT)/rootfs.ext2
# Fixup: MP-BIOS bug: 8254 timer not connected to IO-APIC
# No such issue in newer kernels??
XKCLI   ?= noapic
#
# -m32 must be added to CFLAGS and AFLAGS in linux-stable/arch/i386/Makefile for kernel version <= 2.6.23
# see boards/i386/pc/bsp/patch/linux/v2.6.12/0001-v2.6.12-i386-force-using-m32-for-x86_64-host.patch
