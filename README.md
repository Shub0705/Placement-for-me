# Disk Partitioning and File Systems in Linux

## 1. What is a Partition?
A **partition** is a contiguous set of blocks on a drive, treated as an independent disk.

---

## 2. What is Partitioning?
**Partitioning** involves dividing a single hard drive into multiple logical drives.

---

## 3. Why Have Multiple Partitions?
- **Data Encapsulation:** Limits file system corruption to a single partition, protecting data.
- **Disk Space Efficiency:** Allows formatting with different block sizes, reducing disk wastage.
- **Data Growth Management:** Enables disk quotas to control and limit data growth.

---

## 4. Structure of a Disk Partition
- The first sector of the disk contains the **Master Boot Record (MBR)** (512 bytes).
  - **Initial Program Loader (IPL):** 446 bytes, contains the Secondary Boot Loader for booting the OS.
  - **Partition Table Information (PTI):** Stores details about the number, size, and types of partitions.

---

## 5. Disk Partition Criteria
- A disk can have a maximum of **4 partitions**:
  - **3 Primary Partitions**
  - **1 Extended Partition** (further divided into logical partitions).
- **Primary Partition:** Used for MBR and OS installation.
- **Extended Partition:** Allows creating additional logical partitions.

---

## 6. Disk Identification in Linux
Disks are identified using different naming conventions:
- **IDE Drives:** `/dev/hda`, `/dev/hdb`, `/dev/hdc`, etc. (Partitions: `/dev/hda1`, `/dev/hda2`, etc.)
- **iSCSI/SCSI/SATA Drives:** `/dev/sda`, `/dev/sdb`, `/dev/sdc`, etc. (Partitions: `/dev/sda1`, `/dev/sda2`, etc.)
- **Virtual Drives:** `/dev/vda`, `/dev/vdb`, `/dev/vdc`, etc. (Partitions: `/dev/vda1`, `/dev/vda2`, etc.)

### Abbreviations:
- **IDE:** Integrated Drive Electronics
- **SCSI:** Small Computer System Interface
- **iSCSI:** Internet Small Computer System Interface

---

## 7. What is a File System?
A **file system** organizes and stores data on the disk. Every partition (except MBR and Extended partitions) must have a file system assigned for data storage. File systems are applied by formatting the partition.

---

## 8. Types of File Systems in Linux
### Commonly Supported File Systems:
- **ext2**, **ext3**, **ext4:** Widely used in RHEL-6.
- **xfs:** Introduced in RHEL-7.
- **vfat:** Provides compatibility between Linux and Windows.
- **cdfs:** Used for mounting CD-ROMs.
- **hdfs:** Used for mounting DVDs.
- **iso9660:** Reads CD/DVD `.iso` image files.

# Disk Partitioning and File Systems in Linux

## 8. What are the different types of file systems supported in Linux?
The Linux-supported file systems are ext2, ext3, ext4, xfs, vfat, cdfs, hdfs, iso9660, etc.

- **ext2, ext3, ext4**: Widely used in RHEL-6.
- **xfs**: Introduced in RHEL-7.
- **vfat**: Maintains common storage between Linux and Windows.
- **cdfs**: Used to mount CD-ROMs.
- **hdfs**: Used to mount DVDs.
- **iso9660**: Reads CD/DVD `.iso` image format files in Linux.

## 9. What is mounting, and how many types of mounting are there?
Mounting refers to attaching a directory to the file system to access a partition and its file system. Generally, subdirectories under `/mnt` are the mount points. There are two types of mountings:

- **Temporary Mounting**:
  - Mount lasts only until the system is rebooted.
  - Example: `# mount <options> <device name> <directory name>`

- **Permanent Mounting**:
  - Requires an entry in `/etc/fstab` for persistence across reboots.
  - Example entry: `<device name> <mount point> <file system type> <mount options> <backup option> <fsck value>`
  - Example command: `# mount -a` (mount partitions without reboot).

## 10. Differences between ext2, ext3, ext4, and xfs file systems

| S.No. | Feature                       | ext2                       | ext3                      | ext4                      | xfs                      |
|-------|-------------------------------|----------------------------|---------------------------|---------------------------|--------------------------|
| 1     | Stands for                   | Second Extended File System | Third Extended File System | Fourth Extended File System | Extended File System    |
| 2     | Journaling Feature          | No                         | Yes                       | Yes                       | Yes                      |
| 3     | Max. File Size              | 16 GB to 2 TB              | 16 GB to 2 TB             | 16 GB to 16 TB            | 16 GB to 8 EB            |
| 4     | Max. File System Size       | 2 TB to 32 TB              | 2 TB to 32 TB             | 2 TB to 1 EB              | 2 TB to 16 EB            |
| 5     | Conversion                  | Cannot convert to ext2     | Convert ext2 to ext3      | Convert any ext to ext4   | Requires unmount/remount |

