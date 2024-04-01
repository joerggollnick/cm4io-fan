KDIR ?= /lib/modules/${KERNELRELEASE}/build
PWD := $(shell pwd)

COMMON_HEADERS := $(shell uname -r | sed 's/-rpi-.*/-common-rpi/g')
DTS_INCLUDE := /usr/src/linux-headers-$(COMMON_HEADERS)/include

DTC_INCLUDE := $(KDIR)/include

dtbo-y  += cm4io-fan.dtbo

targets  += $(dtbo-y)
always-y += $(dtbo-y)
clean-files := $(dtbo-y)

all:
        cpp -nostdinc -I $(DTS_INCLUDE) -undef -x assembler-with-cpp cm4io-fan-overlay.dts cm4io-fan-overlay.dts.preprocessed
        dtc -@ -I dts -O dtb -o cm4io-fan.dtbo cm4io-fan-overlay.dts.preprocessed
        $(MAKE) -C $(KDIR) M=$(PWD) DTC_INCLUDE=$(DTC_INCLUDE)

clean:
        $(MAKE) -C $(KDIR) M=$(PWD) clean

install: $(modules_install)
modules_install:
        cp $(dtbo-y) /boot/overlays/
