## Ubuntu apt 获取资源的时候可能导致的问题

### 执行 `sudo apt install` 时提示 `/var/lib/dpkg/lock`

报错日志

```
root@linux:~# apt install openjdk-8-jre-headless
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```

分析原因:

- 另一个程序正在运行，导致资源被锁不可用
- 导致资源被锁的原因可能是上次运行安装或更新没有正常完成，解决办法就是删掉

``` shell
# 查找并杀死s使用 apt 的进程
ps -aux | grep apt
kill pid

lsof /var/lib/dpkg/lock


# 其他方法
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```

参考：How to Fix ‘E: Could not get lock /var/lib/dpkg/lock’ Error in Ubuntu Linux  https://itsfoss.com/could-not-get-lock-error/


### Python cv2库 import 的问题

在 import cv2 时报错

``` text
ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory
```

解决：

``` shell
!apt-get update
!apt-get -y upgrade
!pip install opencv-python
!apt-get install libgtk2.0-dev -y
```

### 记录安装 apt 安装 OpenGL 的问题

``` text
zhi@abc:~$ sudo apt-get install libgl1-mesa-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 libgl1-mesa-dev : Depends: mesa-common-dev (= 11.2.0-1ubuntu2) but it is not going to be installed
                   Depends: libgl1-mesa-glx (= 11.2.0-1ubuntu2) but 17.2.4-0ubuntu1~16.04.4 is to be installed
                   Depends: libdrm-dev (>= 2.4.65) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

``` text
zhi@abc:~$ sudo apt install libdrm-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 libdrm-dev : Depends: libdrm2 (= 2.4.67-1) but 2.4.83-1~16.04.1 is to be installed
              Depends: libdrm-intel1 (= 2.4.67-1) but 2.4.83-1~16.04.1 is to be installed
              Depends: libdrm-radeon1 (= 2.4.67-1) but 2.4.83-1~16.04.1 is to be installed
              Depends: libdrm-nouveau2 (= 2.4.67-1) but 2.4.83-1~16.04.1 is to be installed
              Depends: libdrm-amdgpu1 (= 2.4.67-1) but 2.4.83-1~16.04.1 is to be installed
E: Unable to correct problems, you have held broken packages.
```

``` text
zhi@abc:~$ apt-cache policy libdrm-dev
libdrm-dev:
  Installed: (none)
  Candidate: 2.4.67-1
  Version table:
     2.4.67-1 500
        500 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 Packages
```

Try to solve ->
https://askubuntu.com/questions/581242/ubuntu-14-04-installation-of-libdrm-dev

``` text
zhi@abc:~$ sudo add-apt-repository ppa:xorg-edgers/ppa 
 == Xorg packages fresh from git ==

Currently supported releases are Xenial/16.04 and Yakkety/16.10

* WARNING: Do not use this PPA with enabled HWE stack.

Be sure to revert this PPA before doing a release upgrade or the upgrade will not succeed. To revert to official packages, install the ppa-purge package and run "sudo ppa-purge xorg-edgers".

== Important notice ==

This PPA is currently meant to be used as a whole. Please do _not_ individually install packages from it, add it to your sources and let your package manager pull in every update. The packages here build against each other and compile different features based on whats available at build time. Do not assume that because it lets you install a DDX with just the driver and libdrm update that it will work.  These packages are made with scripts that use the the current packages as the base, so some dependencies can be wrong and your package manager will not resolve that for you.  If you want to individually install something from here, grab the source and rebuild it in your current environment instead.

** Please do not publish instructions for how to install from this archive without linking to this page! Anyone using packages from this archive is expected to read this page first and it is recommended to check back occasionally for notice on problems that may arise. **

** Please use ppa-purge to remove this PPA. It is *particularly* recommended to do this before upgrading to a new ubuntu release! **
 More info: https://launchpad.net/~xorg-edgers/+archive/ubuntu/ppa
Press [ENTER] to continue or ctrl-c to cancel adding it

