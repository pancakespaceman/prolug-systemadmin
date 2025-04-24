# Unit 3 Lab - Storage

## Resources

(Killercoda labs)[https://killercoda.com/learn]
(Ubuntu Documentation on LVM)[https://documentation.ubuntu.com/server/explanation/storage/about-lvm/index.html]

## Pre-Lab Warm-up

Exercises

```
cd ~
mkdir lvm_lab
cd lvm_lab
touch somefile
echo “this is a string of text” > somefile
cat somefile
echo “this is a string of text” > somefile
# Repeat 3 times
cat somefile
# How many lines are there?
Only one line. > operator overwrites the file

Echo “this is a string of text” >> somefile
# Repeat 3 times
cat somefile
# How many lines are there?
cheat with `cat somefile | wc -l`

There are 5 lines, as >> appends to the file.

echo “this is our other test text” >> somefile
# Repeat 3 times
cat somefile | nl
# How many lines are there?
9 lines

cat somefile | nl | grep test
# compare that with 14
cat somefile | grep test | nl

first command adds line numbers first before grep, so grep shows line numbers 6-9.
second command greps first then adds line numbers, so the numbers are 1-4
```
If you want to preserve positional lines in file (know how much you’ve cut out when
you grep something, or generally be able to find it in the unfiltered file for context,
always | nl | before your grep


### Pre-Lab - Disk Speed Tests:

When using the ProLUG lab environment, you should always check that there are no 
other users on the system w or who.
```
[root@rocky7 lvm_lab]# w
 16:42:43 up 2 days, 59 min,  1 user,  load average: 0.07, 0.02, 0.00
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0     16:33    1.00s  0.15s  0.00s w
[root@rocky7 lvm_lab]# who
root     pts/0        2025-04-21 16:33 (192.168.11.245)
```

After this, you may want to check the current state of the disks, as they retain their 
information even after a reboot resets the rest of the machine. lsblk /dev/xvda.

```
[root@rocky7 lvm_lab]# lsblk /dev/xvda
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
xvda 202:0    0  15G  0 disk
```

```
# If you need to wipe the disks, you should use fdisk or a similar partition utility.
fdisk /dev/xvda

p #print to see partitions
d #delete partitions or information
w #Write out the changes to the disk.
```

This is an aside, before the lab. This is a way to test different read or writes
into or out of your filesystems as you create them. Different types of raid and different
disk setups will give different speed of read and write. This is a simple way to test them.
Use these throughout the lab in each mount for fun and understanding.

#### Write tests (saving off write data - rename /tmp/file each time):

```
# Check /dev/xvda for a filesystem
blkid /dev/xvda

[root@rocky7 lvm_lab]# blkid /dev/xvda
/dev/xvda: UUID="178eeaaa-f373-427f-b241-7ba5feb2fef4" TYPE="ext4"


# If it does not have one, make one
mkfs.ext4 /dev/xvda
mkdir /space #(If you don’t have it. Lab will tell you to later as well)

mount /dev/xvda /space

[root@rocky7 space]# ls
lost+found
```

#### Write Test:

```
for i in `seq 1 10`; do time dd if=/dev/zero of=/space/testfile$i bs=1024k count=1000 | tee -a /tmp/speedtest1.basiclvm; done


1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 2.28742 s, 458 MB/s

real    0m2.349s
user    0m0.006s
sys     0m1.822s
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 3.00665 s, 349 MB/s

real    0m3.071s
user    0m0.008s
sys     0m2.444s
```

#### Read Tests:
```
for i in `seq 1 10`; do time dd if=/space/testfile$i of=/dev/null; done


2048000+0 records in
2048000+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 6.65312 s, 158 MB/s

real    0m6.656s
user    0m2.268s
sys     0m3.872s
2048000+0 records in
2048000+0 records out
```

#### Cleanup:
```
for i in `seq 1 10`; do rm -rf /space/testfile$i; done

[root@rocky7 space]# ls
lost+found
```

If you are re-creating a test without blowing away the filesystem, change the name
or counting numbers of testfile because that’s the only way to be sure there is 
not some type of filesystem caching going on to optimize. 
This is especially true in SAN write tests.


## Lab

start in root (#); cd /root

### LVM explanation and use within the system
```
# Check physical volumes on your server (my output may vary)
[root@rocky1 ~]fdisk -l | grep -i xvd

Disk /dev/xvda: 15 GiB, 16106127360 bytes, 31457280 sectors
Disk /dev/xvdb: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvde: 3 GiB, 3221225472 bytes, 6291456 sectors


[root@rocky7 ~]# fdisk -l | grep -i xvd
Disk /dev/xvdb: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvda: 15 GiB, 16106127360 bytes, 31457280 sectors
Disk /dev/xvdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvde: 3 GiB, 3221225472 bytes, 6291456 sectors
```

#### Looking at Logical Volume Management

Logical Volume Management is an abstraction layer that looks a lot like how we 
carve up SAN disks for storage management. We have Physical Volumes that get grouped
up into Volume Groups. We carve Volume Groups up to be presented as Logical Volumes.

Here at the Logical Volume layer we can assign RAID functionality from our Physical
Volumes attached to a Volume Group or do all kinds of different things that are
"under the hood". Logical Volumes get filesystems formatting and are mounted
to the OS.

There are many important commands for showing your physical volumes, volume groups,
and logical volumes.

The three simplest and easiest are:

```
pvs
vgs
lvs
```

With these you can see basic information that allows you to see how the disks
are allocated. Why do you think there is no output from these commands the first
time you run them? Try these next commands to see if you can figure out what
is happening? To see more in depth information try `pvdisplay`, `vgdisplay`, and
`lvdisplay`.

If there is still no output, it's because this system is not configured for LVM.
You will notice that none of the disk you verified are attached are allocated
to LVM yet. We'll do that next.

- So these commands don't show anything because there is no LVM configuration.


#### Creating and Carving up your LVM resources

Disks for this lab are `/dev/xvdb`, `/dev/xvdc`, and `/dev/xvdd`. (verify before continuing
and adjust accordingly)

We can do invidivual pvcreates for each disk `pvcreate /dev/xvdb` but we can also
loop over them with a simple loop as below. Use your drive letters.

```
for disk in b c d; do pvcreate /dev/xvd$disk; done

# Physical volume "/dev/xvdb" successfully created.
# Creating devices file /etc/lvm/devices/system.devices
# Physical volume "/dev/xvdc" successfully created.
# Physical volume "/dev/xvde" successfully created.

[root@rocky7 ~]# for disk in b c e; do pvcreate /dev/xvd$disk; done
  Physical volume "/dev/xvdb" successfully created.
  Physical volume "/dev/xvdc" successfully created.
  Physical volume "/dev/xvde" successfully created.

# To see what we made:

pvs

# PV VG Fmt Attr PSize PFree
# /dev/xvdb lvm2 --- 3.00g 3.00g
# /dev/xvdc lvm2 --- 3.00g 3.00g
# /dev/xvde lvm2 --- 3.00g 3.00g

[root@rocky7 ~]# pvs
  PV         VG Fmt  Attr PSize PFree
  /dev/xvdb     lvm2 ---  3.00g 3.00g
  /dev/xvdc     lvm2 ---  3.00g 3.00g
  /dev/xvde     lvm2 ---  3.00g 3.00g
  
---  

vgcreate VolGroupTest /dev/xvdb /dev/xvdc /dev/xvde

# Volume group "VolGroupTest" successfully created

[root@rocky7 ~]# vgcreate VolGroupTest /dev/xvdb /dev/xvdc /dev/xvde
  Volume group "VolGroupTest" successfully created
  
---

vgs
# VG #PV #LV #SN Attr VSize VFree
# VolGroupTest 3 0 0 wz--n- <8.99g <8.99g

[root@rocky7 ~]# vgs
  VG           #PV #LV #SN Attr   VSize  VFree
  VolGroupTest   3   0   0 wz--n- <8.99g <8.99g
  
---

lvcreate -l +100%FREE -n lv_test VolGroupTest

# Logical volume "lv_test" created.

[root@rocky7 ~]# lvcreate -l +100%FREE -n lv_test VolGroupTest
WARNING: ext4 signature detected on /dev/VolGroupTest/lv_test at offset 1080. Wipe it? [y/n]: y
  Wiping ext4 signature on /dev/VolGroupTest/lv_test.
  Logical volume "lv_test" created.

lvs


# LV VG Attr LSize Pool Origin Data% Meta% Move Log Cpy%Sync Convert
# lv_test VolGroupTest -wi-a----- <8.99g

[root@rocky7 ~]# lvs
  LV      VG           Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_test VolGroupTest -wi-a----- <8.99g

---

# Formatting and mounting the filesystem

mkfs.ext4 /dev/mapper/VolGroupTest-lv_test

# mke2fs 1.42.9 (28-Dec-2013)
# Filesystem label=
# OS type: Linux
# Block size=4096 (log=2)
# Fragment size=4096 (log=2)
# Stride=0 blocks, Stripe width=0 blocks
# 983040 inodes, 3929088 blocks
# 196454 blocks (5.00%) reserved for the super user
# First data block=0
# Maximum filesystem blocks=2151677952
# 120 block groups
# 32768 blocks per group, 32768 fragments per group
# 8192 inodes per group
# Superblock backups stored on blocks:
# 32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208
#
# Allocating group tables: done
# Writing inode tables: done
# Creating journal (32768 blocks): done
# Writing superblocks and filesystem accounting information: done

[root@rocky7 ~]# mkfs.ext4 /dev/mapper/VolGroupTest-lv_test
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 2356224 4k blocks and 589824 inodes
Filesystem UUID: 92502ea1-b5ce-4118-a39b-8af1d04c6f6d
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

---

mkdir /space #Created earlier
vi /etc/fstab

# Add the following line
# /dev/mapper/VolGroupTest-lv_test /space ext4 defaults 0 0

# reload fstab
systemctl daemon-reload
```

If this command works, there will be no output. We use the df -h in the
next command to verify the new filesystem exists. The use of mount -a and 
not manually mounting the filesystem from the command line is an old administration
trick I picked up over the years.

By setting our mount in /etc/fstab and then telling the system to mount everything
we verify that this will come back up properly during a reboot. We have mounted
and verified we have a persistent mount in one step.

```
df -h

# Filesystem Size Used Avail Use% Mounted on
# devtmpfs 4.0M 0 4.0M 0% /dev
# tmpfs 2.0G 0 2.0G 0% /dev/shm
# tmpfs 2.0G 8.5M 1.9G 1% /run
# tmpfs 2.0G 1.4G 557M 72% /
# tmpfs 2.0G 0 2.0G 0% /run/shm
# 192.168.200.25:/home 44G 15G 30G 34% /home
# 192.168.200.25:/opt 44G 15G 30G 34% /opt
# tmpfs 390M 0 390M 0% /run/user/0
# /dev/mapper/VolGroupTest-lv*test 8.8G 24K 8.3G 1% /space


NOTE: i didn't know you had to run mount -a here.
I understood that for the raid part.

[root@rocky7 /]# df -h
Filesystem                Size  Used Avail Use% Mounted on
devtmpfs                  4.0M     0  4.0M   0% /dev
tmpfs                     2.0G     0  2.0G   0% /dev/shm
tmpfs                     2.0G  8.6M  1.9G   1% /run
tmpfs                     8.0G  2.0G  6.1G  25% /
tmpfs                     2.0G     0  2.0G   0% /run/shm
192.168.200.25:/lab_work  194G   45G  150G  23% /lab_work
192.168.200.25:/opt       194G   45G  150G  23% /opt
192.168.200.25:/labs      194G   45G  150G  23% /labs
192.168.200.25:/home      194G   45G  150G  23% /home
tmpfs                     390M     0  390M   0% /run/user/0
```

#### Removing and breaking down the LVM to raw disks

The following command is one way to comment out the line in /etc/fstab.
If you had to do this across multiple severs this could be useful. (or you could
just use vi for simplicity)

```
grep lv_test /etc/fstab; perl -pi -e "s/\/dev\/mapper\/VolGroupTest/#removed \/dev\/mapper\/VolGroupTest/" /etc/fstab; grep removed /etc/fstab
# /dev/mapper/VolGroupTest-lv_test /space ext4 defaults 0 0
#removed dev/mapper/VolGroupTest-lv_test /space ext4 defaults 0 0

[root@rocky7 /]# grep lv_test /etc/fstab; perl -pi -e "s/\/dev\/mapper\/VolGroupTest/#removed \/dev\/mapper\/VolGroupTest/" /etc/fstab; grep removed /etc/fstab
/dev/mapper/VolGroupTest-lv_test /space ext4 defaults 0 0
#removed /dev/mapper/VolGroupTest-lv_test /space ext4 defaults 0 0

---

umount /space
lvremove /dev/mapper/VolGroupTest-lv_test

# Do you really want to remove active logical volume VolGroupTest/lv_test? [y/n]: y
# Logical volume "lv_test" successfully removed

[root@rocky7 /]# lvremove /dev/mapper/VolGroupTest-lv_test
Do you really want to remove active logical volume VolGroupTest/lv_test? [y/n]: y
  Logical volume "lv_test" successfully removed.
  
---

vgremove VolGroupTest

# Volume group "VolGroupTest" successfully removed

[root@rocky7 /]# vgremove VolGroupTest
  Volume group "VolGroupTest" successfully removed
  
---

for disk in c e f; do pvremove /dev/sd$disk; done

# Labels on physical volume "/dev/sdc" successfully wiped.
# Labels on physical volume "/dev/sde" successfully wiped.
# Labels on physical volume "/dev/sdf" successfully wiped.

[root@rocky7 /]# for disk in b c e; do pvremove /dev/xvd$disk; done
  Labels on physical volume "/dev/xvdb" successfully wiped.
  Labels on physical volume "/dev/xvdc" successfully wiped.
  Labels on physical volume "/dev/xvde" successfully wiped.
```

### More complex types of LVM

LVM can also be used to raid disks.

Create a RAID 5 filesystem and mount it to the OS (for brevity's sake we will be
limiting show commands from here on out, please use pvs, vgs, and lvs often for
your own understanding)

```
for disk in c e f; do pvcreate /dev/sd$disk; done

# Physical volume "/dev/sdc" successfully created.
# Physical volume "/dev/sde" successfully created.
# Physical volume "/dev/sdf" successfully created.

[root@rocky7 /]# for disk in b c e; do pvcreate /dev/xvd$disk; done
  Physical volume "/dev/xvdb" successfully created.
  Physical volume "/dev/xvdc" successfully created.
  Physical volume "/dev/xvde" successfully created.
  
---

vgcreate VolGroupTest /dev/sdc /dev/sde /dev/sdf

[root@rocky7 /]# vgcreate VolGroupTest /dev/xvdb /dev/xvdc /dev/xvde
  Volume group "VolGroupTest" successfully created
  
---


lvcreate -l +100%FREE --type raid5 -n lv_test VolGroupTest

[root@rocky7 /]# lvcreate -l +100%FREE --type raid5 -n lv_test VolGroupTest
  Using default stripesize 64.00 KiB.
  Rounding size (2301 extents) down to stripe boundary size (2300 extents)
  Logical volume "lv_test" created.
  
[root@rocky7 /]# pvs
  PV         VG           Fmt  Attr PSize  PFree
  /dev/xvdb  VolGroupTest lvm2 a--  <3.00g    0
  /dev/xvdc  VolGroupTest lvm2 a--  <3.00g    0
  /dev/xvde  VolGroupTest lvm2 a--  <3.00g    0
[root@rocky7 /]# vgs
  VG           #PV #LV #SN Attr   VSize  VFree
  VolGroupTest   3   1   0 wz--n- <8.99g    0
[root@rocky7 /]# lvs
  LV      VG           Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_test VolGroupTest rwi-a-r--- 5.98g                                    100.00
  
---

mkfs.xfs /dev/mapper/VolGroupTest-lv_test

[root@rocky7 /]# mkfs.xfs /dev/mapper/VolGroupTest-lv_test
meta-data=/dev/mapper/VolGroupTest-lv_test isize=512    agcount=8, agsize=196080 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=1568640, imaxpct=25
         =                       sunit=16     swidth=32 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=512   sunit=16 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

---

vi /etc/fstab

# fix the /space directory to have these parameters (change ext4 to xfs)
/dev/mapper/VolGroupTest-lv_test /space xfs defaults 0 0

mount -a

df -h
# Filesystem Size Used Avail Use% Mounted on
# /dev/mapper/VolGroup00-LogVol08 488M 34M 419M 8% /var/log/audit
# /dev/mapper/VolGroupTest-lv_test 10G 33M 10G 1% /space

[root@rocky7 /]# df -h
Filesystem                        Size  Used Avail Use% Mounted on
devtmpfs                          4.0M     0  4.0M   0% /dev
tmpfs                             2.0G     0  2.0G   0% /dev/shm
tmpfs                             2.0G  8.6M  1.9G   1% /run
tmpfs                             8.0G  2.0G  6.1G  25% /
tmpfs                             2.0G     0  2.0G   0% /run/shm
192.168.200.25:/lab_work          194G   45G  150G  23% /lab_work
192.168.200.25:/opt               194G   45G  150G  23% /opt
192.168.200.25:/labs                   194G   45G  150G  23% /labs
192.168.200.25:/home         194G   45G  150G  23% /home
tmpfs                             390M     0  390M   0% /run/user/0
/dev/mapper/VolGroupTest-lv_test  6.0G   75M  5.9G   2% /space
```

Since we're now usin RAID 5 we would expect to see the size no loner match
the full 15B, 10GB ismuch more of a RAID 5 value 66% of raw disk space.

To verify our raid levels we use lvs

```
lvs
# LV VG Attr LSize Pool Origin Data% Meta% Move Log Cpy%Sync
# LogVol00 VolGroup00 -wi-ao---- 2.50g
# LogVol01 VolGroup00 -wi-ao---- 1000.00m
# LogVol02 VolGroup00 -wi-ao---- 5.00g
# LogVol03 VolGroup00 -wi-ao---- 1.00g
# LogVol04 VolGroup00 -wi-ao---- 5.00g
# LogVol05 VolGroup00 -wi-ao---- 1.00g
# LogVol06 VolGroup00 -wi-ao---- 1.00g
# LogVol07 VolGroup00 -wi-ao---- 512.00m
# LogVol08 VolGroup00 -wi-ao---- 512.00m
# lv*app VolGroup01 -wi-ao---- 19.90g
# lv_test VolGroupTest rwi-aor--- 9.98g 100.00

[root@rocky7 /]# lvs
  LV      VG           Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_test VolGroupTest rwi-aor--- 5.98g                                    100.00
```

Spend 5 min reading `man lvs` page to read up on raid levels and what they can
accomplish. To run RAID 5 3 disks are needed. To run RAID 6 at least 4 disks are needed.

#### Set the System back to raw disks

Umount /space and remove entry from etc/fstab
DONE.

```
lvremove /dev/mapper/VolGroupTest-lv_test
# Do you really want to remove active logical volume VolGroupTest/lv_test? [y/n]: y
# Logical volume "lv_test" successfully removed

vgremove VolGroupTest
# Volume group "VolGroupTest" successfully removed

for disk in c e f; do pvremove /dev/sd$disk; done
# Labels on physical volume "/dev/sdc" successfully wiped.
# Labels on physical volume "/dev/sde" successfully wiped.
# Labels on physical volume "/dev/sdf" successfully wiped.


[root@rocky7 /]# lvremove /dev/mapper/VolGroupTest-lv_test
Do you really want to remove active logical volume VolGroupTest/lv_test? [y/n]: y
  Logical volume "lv_test" successfully removed.
[root@rocky7 /]# vgremove VolGroupTest
  Volume group "VolGroupTest" successfully removed
[root@rocky7 /]# for disk in b c e; do pvremove /dev/xvd$disk; done
  Labels on physical volume "/dev/xvdb" successfully wiped.
  Labels on physical volume "/dev/xvdc" successfully wiped.
  Labels on physical volume "/dev/xvde" successfully wiped.
```

### Working with MDADM as another RAID option

There could be a reason to use MDADM on the system. For example you want raid
handled outside of your LVM so that you can bring in sets of new disks already
raided and treat them as their own Physical Volumes. Think, "I want to add another
layer of abstraction so that even my LVM is unaware of the RAID levels."
This has special use case, but is still useful to understand.

May have to install mdadm yum: yum install mdadm

#### Create a raid 5 with MDADM

```
mdadm --create -l raid5 /dev/md0 -n 3 /dev/sdc /dev/sde /dev/sdf

# mdadm: Defaulting to version 1.2 metadata
# mdadm: array /dev/md0 started.

[root@rocky7 /]# mdadm --create -l raid5 /dev/md0 -n 3 /dev/xvdb /dev/xvdc /dev/xvde
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

#### Add newly created /dev/md0 raid to LVM

This is same as any other add. The only differece here is that LVM is unaware of
the lower level RAID that is happening.

```
pvcreate /dev/md0
# Physical volume "/dev/md0" successfully created.

vgcreate VolGroupTest /dev/md0
# Volume group "VolGroupTest" successfully created

lvcreate -l +100%Free -n lv_test VolGroupTest
# Logical volume "lv_test" created.

lvs
# LV VG Attr LSize Pool Origin Data% Meta% Move Log
# LogVol00 VolGroup00 -wi-ao---- 2.50g
# LogVol01 VolGroup00 -wi-ao---- 1000.00m
# LogVol02 VolGroup00 -wi-ao---- 5.00g
# LogVol03 VolGroup00 -wi-ao---- 1.00g
# LogVol04 VolGroup00 -wi-ao---- 5.00g
# LogVol05 VolGroup00 -wi-ao---- 1.00g
# LogVol06 VolGroup00 -wi-ao---- 1.00g
# LogVol07 VolGroup00 -wi-ao---- 512.00m
# LogVol08 VolGroup00 -wi-ao---- 512.00m
# lv_app VolGroup01 -wi-ao---- 19.90g
# lv_test VolGroupTest -wi-a----- 9.99g


[root@rocky7 /]# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
[root@rocky7 /]# vgcreate VolGroupTest /dev/md0
  Volume group "VolGroupTest" successfully created
[root@rocky7 /]# lvcreate -l +100%Free -n lv_test VolGroupTest
  Logical volume "lv_test" created.
[root@rocky7 /]# lvs
  LV      VG           Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_test VolGroupTest -wi-a----- 5.99g
```

Note that LVM does not see that it is dealing with a raid system, but the size
is still 10G instead of 15G.

Fix your /etc/fstab to read
/dev/mapper/VolGroupTest-lv_test /space xfs defaults 0 0

```
mkfs.xfs /dev/mapper/VolGroupTest-lv_test

# meta-data=/dev/mapper/VolGroupTest-lv_test isize=512 agcount=16, agsize=163712 blks
# = sectsz=512 attr=2, projid32bit=1
# = crc=1 finobt=0, sparse=0
# data = bsize=4096 blocks=2618368, imaxpct=25
# = sunit=128 swidth=256 blks
# naming =version 2 bsize=4096 ascii-ci=0 ftype=1
# log =internal log bsize=4096 blocks=2560, version=2
# = sectsz=512 sunit=8 blks, lazy-count=1
# realtime =none extsz=4096 blocks=0, rtextents=0

mount -a
```

Good place to speed test and save off your data

#### Settin the MDADM to persist through reboots

(not in our lab environment though)

```
mdadm --detail --scan >> /etc/mdadm.conf
cat /etc/mdadm.conf
# ARRAY /dev/md0 metadata=1.2 name=ROCKY1:0 UUID=03583924:533e5338:8d363715:09a8b834

[root@rocky7 space]# mdadm --detail --scan >> /etc/mdadm.conf
[root@rocky7 space]# cat /etc/mdadm.conf
ARRAY /dev/md0 metadata=1.2 UUID=f7b6c9c8:4f3e18a2:912f2fac:ddf19789
```

Verify with `df -h` ensure that your /space is mounted

There is no procedure in this lab for breaking down this MDADM RAID.

You are root/administrator on your machine, and you do not care about the data on
this RAID. Can you use the internet/man paegs or other documentation to take this raid
down safely and clear those disks?

Can you document your steps so that you or others could come abck and do this process again?

```
1. rm /etc/mdadm.conf
2. umount /space
3. remove /dev/mapper/VolGroupTest-lv_test line from /etc/fstab
4. run systemctl daemon-reload. if no output, then good.
5. lvremove /dev/mapper/VolGroupTest-lv_test
6. vgremove VolGroupTest
7. pvremote /dev/md0
8. mdadm --stop /dev/md0
9. mdadm --remove /dev/md0
10. mdadm --zero-superblock /dev/xvdb /dev/xvdc /dev/xvde
	- this zeros out the drives to remove data. the prompt said we didn't care about it :D
```