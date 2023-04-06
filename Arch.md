# Install Base

### step 0. USB flash? dd bs=4M if=\*.iso of=/dev/sdX && sync
    shell> timedatectl set-ntp true

### step 1. Disk 
    shell> cfdisk
		MBR方式选择dos
		GPT方式选择gdt
	shell> mkfs.* /dev/sd*
	shell> mkswap /dev/sd* && swapon /dev/sd*

### step 2. Mount
	shell> lsblk /dev/sd*
	shell> mount /dev/sd* /mnt
	shell> mkdir /mnt/data && mount /dev/sd* /mnt/data

### step 3. Install
	# pacman -Syy
	# pacstrap /mnt base base-devel

### step 4. shell> genfstab -U -p /mnt >> /mnt/etc/fstab

### step 5. shell> arch-chroot /mnt /bin/bash

### step 6. Time
    shell> ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
  
### step 7. Locale
	1) /etc/locale.gen
	_____________________
	en_US.UTF-8 UTF-8

	zh_CN.GB18030 GB18030
	zh_CN.GBK GBK
	zh_CN.UTF-8 UTF-8
	zh_CN GB2312
	_____________________

	2) shell> locale-gen

	
### step 8. shell> mkinitcpio -p linux

### step 9. shell> passwd root

### step 10. Install bootloader
	shell> pacman -S grub os-prober
	shell> grub-install --recheck /dev/sda
	shell> grub-mkconfig -o /boot/grub/grub.cfg

### step 11. Configure Network
	1) hostname
	shell> echo Arch > /etc/hostname

	2) dhcp
	shell> systemctl enable dhcpcd.service   # 开机自动连接

	因为默认开启了DHCP服务, 所以写好的DNS将会在下次重启时消失.
	这是因为dhcpd服务刷新了这个文件.
	只需配置/etc/dhcpcd.conf 在末尾加上一句nohook resolv.conf即可,
	dhcpd就不会刷新resolv.conf文件.

### step 12. umount 
	shell> exit
	shell> umount -R /mnt

### step 13. reboot


# Config 

### 0. pacman
	pacman -Syu	# 同步源, 并更新系统
	pacman -Sy	# 仅同步源 
	pacman -Su	# 更新系统
	pacman -Su --ignore foo	# 升级时不升级包foo 

	pacman -S  pkg	# 从本地数据库中得到pkg的信息, 下载安装pkg包 
	pacman -Sy pkg	# 和源同步后安装名为pkg的包 
	pacman -Sf pkg	# 强制安装包pkg 

	pacman -Ss pkg 	# 搜索有关pkg信息的包 
	pacman -Si pkg	# 从数据库中搜索包pkg的信息 
	pacman -Qi pkg	# 列出已安装的包pkg的详细信息 

	pacman -R   pkg	# 删除pkg包 
	pacman -Rc  pkg # 删除pkg包和依赖pkg的包 
	pacman -Rsc pkg	# 删除pkg包和pkg依赖的包 

	pacman -Sc	# 清理/var/cache/pacman/pkg目录下的旧包 
	pacman -Scc	# 清除所有下载的包和数据库 

	pacman -U  pkg	# 安装下载的abs包, 或新编译的pkg包
	pacman -Sd pkg  # 忽略依赖性问题, 安装包pkg 

### 2. Users
	1) shell> useradd -m -s /bin/bash artist
	   shell> passwd artist

	2) shell> pacman -S sudo

	3) sudo: /etc/sudoers
	    ## Allow root to run any commands anywhere
	    root	ALL=(ALL)	ALL
	    artist	ALL=(ALL)	NOPASSWD:ALL

	4) shell> userdel -r artist

# Install necessary

### 1. ssh
	shell> pacman -S openssh
	shell> systemctl start sshd
	shell> systemctl enable sshd.service # 开机启动
	
	/etc/ssh/sshd_config
    __________________________________
	PermitRootLogin yes	# 允许root登陆
    __________________________________

### 2. lrzsz
	shell> pacman -S lrzsz
	    sz: 将选定的文件发送(send)到本地机器
	    rz: 运行该命令会弹出一个文件选择窗口, 从本地选择文件上传到Linux服务器.

### 3. ncftp-client:
	1) tar jxvf ncftp-3.2.5-src.tar.bz2
	2) ./configure
	3) make
	4) make install
	5) Eg: 将本地local/目录内的所有文件和目录, 上传到FTP服务器的Ftp/目录内(如果不存在Ftp/目录则自动创建).
          /usr/local/ncftp/bin/ncftpput -u FTP帐号 -p FTP密码 -P FTP端口 -m -R servip Ftp/ local/*

### 4. kermit
    kermrc
	    set line /dev/ttyS0
	    set speed 9600
	    set carrier-watch off
	    set handshake none
	    set flow-control none
	    robust
	    set file type bin
	    set file name lit
	    set rec pack 1000
	    set send pack 1000
	    set window 5

### 5. COMPILE REDBOOT
	1) cp libtcl.so /usr/lib
	2) cp libstdc++-libc6.1-1.so.2 /usr/lib
	3) vi ~/.bash_profile
		export PATH=${PATH}:/home/src/bootloader/x86/i386-elf/bin
		export _POSIX2_VERSION=199209
