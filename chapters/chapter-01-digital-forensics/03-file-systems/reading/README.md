# File Systems
All computer applications need to store and retrieve information. Magnetic disks have been used for years for this long-term storage. But solid-state drives have become popular as they offer fast random access. Think of a disk as a sequence of fixed-size blocks supporting two operations:
1. Read block k.
2. Write block k. <br />

These are very inconvenient operations, especially on large systems used by many applications and multiple users. A few questions that arise:
1. How do you find information?
2. How do you keep one user from reading another user’s data?
3. How do you know which blocks are free? [1] <br />

Enter: **the file**. <br />
<br />
The part of the operating system dealing with files is the **file system**.

## Files
A file is an abstraction, that gives a way to store data on the disk and read it, shielding the user from the details data storage. <br />
- files may have extensions
  - in Unix like systems, extensions are just conventions, not enforced by the OS
  - in Windows systems, extensions are assigned meaning
- file is just a sequence of bytes, but the OS will execute a file based on its format
  - for example, an executable has five sections: header, text, data, relocation bits, and symbol table
    - the _header_ starts with a so-called _magic number_, identifying the file type
    - after the header are the _text_ and _data_ of the program itself
    - these are loaded into memory and relocated using the _relocation bits_
    - the _symbol table_ is used for debugging
![Executable Sections](../media/exesections.png)
## Directories
To keep track of files, file systems have directories, which are themselves files.
When the file system is organized as a directory tree, file names are specified by one of two methods:
  - **absolute path** names always start at the root directory and are unique
    - In UNIX the components of the path are separated by /. In Windows the separator is \ .
      - The same path name would be:
        - Windows \usr\sss\forensics
        - UNIX /usr/sss/forensics
  - **relative path** names, used in conjunction with the concept of the current working directory

## EXT4
The newest version of an old Unix file system. Similar flavors include FFS (BSD) and UFS (Solaris). <br />
Other Unix file systems include ZFS, ZFS, or BTRFS, but forensic support is still limited for them. <br />
### Layers
![ext](../media/ext.png) <br />
_image and layers source: Hal Pomeranz' Linux Forensics_
- **Physical Layer**: The physical drive or device and the partitions on it.
  - Linux systems often use the old DOS Master Boot Record (MBR) style partitions with four “primary” partitions and chained “extended” (logical) partitions as necessary. GPT (GUID Partition Tables) is a newer disk partitioning scheme designed to overcome the limitations of MBR, and may be found on some Linux systems.
  - Even though multiple partitions may exist on the same disk, the Unix operating system treats them as independent devices and performs file I/O via individual entries in the /dev directory— e,g., /dev/sdal, /dev/sda2. 
- **File System Layer**: Contains all the config and management data associated with the file systems in each partition on the disk.
  - When a file system is created in a partition, a data structure is created at the beginning of the partition to define the attributes of the file system. This is called a **superblock**, and it contains:
    -  FS type/size, block size, number of blocks/inodes, etc.
    -  Modification time, last mounted on, clean/dirty status
    -  Pointer to inode for file system journal (EXT3 and above)
- **File Name Layer** (AKA Human Interface Layer): responsible for mapping human readable file names to metadata addresses.
  - **Directory files** associate _file names_ to index node (_inode_) numbers in the layer below
  - Directories give the file system its hierarchical structure
- **Metadata Layer**: Contains inodes, the data structures responsible for definition and delineation of files.
    - Every file has an inode that contains:
      - File type
      - Access rights
      - Owners
      - Timestamps
      - Size
      - Pointers to data blocks
    - Inodes store everything about the file that you see in the output of "ls — l" except for the file name 
- **Data Layer**: Stores actual file contents - referred to as blocks in Unix file systems (Windows file systems use the term _clusters_ instead).
  - Blocks are composed of sectors (usually 8 in EXT)
    - sectors are the smallest addressable unit of file I/O (usually 512 bytes)
    - to improve performance, EXT normally performs reads/writes in 4K chunks called blocks (512 x 8 = 4096)
  - Blocks that make up a file are allocated consecutively when possible
  - Blocks are organized into Block Groups of 32K blocks
  - Each block group contains inodes and data blocks

## NTFS
New Technology File System (NTFS) is the default file system for modern Windows-based operating systems.

## FAT

## APFS

## File carving

## Deleting a file
In Unix file systems in EXT4: When a file is deleted, its directory entry is not changed. All that happens is the length of the previous record is extended to consume the space the entry of the deleted file.
- Directory entry for deleted file unchanged
- Previous directory entry “grows” to consume space
- Result: See the file name and inode of deleted files!
- Unfortunately, knowing the inode of the deleted file is not as helpful as it sounds. Starting with EXT3, the block pointers in the inode are zeroed on file deletion. So we can’t use this inode number to directly recover the file content.<br />
In Windows file systems:

## Timestamp tampering






## Summary
- summary

## Drills
### Challenge 1
Description
### Challenge 2
Description
### Challenge 3
Description

## Further reading
[1] Modern Operating Systems by Andrew Tanenbaum, chapter 4 File Systems <br />
[+] [Linux Forensics by Hal Pomeranz](https://archive.org/details/HalLinuxForensics/)<br />
### EXT4
[+] [Understanding EXT4 (Part 1): Extents](https://web.archive.org/web/20210618013020/https://www.sans.org/blog/understanding-ext4-part-1-extents/)<br />
[+] [Understanding EXT4 (Part 2): Timestamps](https://web.archive.org/web/20231203210836/https://www.sans.org/blog/understanding-ext4-part-2-timestamps/)<br />
[+] [Understanding EXT4 (Part 3): Extent Trees](https://web.archive.org/web/20221015052801/https://www.sans.org/blog/understanding-ext4-part-3-extent-trees/)<br />
[+] [Understanding EXT4 (Part 4): Demolition Derby](https://web.archive.org/web/20221002010854/https://www.sans.org/blog/understanding-ext4-part-4-demolition-derby/)<br />
[+] [Understanding EXT4 (Part 5): Large Extents](https://web.archive.org/web/20220630125537/https://www.sans.org/blog/understanding-ext4-part-5-large-extents/)<br />
[+] [Understanding EXT4 (Part 6): Directories](https://web.archive.org/web/20221003153121/https://www.sans.org/blog/understanding-ext4-part-6-directories/)<br />
### NTFS
[+] [NTFS Overview](https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview)<br />
[+] [NTFS](https://ntfs.com/ntfs_basics.htm)<br />
[+] []()<br />
[+] []()<br />
[+] []()<br />
