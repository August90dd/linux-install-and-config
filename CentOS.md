## Install necessary
    shell> yum install open-vm-tools.x86_64
    shell> yum install open-vm-tools-desktop.x86_64

    shell> yum install man-pages  
    shell> yum install vim  
    shell> yum install git   
    shell> yum install wget  
    shell> yum install gcc   
    shell> yum install gcc-c++   

    shell> yum install patch   
    shell> yum install bc   
    shell> yum install ncurses-devel   

    shell> yum install glibc-devel.i686   
    shell> yum install glibc-static.i686   
    shell> yum install libgcc.i686 (-m32 编译, 运行32位程序)   

    shell> yum install strace(跟踪系统调用)   
    shell> yum install ltrace(跟踪库函数调用)   

    shell> yum install libcgroup-tools(cgroup-tools)

    shell> yum install bash-completion(for git completion)

## Install kernel-devel
    shell> yum install kernel-devel 省去重新编译安装新内核

## Install systemstap
    shell> yum install systemtap systemtap-runtime
    正常运行仍然需要编译一个新内核并安装, 否则会报错:
    Missing separate debuginfos, use: debuginfo-install kernel-xxx

## Install qemu
    shell> git clone git://git.qemu-project.org/qemu.git

    shell> yum install zlib-devel   
    shell> yum install glib2-devel   
    shell> yum install pixman-devel   
    shell> yum install SDL   
    shell> yum install SDL-devel   
    shell> cd qemu; git submodule update --init dtc   

    shell> ./configure
    shell> make 
    shell> make install

## lspci
    shell> 1) yum whatprovides */lspci # 查找lspci是通过哪个安装包来提供的   
    shell> 2) yum install pciutils 

## pip(python包管理工具)
    shell> yum install python-pip
    shell> pip install --upgrade pip

## matplotlib(smem绘制饼柱图仍有问题)
    shell> yum install python-devel
    shell> pip install matplotlib
    
## Install Xfce Desktop
    shell> yum install epel-release    
    shell> yum groupinstall "X Window system"  
    shell> yum groupinstall Xfce   
    shell> yum install xfce4-terminal # xfce下的命令行工具, 不安装的话无法再xfce进入命令行

    shell> yum install cjkuni-uming-fonts 中文字体 

## Startx
    shell> startxfce4
    shell> systemctl isolate graphical.target
    # 先startxfce4后systemctl isolate graphical.target
