default: build

build:
	echo fetch, flashkernel, or qemu

qemu: flashkernel
	qemu-system-x86_64 -kernel flashkernel -serial stdio -monitor /dev/null

flashkernel: uroot bzImage
	cp linux/arch/x86/boot/bzImage flashkernel

uroot:
	go run github.com/u-root/u-root -build=bb all   
	cp /tmp/initramfs.linux_amd64.cpio .
	lzma -k -f initramfs.linux_amd64.cpio
	ls -l initramfs*
	cp *lzma linux

bzImage:
	cp config-4.14.111 linux/.config
	(cd linux && make oldconfig && make -j32)

fetch: getkernel geturoot 
getkernel:
	rm -rf linux
	wget -O kernel.xz https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.14.111.tar.xz
	xzcat kernel.xz | tar x
	mv linux-4.14.111 linux
geturoot:
	go get -u github.com/u-root/u-root

