### mvms

####bin
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

# make yocto: build kernel-image, device tree, root filesystem
# init environment 
source oe-init-build-env build-$BRANCH
# creates directory build-$BRANCH, must be patched

# later only once called in one session:
source oe-init-build-env build-$BRANCH
bitbake $TARGET
```
