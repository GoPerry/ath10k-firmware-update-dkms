#Created by Perry to add support ath10k dkms  2017.1.4

KVERSION:= $(shell uname -r)
CONFIG_ATH10K = m
CONFIG_ATH10K_PCI = m
CONFIG_ATH10K_DEBUGFS = y
CONFIG_THERMAL=y
CONFIG_MAC80211_DEBUGFS=y
CONFIG_PM=y
CONFIG_ATH10K_DEBUG = y
CONFIG_ATH10K_BOARD_CUSTOM = y

EXTRA_CFLAGS += -DCONFIG_ATH10K_BOARD_CUSTOM
	
MDIR = kernel/drivers/net/wireless/ath/ath10k
MOD_NAME = ath10k_core
obj-$(CONFIG_ATH10K) += $(MOD_NAME).o
$(MOD_NAME)-objs += mac.o \
                 debug.o \
                 core.o \
                 htc.o \
                 htt.o \
                 htt_rx.o \
                 htt_tx.o \
                 txrx.o \
                 wmi.o \
                 wmi-tlv.o \
                 bmi.o \
                 hw.o \
                 p2p.o \
                 swap.o

ath10k_core-$(CONFIG_ATH10K_DEBUGFS) += spectral.o
ath10k_core-$(CONFIG_NL80211_TESTMODE) += testmode.o
ath10k_core-$(CONFIG_ATH10K_TRACING) += trace.o
ath10k_core-$(CONFIG_THERMAL) += thermal.o
ath10k_core-$(CONFIG_MAC80211_DEBUGFS) += debugfs_sta.o
ath10k_core-$(CONFIG_PM) += wow.o

obj-$(CONFIG_ATH10K_PCI) += ath10k_pci.o
ath10k_pci-y += pci.o \
                ce.o

ath10k_pci-$(CONFIG_ATH10K_AHB) += ahb.o


ccflags-y := -I$(src)/include
ccflags-y += -DDBG -DRT3298 -DRTBT_IFACE_PCI -DLINUX

MAKE = make
LINUX_SRC ?= /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)

all:
	$(MAKE) -C $(LINUX_SRC) M=$(PWD)  modules

clean:
	$(MAKE) -C $(LINUX_SRC) M=$(PWD) clean

install:
	$(MAKE) INSTALL_MOD_PATH=$(DESTDIR) INSTALL_MOD_DIR=$(MDIR) \
		-C $(LINUX_SRC) M=$(PWD) modules_install
	depmod -a
	#three lines commented for dkms arch linux package: it autoinstalls the module
	install -m 0755 -o root -g root  ath10k_pci.ko $(DESTDIR)/lib/modules/$(shell uname -r)/kernel/drivers/net/wireless/ath/ath10k/
	install -m 0755 -o root -g root  ath10k_core.ko $(DESTDIR)/lib/modules/$(shell uname -r)/kernel/drivers/net/wireless/ath/ath10k/

	
