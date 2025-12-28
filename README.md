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

## Create Filesystem

## Mount filesystem to Directory

# Process - Visual
