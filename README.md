# Multi-boot
Create a multi-boot usb drive using syslinux
###Requirements
syslinux, memdisk, mcopy, mkfs.vfat
###Install
1. Partition drive big enough for all the iso's and format to fat32
2. Install syslinux


	```
	syslinux /dev/sdx1 

	mkdir /media/usb/syslinux

	vim /media/usb/syslinux/syslinux.cfg
	```

3. Edit syslinux.cfg w/ ISO Files wanted
	```
	# Config file for Syslinux -
	# Default entry
	DEFAULT hdd
	PROMPT 0        # Change to 1 if you do not want to use a menu
	TIMEOUT 0       # 0 = disabled, value in 1/10 seconds
	 
	# Menu Configuration
	# Either menu.c32 or vesamenu32.c32 must be copied to /boot/syslinux 
	UI menu.c32
	#UI vesamenu.c32
	 
	# Refer to http://syslinux.zytor.com/wiki/index.php/Doc/menu
	MENU TITLE Rescue Stick
	MENU COLOR border       30;44   #40ffffff #a0000000 std
	MENU COLOR title        1;36;44 #9033ccff #a0000000 std
	MENU COLOR sel          7;37;40 #e0ffffff #20ffffff all
	MENU COLOR unsel        37;44   #50ffffff #a0000000 std
	MENU COLOR help         37;40   #c0ffffff #a0000000 std
	MENU COLOR timeout_msg  37;40   #80ffffff #00000000 std
	MENU COLOR timeout      1;37;40 #c0ffffff #00000000 std
	MENU COLOR msg07        37;40   #90ffffff #a0000000 std
	MENU COLOR tabmsg       31;40   #30ffffff #00000000 std
	 
	# boot tiny-core image
	LABEL tinycore
		MENU LABEL Tiny-Core
		LINUX memdisk
		INITRD ../tinycore-current.iso
		APPEND iso
	# boot Arch Linux image
	LABEL arch
		MENU LABEL Arch Linux
		LINUX memdisk
		INITRD ../archlinux-2010.05-core-dual.iso
		APPEND iso
	# boot GRML 32-Bit image
	LABEL grml32
		MENU LABEL Grml32
		LINUX memdisk
		INITRD ../grml_2011.05.iso
		APPEND iso
	# boot GRML 64-Bit image
	LABEL grml64
		MENU LABEL Grml64
		LINUX memdisk
		INITRD ../grml64_2011.05.iso
		APPEND iso
	# boot memtest
	LABEL memtest
	        MENU LABEL Memtest
		LINUX ../memtest
	# boot Hardware Detection Tool
	LABEL hdt
	        MENU LABEL HDT (Hardware Detection Tool)
	        COM32 hdt.c32
	# reboot
	LABEL reboot
	        MENU LABEL Reboot
	        COM32 reboot.c32
	# Power off system
	LABEL off
	        MENU LABEL Power Off
	        COMBOOT poweroff.com
	```
4. Copy Over Necessary Binaries
	```
	sudo cp /usr/lib/syslinux/{hdt.c32,menu.c32,reboot.c32,poweroff.com,memdisk} /media/usb/syslinux
	```
5. Copy Over All ISO's to Mountpoint

6. Install mbr
	```
	dd if=/usr/share/syslinux/mbr.bin of=/dev/sdx
	```