## 11. Files related to mounting in Linux
- **/etc/mtab**: Stores information on currently mounted file systems (dynamic).
- **/etc/fstab**: Stores permanent mount points.
  - Entry format: `<device name> <mount point> <file system type> <mount options> <backup> <fsck value>`

## 12. How to create different types of partitions
```bash
# fdisk -l
# fdisk /dev/sdc
Command (m for help): n (create new partition)
(p - primary, e - extended): p
First cylinder: (Enter for default)
Last cylinder: +<size in KB/MB/GB/TB>
Command (m for help): t (change partition ID, e.g., 8e for LVM)
Command (m for help): w (save changes)
# partprobe /dev/sdc1 (update partition table)
```

## 13. How to create a file system in Linux
```bash
# mkfs.<file system type> <device name>
Example: # mkfs.ext4 /dev/sdc1
```

## 14. How to mount file systems
- **Temporary Mount**:
  ```bash
  # mkdir /mnt/oracle
  # mount /dev/sdc1 /mnt/oracle
  ```
- **Permanent Mount**:
  ```bash
  # vim /etc/fstab
  /dev/sdc1 /mnt/oracle xfs defaults 0 0
  # mount -a
  ```

## 15. How to delete a partition
```bash
# fdisk /dev/sdc
Command (m for help): d (delete partition)
Partition number: (specify partition number)
Command (m for help): w (save changes)
# partprobe /dev/sdc1 (update partition table)
```

## 16. Partitions not mounting despite entries in /etc/fstab
1. Check for incorrect entries in `/etc/fstab`.
2. Unmount all partitions: `# umount -a`.
3. Mount all partitions: `# mount -a`.

## 17. Troubleshooting unmount issues
Reasons a directory cannot unmount:
1. Current directory is the mount point (`# pwd`).
2. Files in use:
   ```bash
   # fuser -cu <device>
   # lsof <device>
   # fuser -ck <file path>
   ```
   After resolving, unmount: `# umount <mount point>`.

## 18. View usage of mounted partitions
```bash
# df -hT
```

## 19. View size of files or directories
```bash
# du -h <file/directory>
# du . | sort -nr | head -n10
# ncdu (requires ncdu package)
```

## 20. Assign a label to a partition
```bash
# e2label <device> <label>
Example: # e2label /dev/sdb1 oradisk
# mount -l (view labels of mounted partitions)
```
## **21. How to mount a partition temporarily or permanently using label?**
 mount LABEL=<label name> <mount point>
Example: # mount LABEL=oradisk /mnt/oracle (to mount the oradisk label on /mnt/oracle directory)
#vim /etc/fstab
LABEL=oradisk /mnt/oracle ext4 defaults 0 0
Esc+:+wq! (to save and exit the file)
 mount -a (to mount the partitions)
 mount (to verify whether it is mounted or not)

## **22. How to mount the partition permanently using block id (UUID)?**
blkid <partition name or disk name> (to see the UUID or block id of that partition)
Example: # blkid /dev/sdb2 (to see the UUID or block id of the /dev/sdb2 partition)
Copy that UUID with mouse and paste it in /etc/fstab file and make an entry about that.
Example: # vim /etc/fstab
UUID="{.......................}" /mnt/oracle ext4 defaults 0 0
Esc+:+wq! (to save and exit)

## **23. What is the basic rule for swap size?**
(i) If the size of the RAM is less than or equal to 2GB, then the size of the swap = 2 x RAM size.
(ii) If the size of the RAM is more than 2GB, then the size of the swap = 2GB + RAM size.

## **24. How to create a swap partition and mount it permanently?**
#free -m (to see the present swap size)
#swapon -s (to see the swap usage)
#fdisk <disk name> (to make a partition)
Example: # fdisk /dev/sdb
Command (m for help): n (to create a new partition)
First cylinder: (press Enter to take as default value)
Last cylinder: +2048M (to create 2GB partition)
Command (m for help): t (to change the partition id)
Enter the partition No.: 2 (to change the /dev/sdb2 partition id)
Enter the id: 82 (to change the partition id Linux to Linux Swap)

