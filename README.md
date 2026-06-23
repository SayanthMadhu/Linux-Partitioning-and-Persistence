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

OUTPUT:
<img width="542" height="336" alt="image" src="https://github.com/user-attachments/assets/18a90b0d-f3da-4e4e-85ab-b8a661be7eab" />




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
OUTPUT :
<img width="707" height="797" alt="image" src="https://github.com/user-attachments/assets/60ad79a2-519e-4393-9db2-e8809cafa54b" />


-------------------------------------------------------------------

STEP 3: Format the Partition with ext3 Filesystem

Format the newly created partition using ext3 filesystem.

Command:

sudo mkfs.ext3 /dev/sdb1

Explanation:

- mkfs.ext3 creates an ext3 filesystem on the partition.
- /dev/sdb1 is the newly created partition.

OUTPUT :
<img width="560" height="202" alt="image" src="https://github.com/user-attachments/assets/b8b3cc42-70c9-4755-8338-af9eee44589b" />


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

OUTPUT :
<img width="712" height="225" alt="image" src="https://github.com/user-attachments/assets/240c4b33-7a32-46fd-aa89-3c633ac9e3ac" />


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

OUTPUT :
<img width="295" height="47" alt="image" src="https://github.com/user-attachments/assets/68f0ffc2-a826-405d-a8fe-65a937792291" />


-------------------------------------------------------------------

STEP 8: Retrieve UUID of Partition

Get the UUID of the partition for permanent mounting.

Command:

sudo blkid /dev/sdb1

OUTPUT :
<img width="877" height="47" alt="image" src="https://github.com/user-attachments/assets/7bcf2f73-271a-4453-836d-d592c567d76f" />




Explanation:

- blkid displays filesystem information.
- Copy the UUID value for use in /etc/fstab.

-------------------------------------------------------------------

STEP 9: Edit /etc/fstab File

Open the fstab file.

Command:

sudo nano /etc/fstab

Add the following line at the end of the file:

UUID=a1274c89-b68f-489f-b370-bbdf30a0f44d   /mnt/mypartition   ext3   defaults   0   0

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
OUTPUT:
<img width="600" height="67" alt="image" src="https://github.com/user-attachments/assets/4caef233-2ea2-4a29-8896-0751454dd1b0" />


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

Output:

<img width="667" height="277" alt="image" src="https://github.com/user-attachments/assets/62b7c552-3cf8-4052-b699-34ae8fc0d441" />

This confirms the partition mounted automatically after reboot.

-------------------------------------------------------------------

STEP 13: Verify Address File Exists After Reboot

Check whether the address file still exists.

Command:

ls /mnt/mypartition

OUTPUTt:
<img width="365" height="47" alt="image" src="https://github.com/user-attachments/assets/5a4baca5-88bd-467a-8e5b-2c774eea9924" />



View file contents.

Command:

cat /mnt/mypartition/address

OUTPUT:
<img width="396" height="107" alt="image" src="https://github.com/user-attachments/assets/c5baa5c8-49a3-4e2c-b1b5-d5bc28e31059" />



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

Conclusion

Successfully created a new partition, formatted it with ext3 filesystem, mounted it to /mnt/mypartition, created a file containing a fake address, configured automatic mounting using /etc/fstab, and verified that the data persists after reboot.
