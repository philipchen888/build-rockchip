export OUT=$(PWD)/out
export BOOT=$(OUT)/boot.img
export ARCH=arm64
export MYCHIP := rockpi4b
export CROSS_COMPILE=/usr/bin/aarch64-linux-gnu-

kernel: buildkernel

clean:
	cd linux; \
	sudo make mrproper

buildkernel:
	cp ./patches/$(MYCHIP)_40_custom_uuid ./patches/40_custom_uuid;
	cd linux; \
	sudo \rm -rf tmp; mkdir tmp; mkdir -p tmp/lib; \
	sudo make mrproper; \
	make defconfig; \
	make all -j8; \
	sudo make ARCH=arm64 INSTALL_MOD_PATH=./tmp modules_install; \
	sudo make ARCH=arm64 INSTALL_DTBS_PATH=./tmp/boot/dtb dtbs_install; \
	sudo make ARCH=arm64 INSTALL_PATH=./tmp/boot install

postbuildkernel:
	\rm -rf out; mkdir out;
	cd linux; \
	\rm -f $(BOOT); \
	mkfs.vfat -n "boot" -S 512 -C ${BOOT} 102400; \
	mmd -i ${BOOT} ::/extlinux; \
	mcopy -i ${BOOT} -s ../rk3328.conf ::/extlinux/extlinux.conf; \
	mcopy -i ${BOOT} -s ./arch/arm64/boot/Image ::; \
	mcopy -i ${BOOT} -s ./arch/arm64/boot/dts/rockchip/rk3328-roc-cc.dtb ::
