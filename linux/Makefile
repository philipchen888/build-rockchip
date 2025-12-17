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
