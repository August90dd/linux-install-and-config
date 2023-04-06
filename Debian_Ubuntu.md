## 修改源后执行
    apt update
    apt upgrade

## 清空不必要的安装缓存
    apt autoclean
    apt autoremove

## Install necessary
    apt install open-vm-tools            自动适应窗口大小
    apt install open-vm-tools-desktop    虚拟机与主机之间复制粘贴

    apt install openssh-server
    apt install git    

    apt install manpages-de manpages-de-dev manpages-dev glibc-doc manpages-posix manpages-posix-dev

## 编译内核模块所需
    apt install linux-headers-$(uname -r)

## 编译内核所需
    apt install libncurses-dev
    apt install libssl-dev

    apt install cgroup-tools

## 删除旧内核
    dpkg --get-selections | grep linux-
    apt purge linux-image-
    apt purge linux-modules-
    apt purge linux-headers-

## 显示grub菜单
    /etc/default/grub
    GRUB_TIMEOUT=5
    #GRUB_TIMEOUT_STYLE=hidden

    update-grub
