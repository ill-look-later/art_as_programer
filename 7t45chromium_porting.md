# 7t45_chromium_porting

## 1.搭建硬件平台 升级rootfs && kernel
- cp 升级包到u盘根目录
- 进入uboot

```shell
#usb start 1
#run updateall
#setenv bootargs 'console=ttyS0,115200 root=ubi:RFS rootfstype=ubifs rw ubi.mtd=UBI ubi.mtd=UBILD  init=/init LX_MEM=0x06800000 GMAC_MEM=0x06800000,0x00100000 DRAM_LEN=0x20000000 LX_MEM2=0x50C10400,0x000C800000 BB_ADDR=0  mtdparts=edb64M-nand:3m(BFN),0x200000(UBILD),0x7800000(UBI)'
#saveenv
#reset


```

- config the profile, add cmd 'udhcpc' to ensure the network work well
- 此时，通过#cat /proc/memeinfo 可以看到

```shell

MemTotal:         300252 kB
MemFree:          287932 kB
Buffers:               0 kB
Cached:             2436 kB
SwapCached:            0 kB
Active:             1836 kB
Inactive:           1236 kB
Active(anon):        636 kB
Inactive(anon):        0 kB
Active(file):       1200 kB
Inactive(file):     1236 kB
Unevictable:           0 kB
Mlocked:               0 kB
HighTotal:        204796 kB
HighFree:         198136 kB
LowTotal:          95456 kB
LowFree:           89796 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 4 kB
Writeback:             0 kB
AnonPages:           672 kB
Mapped:             1644 kB
Shmem:                 0 kB
Slab:               4352 kB
SReclaimable:       1640 kB
SUnreclaim:         2712 kB
KernelStack:         400 kB
PageTables:          112 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:      150124 kB
Committed_AS:       3168 kB
VmallocTotal:     491512 kB
VmallocUsed:        3780 kB
VmallocChunk:     487284 kB
```
---
## 2 .configure sraf browser runtime environment 
- mount nfs

```shell
mkdir /tmp/nfs202
mkdir /tmp/nfs206
mount -o nolock 192.168.18.206:/nfs /tmp/nfs206
mount -o nolock 192.168.18.202:/nfs /tmp/nfs202
export SSL_CERT_DIR=/tmp/nfs206/certs
export http_proxy=192.168.18.205:808
export https_proxy=192.168.18.205:808
```