gpg: keyring `/tmp/tmp798nb164/secring.gpg' created
gpg: keyring `/tmp/tmp798nb164/pubring.gpg' created
gpg: requesting key 8844C542 from hkp server keyserver.ubuntu.com
gpg: /tmp/tmp798nb164/trustdb.gpg: trustdb created
gpg: key 8844C542: public key "Launchpad PPA for xorg crack pushers" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
OK
```

Then ->

``` text
zhi@abc:~$ sudo apt-get install libgl1-mesa-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libdrm-amdgpu1 libdrm-amdgpu1:i386 libdrm-common libdrm-dev libdrm-intel1 libdrm-intel1:i386 libdrm-nouveau2 libdrm-nouveau2:i386 libdrm-radeon1
  libdrm-radeon1:i386 libdrm2 libdrm2:i386 libgl1-mesa-glx libgl1-mesa-glx:i386 libglapi-mesa libglapi-mesa:i386 libgles2-mesa libosmesa6
  libosmesa6:i386 libx11-xcb-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-shape0-dev
  libxcb-sync-dev libxcb-xfixes0-dev libxshmfence-dev libxxf86vm-dev mesa-common-dev x11proto-dri2-dev x11proto-gl-dev x11proto-xf86vidmode-dev
The following NEW packages will be installed:
  libdrm-dev libgl1-mesa-dev libx11-xcb-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-shape0-dev
  libxcb-sync-dev libxcb-xfixes0-dev libxshmfence-dev libxxf86vm-dev mesa-common-dev x11proto-dri2-dev x11proto-gl-dev x11proto-xf86vidmode-dev
The following packages will be upgraded:
  libdrm-amdgpu1 libdrm-amdgpu1:i386 libdrm-common libdrm-intel1 libdrm-intel1:i386 libdrm-nouveau2 libdrm-nouveau2:i386 libdrm-radeon1
  libdrm-radeon1:i386 libdrm2 libdrm2:i386 libgl1-mesa-glx libgl1-mesa-glx:i386 libglapi-mesa libglapi-mesa:i386 libgles2-mesa libosmesa6
  libosmesa6:i386
18 upgraded, 17 newly installed, 0 to remove and 15 not upgraded.
Need to get 4,316 kB of archives.
After this operation, 6,655 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://cn.archive.ubuntu.com/ubuntu xenial-security/main amd64 libx11-xcb-dev amd64 2:1.6.3-1ubuntu2.1 [9,718 B]
Get:2 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-dri3-dev amd64 1.11.1-1ubuntu1 [5,752 B]
Get:3 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-randr0-dev amd64 1.11.1-1ubuntu1 [18.2 kB]
Get:4 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-shape0-dev amd64 1.11.1-1ubuntu1 [6,900 B]
Get:5 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-xfixes0-dev amd64 1.11.1-1ubuntu1 [11.2 kB]
Get:6 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-sync-dev amd64 1.11.1-1ubuntu1 [10.1 kB]
Get:7 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-present-dev amd64 1.11.1-1ubuntu1 [6,618 B]
Get:8 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxshmfence-dev amd64 1.2-1 [3,676 B]
Get:9 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-dri2-0-dev amd64 1.11.1-1ubuntu1 [8,384 B]
Get:10 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxcb-glx0-dev amd64 1.11.1-1ubuntu1 [26.9 kB]
Get:11 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 x11proto-xf86vidmode-dev all 2.3.1-2 [6,116 B]
Get:12 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 libxxf86vm-dev amd64 1:1.1.4-1 [13.3 kB]
Get:13 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 x11proto-dri2-dev all 2.8-2 [12.6 kB]
Get:14 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 x11proto-gl-dev all 1.4.17-1 [17.9 kB]
Get:15 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-common all 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [13.7 kB]
Get:16 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm2 amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [39.7 kB]    
Get:17 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libdrm2 i386 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [42.6 kB]      
Get:18 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libdrm-amdgpu1 i386 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [31.9 kB]
Get:19 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-amdgpu1 amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [28.6 kB]
Get:20 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libdrm-intel1 i386 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [73.5 kB]
Get:21 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-intel1 amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [69.6 kB]
Get:22 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libdrm-radeon1 i386 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [32.7 kB]
Get:23 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-radeon1 amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [31.2 kB]
Get:24 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libdrm-nouveau2 i386 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [27.8 kB]
Get:25 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-nouveau2 amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [25.9 kB]
Get:26 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libdrm-dev amd64 2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2 [244 kB]  
Get:27 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 mesa-common-dev amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [544 kB]                   
Get:28 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libosmesa6 i386 18.0.5-0ubuntu0~16.04.1~ppa1 [1,346 kB]                        
Err:28 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libosmesa6 i386 18.0.5-0ubuntu0~16.04.1~ppa1                                   
  Connection timed out [IP: 91.189.95.83 80]