Command (m for help): w (to save the changes into the disk)
 partprobe /dev/sdb (to update the partition table information)
 mkswap <device or partition name> (to format the partition with swap file system)
Example: # mkswap /dev/sdb2 (to format the /dev/sdb2 partition with swap file system)
#swapon <device or partition name> (to activate the swap space)
Example: # swapon /dev/sdb2 (to activate /dev/sdb2 swap space)
#free -m (to see the swap size)
#vim /etc/fstab (to make an entry to permanently mount the swap partition)
/dev/sdb2 swap swap defaults 0 0
Esc+:+wq! (to save and exit)

## **25. What are the attributes of the file system?**
(i) Inode number
(ii) File name
(iii) Data block

## **26. What is inode number and what is the use of it?**
Inode numbers are the objects the Linux O/S uses to record the information about the file.
Generally, an inode number contains two parts:
(a) Inode first part contains information about the file, owner, its size, and its permissions.
(b) Inode second part contains a pointer to data blocks associated with the file content.
That's why using the inode number we can get the file information quickly.

## **27. How to check the integrity of a file system or consistency of the file system?**
By running the **# fsck <device or partition name>** command we can check the integrity of the file system.
But before running the **fsck** command, first unmount that partition and then run the **fsck** command.

## **28. What is fsck check or what are the phases of the fsck?**
(a) First it checks blocks and sizes of the file system
(b) Second it checks file system path names
(c) Third it checks file system connectivity
(d) Fourth it checks file system reference counts (nothing but inode numbers)
(e) Finally it checks file system occupied cylindrical groups

## **29. Why should the file system be unmounted before running the fsck command?**
If we run **fsck** on mounted file systems, it leaves the file systems in an unusable state and also deletes the data.
So, before running the **fsck** command the file system should be unmounted.

# **30. Which type of file system problems do you face?**
(i) File system full
(ii) File system corrupted

## **31. How to extend the root file system which is not on LVM?**
By using the **# gparted** command we can extend the root partition; otherwise, we cannot extend the file systems which are not on LVM.

## **32. How to unmount a file system forcefully?**
 umount -f <mount point>
fuser -ck <mount point>

## **33. How to know the file system type?**
df -hT (command gives the file system type information)

## **34. How to know which file system occupies more space and the top 10 file systems?**
 df -h <device or partition name> | sort -r | head -10

## **35. What is the command to know the mounted file systems?**
 mount or # cat /etc/mtab

## **36. How to know whether the file system is corrupted or not?**
First unmount the file systems and then run the **fsck** command on that file system.

## **37. How to recover if a file system is corrupted or crashed?**
If the normal or not related to O/S file system is corrupted, first unmount that file system and run the **fsck** command on that file system. If the O/S related file system is corrupted, then boot the system with CDROM in single-user mode and run the **fsck** command.
If the normal or not related to O/S file system is crashed, then restore it from the recent backup. If the O/S related file system is crashed, then boot the system with CDROM in single-user mode and restore it from the recent backup.

## **38. How to create a file with a particular size?**
dd if=/dev/zero of=/orafile bs=1MB count=500 (to create 500MB size /orafile with 4KB block size)

## **39. How to find how many disks are attached to the system?**
fdisk -l (to see how many disks are attached to the system)

## **40. What is journaling?**
It is a dedicated area in the file system where all the changes are tracked when the system crashes. So the possibility of the file system corruption or crashes is less because of this journaling feature.

# Linux Filesystem and Superblock Management

### **41. How to repair the Superblock of the file system?**
If an input/output error occurs when storing data on the hard disk, the Superblock of the file system may become corrupted. To repair or restore the Superblock:

```bash
# umount <file system mount point>                # Unmount the file system
# dumpe2fs </dev/vgname/lvname> | grep superblock # List the Superblocks (primary and secondary)
# e2fsck -b <secondary_superblock> </dev/vgname/lvname> # Restore the damaged Superblock
# mount -a                                        # Mount the file system
```

### **42. How to create the file systems with user-specified Superblock reserve space?**
By default, 5% of a partition is reserved for the Superblock. To create a file system with custom reserve space:

```bash
# mkfs.ext4 -m <percentage> <partition name>
```

### **43. How to modify the Superblock reserve space?**
To adjust the reserved space percentage:

```bash
# tune2fs -m <percentage> <partition name>
```

---

### **Important Commands:**

