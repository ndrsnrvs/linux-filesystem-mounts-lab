# linux-filesystem-mounts-lab
Linux lab that demonstrates how Linux discovers, prepares, mounts, verifies, automates, and optionally abstracts a filesystem instance into the root (/) filesystem hierarchy.

# Process


## Understand the Current Environment
I had the foresight coming into this to be generally aware that manipulating anything in this lab environment would require me to back up what I was changing should any mistakes occur to the partition table, I have a backup
I can restore from.

Leveraging `sfdisk`'s `-d` dump operation, I was able to save the partition table to my `/vault` folder using `sfdisk -d /dev/sda > sda.partition.table.12-27-2025.txt`. 
## Create Virtual Memory
In the VirtualBox Manager GUI, I attached a `.vmdk` hard disk file to my VM.
## Create Partition
### Before
So to do this in my research I felt like I had many options to choose from, `fdisk`, `sfdisk`, and `parted` 
were the common utility options.  I learned that `sfdisk` helps with automating the partition creating process which will
be useful for later on in this lab, but I didn't want to go that route initially as this is a learning opportunity.  I found a [stackexchange post](https://unix.stackexchange.com/questions/104238/fdisk-vs-parted) that mentioned that `parted` is useful for drives that are greater than 2TB, and since my virtual drive is just 2GB, I decided to go with `fdisk`. 

## Create Filesystem
## Mount filesystem to Directory