Get:29 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libosmesa6 amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [1,272 kB]
Get:30 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libgles2-mesa amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [13.4 kB]                                                                          
Get:31 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libgl1-mesa-glx i386 18.0.5-0ubuntu0~16.04.1~ppa1 [139 kB]                                                                           
Get:32 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libgl1-mesa-glx amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [132 kB]                                                                         
Get:33 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libglapi-mesa amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [23.4 kB]                                                                          
Get:34 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libglapi-mesa i386 18.0.5-0ubuntu0~16.04.1~ppa1 [23.4 kB]                                                                            
Get:35 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main amd64 libgl1-mesa-dev amd64 18.0.5-0ubuntu0~16.04.1~ppa1 [4,454 B]                                                                        
Fetched 2,971 kB in 3min 4s (16.1 kB/s)                                                                                                                                                                      
E: Failed to fetch http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu/pool/main/m/mesa/libosmesa6_18.0.5-0ubuntu0~16.04.1~ppa1_i386.deb  Connection timed out [IP: 91.189.95.83 80]

E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
```

Again ->

``` text
zhi@abc:~$ sudo apt-get install libgl1-mesa-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libdrm-amdgpu1 libdrm-amdgpu1:i386 libdrm-common libdrm-dev libdrm-intel1 libdrm-intel1:i386 libdrm-nouveau2 libdrm-nouveau2:i386 libdrm-radeon1
  libdrm-radeon1:i386 libdrm2 libdrm2:i386 libgl1-mesa-glx libgl1-mesa-glx:i386 libglapi-mesa libglapi-mesa:i386 libgles2-mesa libosmesa6
  libosmesa6:i386 libx11-xcb-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-shape0-dev
  libxcb-sync-dev libxcb-xfixes0-dev libxshmfence-dev libxxf86vm-dev mesa-common-dev x11proto-dri2-dev x11proto-gl-dev x11proto-xf86vidmode-dev
The following NEW packages will be installed:
  libdrm-dev libgl1-mesa-dev libx11-xcb-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-shape0-dev
  libxcb-sync-dev libxcb-xfixes0-dev libxshmfence-dev libxxf86vm-dev mesa-common-dev x11proto-dri2-dev x11proto-gl-dev x11proto-xf86vidmode-dev
The following packages will be upgraded:
  libdrm-amdgpu1 libdrm-amdgpu1:i386 libdrm-common libdrm-intel1 libdrm-intel1:i386 libdrm-nouveau2 libdrm-nouveau2:i386 libdrm-radeon1
  libdrm-radeon1:i386 libdrm2 libdrm2:i386 libgl1-mesa-glx libgl1-mesa-glx:i386 libglapi-mesa libglapi-mesa:i386 libgles2-mesa libosmesa6
  libosmesa6:i386