1. **Filesystem Consistency Checks:**
    ```bash
    # fsck <partition name>          # Check the consistency of the file system
    # e2fsck <partition name>        # Check the consistency in interactive mode
    # e2fsck -p <partition name>     # Check the consistency without interactive mode
    # fsck -AR -y                    # Run fsck without asking any questions
    ```

2. **Filesystem Formatting and Metadata:**
    ```bash
    # mke2fs -n <partition name>       # View Superblock information
    # mke2fs -t <file system type> <partition name>  # Format in a specified file system type
    # blockdev --getbs /dev/sdb1       # Check the block size of /dev/sdb1
    ```

3. **Filesystem Mounting and Unmounting:**
    ```bash
    # umount -a                        # Unmount all file systems except the root
    # mount -a                         # Mount all file systems with entries in /etc/fstab
    ```

4. **Filesystem Tuning:**
    ```bash
    # tune2fs -l /dev/sdb1             # Check if journaling is enabled
    # tune2fs -j /dev/sdb1             # Convert ext2 to ext3 (add journaling)
    # tune2fs -O dir_index,has_journal,unit_bg /dev/sdb1 # Convert ext2 to ext4
    ```

5. **Filesystem Usage and Repair Tools:**
    ```bash
    # fuser -cu <device or partition name>  # See users accessing the file system
    # fuser -cK <device or partition name>  # Kill user processes accessing the file system
    ```

---

### **Journalctl Commands**

- **View Logs:**
    ```bash
    # journalctl -n 5                     # Display last five lines of logs
    # journalctl -p err                   # Display error messages
    # journalctl --since "date" --until "date" # View logs between two dates
    ```

- **Change Log Directory:**
    ```bash
    # mkdir -p /var/log/journal
    # chown root:systemd-journal /var/log/journal
    # chmod g+s /var/log/journal
    # killall -USR1 systemd-jjournald
    
# Logical Volume Management (LVM) Operations

## 6. How to See the Details of the Logical Volumes?
- **Commands:**
  - `# lvs` - Displays all logical volumes with less details.
  - `# lvdisplay` - Displays all logical volumes with more details.
  - `# lvdisplay <LV name>` - Displays the specified logical volume details.
  - `# lvscan` - Scans all logical volumes.
  - `# lvscan <LV name>` - Scans the specified logical volume.

---

## 7. How to Extend the Volume Group?
- **Steps:**
  1. Create a new partition using `# fdisk` and set the partition ID to **8e**.
  2. Save changes and update the partition table using `# partprobe`.
  3. Create a physical volume using `# pvcreate`.
  4. Add the partition to the volume group using `# vgextend`.

- **Example:**
  ```bash
  # fdisk /dev/sdb
  Command (m for help): n
  First cylinder: [Press Enter for default]
  Last cylinder: +500M
  Command (m for help): t
  Select the partition: [Type partition number]
  Specify the Hexa code: 8e
  Command (m for help): w
  # partprobe /dev/sdb
  # pvcreate /dev/sdb
  # vgextend <VG name> /dev/sdb
  # vgdisplay <VG name>
  ```

---

## 8. How to Extend the Logical Volume and Update Its File System?
- **Steps:**
  1. Check current size with `# lvdisplay` and `# df -hT`.
  2. Increase size using `# lvextend` or `# lvresize`.
  3. Update the file system using `# resize2fs` or `# xfs_growfs`.

- **Example:**
  ```bash
  # df -hT
  # lvextend -L +<size in MB> /dev/<vgname>/<lvname>
  # resize2fs /dev/<vgname>/<lvname>
  # lvdisplay /dev/<vgname>/<lvname>
  # df -hT
  ```

---

## 9. How to Reduce the Logical Volume and Update the File System?
- **Steps:**
  1. Unmount the file system using `# umount`.
  2. Check file system consistency using `# e2fsck`.
  3. Reduce the logical volume using `# lvreduce`.
  4. Update the file system using `# resize2fs`.
  5. Remount the file system.

- **Example:**
  ```bash
  # umount <file system mount point>
  # e2fsck <device or partition name>
  # lvreduce -L -<size in MB> /dev/<vgname>/<lvname>
  # resize2fs /dev/<vgname>/<lvname>
  # lvdisplay /dev/<vgname>/<lvname>
  # mount -a
  # df -hT
  ```

---

