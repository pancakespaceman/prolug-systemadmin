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
Echo “this is a string of text” >> somefile
# Repeat 3 times
cat somefile
# How many lines are there?
cheat with `cat somefile | wc -l`
echo “this is our other test text” >> somefile
# Repeat 3 times
cat somefile | nl
# How many lines are there?
cat somefile | nl | grep test
# compare that with 14
cat somefile | grep test | nl
```
If you want to preserve positional lines in file (know how much you’ve cut out when
you grep something, or generally be able to find it in the unfiltered file for context,
always | nl | before your grep


**Pre-Lab - Disk Speed Tests:**

When using the ProLUG lab environment, you should always check that there are no 
other users on the system w or who.

After this, you may want to check the current state of the disks, as they retain their 
information even after a reboot resets the rest of the machine. lsblk /dev/xvda.

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

**Write tests (saving off write data - rename /tmp/file each time):**

```
# Check /dev/xvda for a filesystem
blkid /dev/xvda

# If it does not have one, make one
mkfs.ext4 /dev/xvda
mkdir /space #(If you don’t have it. Lab will tell you to later as well)

mount /dev/xvda /space
```

**Write Test:**

```
for i in `seq 1 10`; do time dd if=/dev/zero of=/space/testfile$i bs=1024k count=1000 | tee -a /tmp/speedtest1.basiclvm; done
```

**Read Tests:**
```
for i in `seq 1 10`; do time dd if=/space/testfile$i of=/dev/null; done
```

**Cleanup:**
```
for i in `seq 1 10`; do rm -rf /space/testfile$i; done
```

If you are re-creating a test without blowing away the filesystem, change the name
or counting numbers of testfile because that’s the only way to be sure there is 
not some type of filesystem caching going on to optimize. 
This is especially true in SAN write tests.


## Lab

start in root (#); cd /root

**LVM explanation and use within the system:**
```
# Check physical volumes on your server (my output may vary)
[root@rocky1 ~]fdisk -l | grep -i xvd

Disk /dev/xvda: 15 GiB, 16106127360 bytes, 31457280 sectors
Disk /dev/xvdb: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Disk /dev/xvde: 3 GiB, 3221225472 bytes, 6291456 sectors
```

**Looking at Logical Volume Management:**

Logical Volume Management is an abstraction layer that looks a lot like how we 
carve up SAN disks for storage management. We have Physical Volumes that get grouped
up into Volume Groups. We carve Volume Groups up to be presented as Logical Volumes.