18 upgraded, 17 newly installed, 0 to remove and 15 not upgraded.
Need to get 1,346 kB/4,316 kB of archives.
After this operation, 6,655 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://ppa.launchpad.net/xorg-edgers/ppa/ubuntu xenial/main i386 libosmesa6 i386 18.0.5-0ubuntu0~16.04.1~ppa1 [1,346 kB]
Fetched 21.8 kB in 6s (3,434 B/s)                                                                                                                                                                            
Extracting templates from packages: 100%
(Reading database ... 325875 files and directories currently installed.)
Preparing to unpack .../libdrm-common_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_all.deb ...
Unpacking libdrm-common (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm2_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
De-configuring libdrm2:i386 (2.4.83-1~16.04.1) ...
Unpacking libdrm2:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm2_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_i386.deb ...
Unpacking libdrm2:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-amdgpu1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_i386.deb ...
De-configuring libdrm-amdgpu1:amd64 (2.4.83-1~16.04.1) ...
Unpacking libdrm-amdgpu1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-amdgpu1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
Unpacking libdrm-amdgpu1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-intel1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
De-configuring libdrm-intel1:i386 (2.4.83-1~16.04.1) ...
Unpacking libdrm-intel1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-intel1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_i386.deb ...
Unpacking libdrm-intel1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-radeon1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
De-configuring libdrm-radeon1:i386 (2.4.83-1~16.04.1) ...
Unpacking libdrm-radeon1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-radeon1_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_i386.deb ...
Unpacking libdrm-radeon1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-nouveau2_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
De-configuring libdrm-nouveau2:i386 (2.4.83-1~16.04.1) ...
Unpacking libdrm-nouveau2:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Preparing to unpack .../libdrm-nouveau2_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_i386.deb ...
Unpacking libdrm-nouveau2:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) over (2.4.83-1~16.04.1) ...
Selecting previously unselected package libdrm-dev:amd64.
Preparing to unpack .../libdrm-dev_2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2_amd64.deb ...
Unpacking libdrm-dev:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Selecting previously unselected package mesa-common-dev:amd64.
Preparing to unpack .../mesa-common-dev_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
Unpacking mesa-common-dev:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Preparing to unpack .../libosmesa6_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
De-configuring libosmesa6:i386 (17.2.4-0ubuntu1~16.04.4) ...
Unpacking libosmesa6:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libosmesa6_18.0.5-0ubuntu0~16.04.1~ppa1_i386.deb ...
Unpacking libosmesa6:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libgles2-mesa_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
Unpacking libgles2-mesa:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libgl1-mesa-glx_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
De-configuring libgl1-mesa-glx:i386 (17.2.4-0ubuntu1~16.04.4) ...
Unpacking libgl1-mesa-glx:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libgl1-mesa-glx_18.0.5-0ubuntu0~16.04.1~ppa1_i386.deb ...
Unpacking libgl1-mesa-glx:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libglapi-mesa_18.0.5-0ubuntu0~16.04.1~ppa1_i386.deb ...
De-configuring libglapi-mesa:amd64 (17.2.4-0ubuntu1~16.04.4) ...
Unpacking libglapi-mesa:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Preparing to unpack .../libglapi-mesa_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
Unpacking libglapi-mesa:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) over (17.2.4-0ubuntu1~16.04.4) ...
Selecting previously unselected package libx11-xcb-dev:amd64.
Preparing to unpack .../libx11-xcb-dev_2%3a1.6.3-1ubuntu2.1_amd64.deb ...
Unpacking libx11-xcb-dev:amd64 (2:1.6.3-1ubuntu2.1) ...
Selecting previously unselected package libxcb-dri3-dev:amd64.
Preparing to unpack .../libxcb-dri3-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-dri3-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-randr0-dev:amd64.
Preparing to unpack .../libxcb-randr0-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-randr0-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-shape0-dev:amd64.
Preparing to unpack .../libxcb-shape0-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-shape0-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-xfixes0-dev:amd64.
Preparing to unpack .../libxcb-xfixes0-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-xfixes0-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-sync-dev:amd64.
Preparing to unpack .../libxcb-sync-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-sync-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-present-dev:amd64.
Preparing to unpack .../libxcb-present-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-present-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxshmfence-dev:amd64.
Preparing to unpack .../libxshmfence-dev_1.2-1_amd64.deb ...
Unpacking libxshmfence-dev:amd64 (1.2-1) ...
Selecting previously unselected package libxcb-dri2-0-dev:amd64.
Preparing to unpack .../libxcb-dri2-0-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-dri2-0-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package libxcb-glx0-dev:amd64.
Preparing to unpack .../libxcb-glx0-dev_1.11.1-1ubuntu1_amd64.deb ...
Unpacking libxcb-glx0-dev:amd64 (1.11.1-1ubuntu1) ...
Selecting previously unselected package x11proto-xf86vidmode-dev.
Preparing to unpack .../x11proto-xf86vidmode-dev_2.3.1-2_all.deb ...
Unpacking x11proto-xf86vidmode-dev (2.3.1-2) ...
Selecting previously unselected package libxxf86vm-dev:amd64.
Preparing to unpack .../libxxf86vm-dev_1%3a1.1.4-1_amd64.deb ...
Unpacking libxxf86vm-dev:amd64 (1:1.1.4-1) ...
Selecting previously unselected package x11proto-dri2-dev.
Preparing to unpack .../x11proto-dri2-dev_2.8-2_all.deb ...
Unpacking x11proto-dri2-dev (2.8-2) ...
Selecting previously unselected package x11proto-gl-dev.
Preparing to unpack .../x11proto-gl-dev_1.4.17-1_all.deb ...
Unpacking x11proto-gl-dev (1.4.17-1) ...
Selecting previously unselected package libgl1-mesa-dev:amd64.
Preparing to unpack .../libgl1-mesa-dev_18.0.5-0ubuntu0~16.04.1~ppa1_amd64.deb ...
Unpacking libgl1-mesa-dev:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicudata.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libiculx.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickControls2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5RemoteObjects.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5EglFSDeviceIntegration.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5DataVisualization.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuuc.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Bluetooth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DLogic.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutest.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Charts.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Scxml.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuio.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5MultimediaQuick_p.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutu.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Gamepad.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickTemplates2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicule.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DRender.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebView.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicui18n.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialBus.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickParticles.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebChannel.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5ScriptTools.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickScene2D.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialPort.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Purchasing.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5XmlPatterns.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebSockets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngine.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5NetworkAuth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Sensors.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuick.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5TextToSpeech.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Nfc.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineWidgets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Script.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Location.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickRender.so.5 is not a symbolic link