## 10. How to Move or Migrate the Logical Volume Data from One Physical Volume to Another Physical Volume?
- **Steps:**
  1. Access the mount point of the failing physical volume and check the data.
  2. Verify the size of the physical volume with `# pvs` or `# pvdisplay`.
  3. Unmount the file system of that physical volume with `# umount`.
  4. Add a new physical volume (ensure it is the same size or larger).
  5. Migrate data using `# pvmove`.
  6. Mount back the logical volume and verify the data.
  7. Remove the failed physical volume using `# vgreduce`.

- **Example:**
  ```bash
  # cd <file system mount point>
  # ls
  # pvs <pvname> or # pvdisplay <pvname>
  # umount <file system mount point>
  # pvcreate <device or partition name>
  # vgextend <vgname> <pvname>
  # pvmove <old pvname> <new pvname>
  # mount -a
  # vgreduce <vgname> <failed pvname>
  # cd <file system mount point>
  # ls
  
  # Logical Volume Management (LVM) Questions and Commands

### **11. How to delete or remove the logical volume?**
- Steps:
  1. Unmount the file system: `# umount <mount point>`
  2. Remove the entry in `/etc/fstab` file.
  3. Remove the logical volume: `# lvremove </dev/vgname/lvname>`
  4. Verify the logical volume removal: `# lvs` or `# lvdisplay`

**Example**:
```bash
# umount <file system mount point>
# vim /etc/fstab
# lvremove </dev/vgname/lvname>
# lvs or # lvdisplay
```

---

### **12. How to delete or remove the volume group?**
- Steps:
  1. Ensure no logical volumes are mounted.
  2. Delete the volume group: `# vgremove <vgname>`
  3. Verify volume group removal: `# vgs` or `# vgdisplay`

**Example**:
```bash
# umount <file system mount point>
# vim /etc/fstab
# vgremove <vgname>
# vgs or # vgdisplay
```

---

### **13. How to delete or remove the physical volume?**
- Steps:
  1. Ensure the physical volume is not part of any volume group.
  2. Delete the physical volume: `# pvremove <pvname>`
  3. Verify physical volume removal: `# pvs` or `# pvdisplay`

**Example**:
```bash
# pvremove <pvname>
# pvs or # pvdisplay
```

---

### **14. How to restore the volume group which is removed mistakenly?**
- Steps:
  1. Unmount the file system: `# umount <file system mount point>`
  2. Check the volume group backup list: `# vgcfgrestore --list <volume group name>`
  3. Remove the logical volume: `# lvremove </dev/vgname/lvname>`
  4. Restore the volume group from backup:
     ```bash
     # vgcfgrestore -f <backup file name> <volume group name>
     ```
  5. Check the status of the volume group: `# vgscan`
  6. Check the status of the logical volume: `# lvscan`
  7. Activate the volume group: `# vgchange -ay <volume group name>`
  8. Activate the logical volume: `# lvchange -ay <logical volume name>`
  9. Mount the logical volume file system: `# mount -a`

**Example**:
```bash
# umount <file system mount point>
# vgcfgrestore --list <volume group name>
# lvremove </dev/vgname/lvname>
# vgcfgrestore -f <backup file> <volume group name>
# vgscan
# lvscan
# vgchange -ay <volume group name>
# lvchange -ay <logical volume name>
# mount -a
```

---

### **15. How to change the volume group name and other parameters?**
- Rename the volume group: `# vgrename <existing volume group name> <new volume group name>`
- Limit the maximum number of logical volumes:
  ```bash
  # vgchange -l <number> <volume group>
  ```
- Limit the maximum number of physical volumes:
  ```bash
  # vgchange -p <number> <volume group>
  ```
- Change the block size:
  ```bash
  # vgchange -s <block size> <volume group>
  ```

**Examples**:
```bash
# vgchange -l 2 <vgname>
# vgchange -p 2 <vgname>
# vgchange -s 4 <vgname>
```

---

### **16. How to change the logical volume name and other parameters?**
- Rename the logical volume: `# lvrename <existing lvname> <new lvname>`
- Set the logical volume to read-only: `# lvchange -pr <logical volume>`
- Set the logical volume to read-write: `# lvchange -prw <logical volume>`

**Example**:
```bash
# lvrename <existing lvname> <new lvname>
# lvchange -pr <logical volume>
# lvchange -prw <logical volume>
```

---

### **17. How to disable the volume group and logical volume?**
- Disable the volume group: `# vgchange -an <volume group>`
- Disable the logical volume: `# lvchange -an <logical volume>`

**Example**:
```bash
# vgchange -an <volume group>
# lvchange -an <logical volume>
```

---

