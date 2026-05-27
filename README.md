# Linux-Partitioning-and-Persistence
# Linux Partitioning and Persistence Assignment

## Objective

The objective of this assignment is to create a new partition with an ext3 filesystem, mount it to /mnt/mypartition, create a file containing a fake address, and ensure the partition and data persist after a system reboot.

-------------------------------------------------------------------

STEP 1: Identify Available Disk

First, identify the available disk using the following command.

Command:

sudo fdisk -l

Explanation:

- fdisk -l lists all available disks and partitions in the system.
- From the output, select an unused disk such as /dev/sdb.

Example Output:

Disk /dev/sdb: 10 GiB

Here /dev/sdb is selected for partition creation.

-------------------------------------------------------------------

STEP 2: Create a New Partition

Create a new partition on the selected disk.

Command:

sudo fdisk /dev/sdb

Inside fdisk type the following commands one by one:

n
p
1
Enter
Enter
w

Explanation:

- n → Create a new partition
- p → Create a primary partition
- 1 → Partition number
- Enter → Default first sector
- Enter → Default last sector
- w → Save changes and exit

After this, the new partition /dev/sdb1 will be created.

-------------------------------------------------------------------

STEP 3: Format the Partition with ext3 Filesystem

Format the newly created partition using ext3 filesystem.

Command:

sudo mkfs.ext3 /dev/sdb1

Explanation:

- mkfs.ext3 creates an ext3 filesystem on the partition.
- /dev/sdb1 is the newly created partition.

Example Output:

Creating filesystem with 2621440 4k blocks
Filesystem UUID: 1234-abcd

-------------------------------------------------------------------

STEP 4: Create Mount Point Directory

Create a directory where the partition will be mounted.

Command:

sudo mkdir -p /mnt/mypartition

Explanation:

- mkdir creates a directory.
- -p ensures the full path is created if it does not already exist.

-------------------------------------------------------------------

STEP 5: Mount the Partition

Mount the partition to the mount point.

Command:

sudo mount /dev/sdb1 /mnt/mypartition

Explanation:

- mount attaches the filesystem to the directory /mnt/mypartition.

Verify Mount:

df -h

Example Output:

/dev/sdb1   9.8G   24M   9.2G   1%   /mnt/mypartition

This confirms the partition is mounted successfully.

-------------------------------------------------------------------

STEP 6: Create a File with Fake Address

Create a file named address inside the mounted partition.

Command:

sudo nano /mnt/mypartition/address

Add the following content:

Aryasree
Green Villa
Kannur, Kerala
PIN: 670001
India

Save the file using:

CTRL + O
ENTER
CTRL + X

Explanation:

- nano opens a text editor.
- The file address is created inside /mnt/mypartition.

-------------------------------------------------------------------

STEP 7: Verify File Creation

Check whether the file exists.

Command:

ls /mnt/mypartition

Output:

address

-------------------------------------------------------------------

STEP 8: Retrieve UUID of Partition

Get the UUID of the partition for permanent mounting.

Command:

sudo blkid /dev/sdb1

Example Output:

/dev/sdb1: UUID="1234-abcd-5678-efgh" TYPE="ext3"

Explanation:

- blkid displays filesystem information.
- Copy the UUID value for use in /etc/fstab.

-------------------------------------------------------------------

STEP 9: Edit /etc/fstab File

Open the fstab file.

Command:

sudo nano /etc/fstab

Add the following line at the end of the file:

UUID=1234-abcd-5678-efgh   /mnt/mypartition   ext3   defaults   0   0

Explanation:

- UUID identifies the partition uniquely.
- /mnt/mypartition is the mount location.
- ext3 specifies the filesystem type.
- defaults uses default mount options.
- 0 0 disables dump and filesystem check order.

Save and exit the file.

-------------------------------------------------------------------

STEP 10: Test fstab Configuration

Before rebooting, verify the configuration.

Command:

sudo mount -a

Explanation:

- mount -a mounts all filesystems listed in /etc/fstab.
- If there is no error, the configuration is correct.

-------------------------------------------------------------------

STEP 11: Reboot the System

Restart the system.

Command:

sudo reboot

-------------------------------------------------------------------

STEP 12: Verify Automatic Mount After Reboot

After reboot, check whether the partition is mounted automatically.

Command:

df -h

Example Output:

/dev/sdb1   9.8G   24M   9.2G   1%   /mnt/mypartition

This confirms the partition mounted automatically after reboot.

-------------------------------------------------------------------

STEP 13: Verify Address File Exists After Reboot

Check whether the address file still exists.

Command:

ls /mnt/mypartition

Output:

address

View file contents.

Command:

cat /mnt/mypartition/address

Output:

Aryasree
Green Villa
Kannur, Kerala
PIN: 670001
India

This confirms the data persists after reboot.

-------------------------------------------------------------------

Commands Used:

sudo fdisk -l

sudo fdisk /dev/sdb

sudo mkfs.ext3 /dev/sdb1

sudo mkdir -p /mnt/mypartition

sudo mount /dev/sdb1 /mnt/mypartition

sudo nano /mnt/mypartition/address

ls /mnt/mypartition

sudo blkid /dev/sdb1

sudo nano /etc/fstab

sudo mount -a

sudo reboot

df -h

cat /mnt/mypartition/address

-------------------------------------------------------------------

Contents of /etc/fstab

UUID=1234-abcd-5678-efgh   /mnt/mypartition   ext3   defaults   0   0

-------------------------------------------------------------------

Output of ls /mnt/mypartition After Reboot

address

-------------------------------------------------------------------

Conclusion

Successfully created a new partition, formatted it with ext3 filesystem, mounted it to /mnt/mypartition, created a file containing a fake address, configured automatic mounting using /etc/fstab, and verified that the data persists after reboot.