Processing triggers for man-db (2.7.5-1) ...
Setting up libdrm-common (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm2:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm2:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-amdgpu1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-amdgpu1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-intel1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-intel1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-radeon1:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-radeon1:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-nouveau2:i386 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-nouveau2:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up libdrm-dev:amd64 (2.4.92+git20180609.c1f2d9b9-0ubuntu0ricotz~16.04.2) ...
Setting up mesa-common-dev:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libglapi-mesa:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libglapi-mesa:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libosmesa6:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libosmesa6:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libgles2-mesa:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Setting up libgl1-mesa-glx:i386 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicudata.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libiculx.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickControls2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5RemoteObjects.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5EglFSDeviceIntegration.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5DataVisualization.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuuc.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Bluetooth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DLogic.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutest.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Charts.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Scxml.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuio.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5MultimediaQuick_p.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutu.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Gamepad.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickTemplates2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicule.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DRender.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebView.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicui18n.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialBus.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickParticles.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebChannel.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5ScriptTools.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickScene2D.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialPort.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Purchasing.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5XmlPatterns.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebSockets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngine.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5NetworkAuth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Sensors.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuick.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5TextToSpeech.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Nfc.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineWidgets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Script.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Location.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickRender.so.5 is not a symbolic link

Setting up libgl1-mesa-glx:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
update-alternatives: warning: forcing reinstallation of alternative /usr/lib/x86_64-linux-gnu/mesa/ld.so.conf because link group x86_64-linux-gnu_gl_conf is broken
/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicudata.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libiculx.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickControls2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5RemoteObjects.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5EglFSDeviceIntegration.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5DataVisualization.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuuc.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Bluetooth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DLogic.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutest.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Charts.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Scxml.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuio.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5MultimediaQuick_p.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutu.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Gamepad.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickTemplates2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicule.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DRender.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebView.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicui18n.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialBus.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickParticles.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebChannel.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5ScriptTools.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickScene2D.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialPort.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Purchasing.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5XmlPatterns.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebSockets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngine.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5NetworkAuth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Sensors.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuick.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5TextToSpeech.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Nfc.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineWidgets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Script.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Location.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickRender.so.5 is not a symbolic link

Setting up libx11-xcb-dev:amd64 (2:1.6.3-1ubuntu2.1) ...
Setting up libxcb-dri3-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-randr0-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-shape0-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-xfixes0-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-sync-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-present-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxshmfence-dev:amd64 (1.2-1) ...
Setting up libxcb-dri2-0-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up libxcb-glx0-dev:amd64 (1.11.1-1ubuntu1) ...
Setting up x11proto-xf86vidmode-dev (2.3.1-2) ...
Setting up libxxf86vm-dev:amd64 (1:1.1.4-1) ...
Setting up x11proto-dri2-dev (2.8-2) ...
Setting up x11proto-gl-dev (1.4.17-1) ...
Setting up libgl1-mesa-dev:amd64 (18.0.5-0ubuntu0~16.04.1~ppa1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicudata.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libiculx.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickControls2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5RemoteObjects.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5EglFSDeviceIntegration.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5DataVisualization.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuuc.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Bluetooth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DLogic.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutest.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Charts.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Scxml.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicuio.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5MultimediaQuick_p.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicutu.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Gamepad.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickTemplates2.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicule.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DRender.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebView.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libicui18n.so.56 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialBus.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5QuickParticles.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickInput.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebChannel.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5ScriptTools.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickScene2D.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5SerialPort.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Purchasing.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5XmlPatterns.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebSockets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngine.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5NetworkAuth.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineCore.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickAnimation.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Sensors.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuick.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5TextToSpeech.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Nfc.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5WebEngineWidgets.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Script.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickExtras.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt5Location.so.5 is not a symbolic link

/sbin/ldconfig.real: /usr/lib/x86_64-linux-gnu/libQt53DQuickRender.so.5 is not a symbolic link
```