### **18. How to take a backup of the volume group?**
- Take a backup of all volume groups: `# vgcfgbackup`
- Take a backup of a specific volume group: `# vgcfgbackup <volume group>`

**Example**:
```bash
# vgcfgbackup
# vgcfgbackup <volume group>
```

---

### **19. What is the configuration file of the logical volume?**
- View the configuration file: `# cat /etc/lvm/lvm.conf`

**Example**:
```bash
# cat /etc/lvm/lvm.conf
```

---

### **20. What are the locations of the logical volume and volume groups?**
- Logical volume backup location: `/etc/lvm/backup`
- Volume group backup location: `/etc/lvm/archive`

**Example**:
```bash
# cd /etc/lvm/backup
# cd /etc/lvm/archive
```

---

### **21. How to know the current version of the LVM package?**
- Command: `# rpm -qa lvm*`

**Example**:
```bash
# rpm -qa lvm*
```

---

### **22. What are the attributes of the volume group?**
- Command: `# vgs`
- Check UUID of the volume group: `# vgs -v`

**Attributes**:
- `w` - Writable
- `z` - Extendable
- `n` - Normal

**Example**:
```bash
# vgs
# vgs -v

## 23. How to Extend the Logical Volume to Maximum Disk Space and Half of the Disk Space

### Maximum Disk Space:
```bash
lvextend -l +100%FREE <logical_volume>
```
This command extends the logical volume by adding the volume group's total available space.

### Half of the Disk Space:
```bash
lvextend -l 50%<vgname><lvname>
```
This command extends the logical volume by adding 50% of the free space from the volume group.

---

## 24. How to Check on Which Physical Volume the Data is Writing in the Logical Volume

### All Logical Volumes:
```bash
lvdisplay -m
```
### Specific Logical Volume:
```bash
lvdisplay -m <lvname>
```

---

## 25. Types of File Systems Available
- `ext2`: Second extended file system (default in RHEL 3 & 4)
- `ext3`: Third extended file system (default in RHEL 5)
- `ext4`: Fourth extended file system (default in RHEL 6)
- `xfs`: Extended file system (default in RHEL 7)
- `ufs`: Unix file system (default in Solaris)
- `jfs`: Journal file system (default in IBM-AIX)
- `hfs`: High performance file system (default in HP-UX)
- `vxfs`: Veritas file system
- `procfs`: Process file system (temporary)
- `tempfs`: Temporary file system (temporary)
- `cdfs`: Compact disk file system
- `hdfs`: DVD file system
- `iso9660`: To read CD/DVD `.iso` image files in Linux

---

## 26. How to Scan and Detect the LUNs Over the Network

1. Check available fiber channels:
   ```bash
   ls /sys/class/fc_host
   ```
2. Scan and detect the LUNs:
   ```bash
   echo "---" > /sys/class/scsi_host/<lun_no>/scan
   ```

---

## 27. How to Mount a Pen Drive in Linux

1. Identify the pen drive name:
   ```bash
   lsusb
   fdisk -l
   ```
2. Create a mount point:
   ```bash
   mkdir /mnt/pendrive
   ```
3. Mount the pen drive:
   ```bash
   mount <pen_drive_name> <mount_point>
   ```
4. Access the pen drive:
   ```bash
   cd /mnt/pendrive
   ```

---

## 28. How to Mount a CD/DVD ROM Drive in Linux

1. Create a mount point:
   ```bash
   mkdir /mnt/mycdrom
   ```
2. Mount the CD/DVD:
   ```bash
   mount /dev/cdrom /mnt/mycdrom
   ```
3. Access the CD/DVD:
   ```bash
   cd /mnt/mycdrom
   ```

---

## 29. How to Mount `.iso` Image Files in Linux

### Mount the `.iso` File:
```bash
mount -t iso9660 /root/rhel6.iso /iso -o ro,loop
```
### Burn the `.iso` File to CD/DVD:
```bash
cdrecord /root/Desktop/rhel6.iso
```
### Eject and Insert the CD/DVD Tray:
```bash
eject
eject -t
```

---

## 30. What is RAID?

**RAID** stands for Redundant Array of Independent Disks. It provides fault tolerance and load balancing using concepts of stripping, mirroring, and parity.

### Types of RAID:
1. Hardware RAID (vendor-dependent, expensive)
2. Software RAID (maintained by system administrators, less expensive)

---

## 31. Types of Software RAIDs and Their Requirements

- **RAID-0**: Stripping (Minimum 2 disks required)
- **RAID-1**: Mirroring (Minimum 2 disks required)
- **RAID-10 (1+0)**: Mirroring + Stripping (Minimum 4 disks required)
- **RAID-01 (0+1)**: Stripping + Mirroring (Minimum 4 disks required)
- **RAID-5**: Stripping with parity (Minimum 3 disks required)

---

## 32. How to Configure RAID-0 in Linux

### RAID-0 Configuration:
- Minimum 2 disks required (partition ID: `fd`).
- High performance but no redundancy.

#### Steps:
1. Create RAID-0:
   ```bash
   mdadm -Cv /dev/md0 -n 2 /dev/sdb /dev/sdc -l 0
   ```
2. Verify RAID creation:
   ```bash
   cat /proc/mdstat
   ```
3. Create a file system:
   ```bash
   mkfs.ext4 /dev/md0
   ```
4. Create a mount point:
   ```bash
   mkdir /mnt/raid0
   ```
5. Mount RAID-0:
   ```bash
   mount /dev/md0 /mnt/raid0
   ```
6. View details:
   ```bash
   mdadm -D /dev/md0
   ```
7. Fail and replace disks:
   ```bash
   mdadm /dev/md0 -f /dev/sdb
   mdadm /dev/md0 -r /dev/sdb
   mdadm /dev/md0 -a /dev/sdd
   ```
8. Stop and grow RAID:
   ```bash
   mdadm --stop /dev/md0
   mdadm --grow /dev/md0 --raid_device=3
   ```

---

## 33. How to Configure RAID-1 in Linux

### RAID-1 Configuration:
- Minimum 2 disks required (partition ID: `fd`).
- High availability, redundancy, and fault tolerance.

#### Steps:
1. Create RAID-1:
   ```bash
   mdadm -Cv /dev/md0 -n 2 /dev/sdb /dev/sdc -l 1
   ```
2. Verify RAID creation:
   ```bash
   cat /proc/mdstat
   ```
3. Create a file system:
   ```bash
   mkfs.ext4 /dev/md0
   ```
4. Create a mount point:
   ```bash
   mkdir /mnt/raid1
   ```
5. Mount RAID-1:
   ```bash
   mount /dev/md0 /mnt/raid1
   ```
6. View details:
   ```bash
   mdadm -D /dev/md0
   ```
7. Fail and replace disks:
   ```bash
   mdadm /dev/md0 -f /dev/sdb
   mdadm /dev/md0 -r /dev/sdb
   mdadm /dev/md0 -a /dev/sdd
   ```
8. Stop and grow RAID:
   ```bash
   mdadm --stop /dev/md0
   mdadm --grow /dev/md0 --raid_device=3
   ```

   **34. How to configure RAID - 5 in Linux?**

- To configure RAID - 5, minimum 3 disks are required and the partition id is "fd".
- In every disk, approximately 25-30% of space is reserved for parity.
- Reading and writing are very fast, so it produces high performance.
- This uses the Stripping with parity concept.
- If one disk fails, we can recover the data using the remaining two disks and parity.
- If two disks fail, then we cannot recover the data.
- So, there is no redundancy and fault tolerance in RAID - 5.

**Example:** For example, if the data is 1, 2, 3, 4, 5, and 6, then:

```
1       
3+     
6       
```

```
### 2
### 3
### 5+
### 1+
### 4
### 5
```

Disk - 1 | Disk - 2 | Disk - 3
---------|----------|----------
1        | 2        | 3+
4        | 5+       | 6

If the Disk - 1 is `/dev/sdb`, the Disk - 2 is `/dev/sdc`, and the Disk - 3 is `/dev/sdd`:

```
# mdadm -Cv /dev/md0 -n 3 /dev/sdb /dev/sdc /dev/sdd -l 5 (to create the RAID - 5 using disks - 1, 2, and 3)
# cat /proc/mdstat (to check if the RAID - 5 is created)
# mkfs.ext4 /dev/md0 (to create the ext4 file system on the RAID - 5)
# mkdir /mnt/raid5 (to create the RAID - 5 mount point)
# mount /dev/md0 /mnt/raid5 (to mount RAID - 5 on the mount point)
# mdadm -D /dev/md0 (to see the details of the RAID - 5 partition)
# mdadm /dev/md0 -f /dev/sdb (to fail the disk manually)
# mdadm /dev/md0 -r /dev/sdb (to remove the failed disk)
# mdadm /dev/md0 -a /dev/sde (to add the new disk in place of the failed disk)
# umount /mnt/raid5 (to unmount the RAID file system)
# mdadm --stop /dev/md0 (to stop the RAID - 5 volume)
# mdadm /dev/md0 --add /dev/sdf (to add a fourth disk to the RAID - 5 volume)
# mdadm --grow /dev/md0 --raid-devices=4 (to grow the RAID - 5 file system)
```

**35. What are the main advantages of RAID - 5?**

- RAID - 5 uses Stripping with parity and requires only three disks.
- Because of Stripping, data reading and writing are fast.
- By using parity, we can recover the data if one of the three disks fails.
- The main advantage of RAID - 5 is fast writing, reading, redundancy, and fault tolerance with less expense.

**36. How will you troubleshoot if one of the eight disks failed in LVM?**

1. Unmount the file system.
2. Add a new disk with the same size as the failed disk to the volume group.
3. Move the data from the failed physical volume to the newly added physical volume.
4. Remove the failed physical volume from the volume group.
5. Finally, mount the file system.

**37. What is `pvmove`, and when is it used in LVM?**

- The `pvmove` command is used to move the data from a failed physical volume to a newly added physical volume.
- This command is used when one of the physical volumes fails in the LVM.

**38. How to inform the client and troubleshoot if the disk is full?**

1. Check which files are accessing more disk space using `# du -h | sort -r`.
2. If any temporary or junk files are present, remove them to make space.
3. Inform the actual situation to the client and take permission to get the LUN from storage.
4. Extend the file system by adding the LUN to the LVM.

