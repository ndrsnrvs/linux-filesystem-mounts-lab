# linux-filesystem-mounts-lab
Linux lab that demonstrates how Linux discovers, prepares, mounts, verifies, automates, and optionally abstracts a filesystem instance into the root (/) filesystem hierarchy.

# Process


## Understand the Current Environment
I had the foresight coming into this to be generally aware that manipulating anything in this lab environment would require me to back up what I was changing should any mistakes occur to the partition table, I have a backup
I can restore from.

Leveraging `sfdisk`'s `-d` dump operation, I was able to save the partition table to my `/vault` folder using `sfdisk -d /dev/sda > sda.partition.table.12-27-2025.txt`. 
## Create Partition
### Before
So to do this in my research I felt like I had many options to choose from, `fdisk`, `sfdisk`, and `parted` 
were the common utility options.  I learned that `sfdisk` helps with automating the partition creating process which will
be useful for later on in this lab, but I didn't want to go that route initially as this is a learning opportunity.  I found a [stackexchange post](https://unix.stackexchange.com/questions/104238/fdisk-vs-parted) that mentioned that `parted` is useful for drives that are greater than 2TB, and since my virtual drive is just 2GB, I decided to go with `fdisk`. 

![Figure 1: Before Partition Being Created](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/Before%20Creating%20Partition.png)

Using `fdisk`'s list option, I am able to check the partition table of my VM and I can verify that no partition table entry exists for a new virtual hard drive/disk, so I need to create and attach this virtual drive to my VM.

#### Create Virtual Memory
In the VirtualBox Manager GUI, I attached a `.vmdk` hard disk file to my VM.

### After

Using `fdisk`'s menu navigation tools, I was able to successfully create a partition that exists at `/dev/sdb` accepting all default configuration settings for the virtual disk. 

![Figure 2: After Partition Being Created](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/After%20Creating%20Partition.png)

Examining `fdisk`'s list option again, I am able to verify that the virtual disk has been partitioned by the "Disk /dev/sdb: 2GiB" signifying that the virtual storage device itself has been recognized, along with 
the `dev/sdb1` entry which signifies the newly created partition. 

The next step is to assign a filesystem to the partition.

## Assign Filesystem
### Before

![Figure 3: Before Assigning Filesystem](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/Before%20Assigning%20Filesystem%20to%20Partition.png)

To demonstrate that the filesystem at the partition hasn't been assigned yet, one way to show that is to use `lsblk` utility to see what source filesystems are assigned to which partitions. Note that my `/dev/sdb1` partition has no entry metadata because I haven't assigned the filesystem type to it just yet.

Another way to check, if I were to use the `mount` utility, the util would error out and say that there isn't a filesystem at the `/dev/sdb1` location. 

### After
Using `mkfs -t ext4 /dev/sdb1` to assign the filesystem type ext4 to `/dev/sdb1` now gives the root `/` directory the ability to map it along its tree. 

![Figure 4: After Assigning Filesystem to Partition](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/After%20Assigning%20Filesystem%20to%20Partition.png)

Notice now in `lsblk` we're able to see the filesystem metadata assigned to `/dev/sdb1`, and the filesystem has yet to be mounted to a directory.

## Mount Filesystem to Directory

### Before

So I've assigned a filesystem to a partition, but can Linux recognize all the work done up until this point? The answer is NO!  The filesystem is not mounted to any directory along the root (`/`) directory.

![Figure 5: Before Mounting Filesystem to Directory](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/Before%20Mounting%20Filesystem%20to%20Directory.png)

To prove this notice when using `findmnt` utility there isn't a `dev/sdb1` filesystem entry along the root (`/`) because no directory has been given access to this new filesystem space. Also in the previous figure, after assigning the filesystem, the `lsblk` output shows sdb1 isn't mounted anywhere so it makes sense that the root directory tree isn't able to see this new filesystem.

### After

![Figure 6: After Mounting Filesystem to Directory](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/After%20Mounting%20Filesystem%20to%20Directory.png)

Using the `mount` utilty, `mount /dev/sdb1 /secondstorage` our filesystem is now mounted at a directory and Linux is now able to see its relationship relative to the root (`/`) directory.

# Process - Visual

I wanted to create a visual using Obsidian's canvas feature that captures the jist of what I've done here in this lab.  I like how it assigns purpose and frame to the `lsblk` and `findmnt` commands and where one would use it along in this process, whether to gain insight from a block device's point-of-view or from the mounted directory's point-of-view.

![](https://github.com/ndrsnrvs/linux-filesystem-mounts-lab/blob/main/src/Process%20-%20Visual.png)

# Next Steps...

I plan on leveraging `sfdisk` to automate the creation of the partition, I'm still brainstorming different ways to capture more of the workflow processs here in a future automation script.
