### mvms

####build kernel-image, device tree, root filesystem
#####bin/setup
defines environment vars: MVMS ( = $PWD ), POKY, BOOT, ROOTFS, BRANCH, TARGET

#####bin/getyocto
clone repositories:
```
   ./poky
   ./poky/meta-openembedded
   ./poky/meta-atmel
```
creates directories

builddir = build-$BRANCH

#####bin/checkout
checkout repositories ( poky, meta-openembedded, meta-atmel ) to $BRANCH

#### usage example

```
cd /data
git clone https://github.com/WernerArmingeon/mvms.git

. mvms/bin/setup

bin/getyocto

cd poky

# init environment & build-directory 
source oe-init-build-env build-$BRANCH
# creates directory build-$BRANCH, config/ must be patched:

# patched files in github mvms repository:
#     poky/build-mvms-d3/conf/bblayers.conf
#     poky/build-mvms-d3/conf/local.conf

w

# later only once called in one session:
. /data/mvms/bin/setup
cd poky
source oe-init-build-env build-$BRANCH

# make yocto: build kernel-image, device tree, root filesystem
# runs here about 55 minutes
bitbake $TARGET

# prepare boot files & rootfs for NFS download / mount
/data/mvms/bin/update
```
####uboot setup
```
# add your account to group dialout
# nfs export /data/mvms:
sudo vi /etc/exports
# add line:
/data/mvms              *(rw,rw,no_root_squash,no_all_squash,no_subtree_check)

sudo exportfs -r

# power on EVB, reset few times, restart command:
picocom -b 115200 /dev/ttyACM0

uboot >print

# verify settings:

setenv bootdelay 3

setenv eth1addr 3e:36:65:ba:6f:bf

setenv ethact gmacb0

setenv ethaddr 3e:36:65:ba:6f:be

setenv ethprime gmacb0
setenv ipaddr 192.168.10.10
setenv serverip 192.168.10.1

setenv bootargs "root=/dev/nfs rw nfsroot=192.168.10.1:/data/mvms/mvms-d3/rootfs ip=192.168.10.10:192.168.10.1:192.168.10.1:255.255.255.0:sama5d3ek:eth0:off console=ttyS0,115200"

setenv go1 "nfs 0x21000000 /data/mvms/mvms-d3/boot/sama5d3ek.dtb; nfs 0x22000000 /data/mvms/mvms-d3/boot/sama5d3ek.bin;  bootz 0x22000000 - 0x21000000"

savenv
run go1
```