**39. Did you work on storage?**

- Although I haven't worked on storage, I know the procedure to export LUN from storage to the client using the iSCSI target.
- Then scan the LUN on the client side and add the LUN to the LVM.
- I am familiar with storage hardware from EMC, NetApp, and others and aspire to work on storage, cloud, and virtualization.

**40. I have four disks, each 1TB, in RAID - (1+0). How much disk space can I utilize in that RAID - (1+0)?**

- RAID - (1+0) means Mirroring + Stripping.
- It requires four disks: two disks for mirroring and two disks for stripping.
- 5-10% disk space is used for superblock information.
- Finally, we can utilize `2TB - (2TB x 10%)` disk space in that RAID - (1+0).

**41. If two disks fail in RAID - (1+0), can we recover the data?**

- RAID - (1+0) requires a minimum of four disks and uses Mirroring + Stripping.
- If one disk fails, we can recover the data.
- If two disks fail, we cannot recover the data.

**42. How many types of disk space issues can we normally get?**

1. Disk is full.
2. Disk is failing or failed.
3. File system corrupted or crashed.
4. OS is not recognizing the remote LUNs when scanning, etc.

**43. What is a link file, and how many types are there?**

- A link file is a shortcut to the original file. Creating and removing (deleting) links between two files is known as managing links.
- There are two types of link files in Linux:
  1. Soft link
  2. Hard link

**44. What is a soft link, and how do you create it?**

- A soft link is a shortcut file. If the original file is deleted, the soft link becomes unusable.
- Soft links can be applied to both directories and files and can span across different file systems.
- If a file is edited, the linked files are updated automatically.
- The soft link and the original file have different inode numbers.
- Command to create a soft link:

```
# ln -s <original file or directory> <link file or directory with path>
Example: # ln -s /root/script /root/Desktop/script
```

**45. What is a hard link, and how do you create it?**

- A hard link is a backup file. If the original file is deleted, the hard link remains unaffected.
- Hard links can only be applied to files, not directories.
- Both the original and hard link files must reside on the same file system.
- The inode number is the same for both original and hard link files.
- Command to create a hard link:

```
# ln <original file> <link file>
```

**46. What are the commands to search files and directories?**

1. `locate`
2. `find`

**47. Explain the `locate` command and how to use it.**

- `locate` searches the `locate` database stored in `/var/lib/mlocate/mlocate.db`.
- Use `# updatedb` to update the `locate` database.
- Command:

```
# locate <file name/directory name>
```

**48. Explain the `find` command and how to use it.**

- The `find` command requires a specific location to search files or directories.
- Syntax:

```
# find <location> <options> <file or directory>
```

- Common options:
  - `-name`: search files and directories by name.
  - `-size`: search for specific file sizes.
  - `-user`: search files by owner.
  - `-empty`: search for empty files or directories.

- Examples:

```
# find / -name <file name> (search for files in the root directory)
# find / -size +10M (search for files greater than 10MB)
# find / -user student (search for files owned by the user "student")
