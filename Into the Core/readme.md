# Into the Core - A look at Tiny Core Linux

### Chapter 1. Core concepts

**1.1. Philosophies**

Tiny core loads itself into RAM from storage on boot. Then, Tiny Core either mount or install applications (extension) in storage to RAM.

Tiny Core is designed to run from a RAM copy created at boot time, which protects system files from changes. This is similar to live boot disks.

**1.2. Frugal Install**

**Frugal** is the installation method for Tiny Core. Traditional hard drive installation had all the files scattered in the disk. With Frugal installation, there's only 2 files - **vmlinuz** and **core.gz** - whose locations are specified by the boot loader. All user files and extensions are stored outside the OS.

**1.3. Boot codes**

**Boot codes** affect how Tiny Core operates at boot-time. A list of boot code can be found by pressing F2, F3, or F4 at CD boot prompt.

**Base** is a boot code that skip all extension installation, which can be useful for troubleshooting, extension building, and upgrading.

**1.4. USB and other external storage devices**

Tiny Core can search for data on external storage devices at boot time. If the external device is slow, Tiny Core will boot without the data. To overcome this, boot code **waitusb=5** can be used to pause the boot process for 5 seconds.

**1.5. Dependency checking and downloading**

The **Apps** tool provides application details from ".info" files. Apps tool will take care of checking and downloading dependencies.

**1.6. Modes of operation**

Tiny Core has 3 modes:
- Default mode - cloud/internet
- Mount mode - TCZ/install
- Copy mode - TCZ/install + copy2fs.flg/lst

**1.7. The default mode: cloud/internet**

In default mode, Tiny Core boots entirely into RAM. Users can download applications through Apps tool, and they only last for the current session.

**1.8. Mount mode**

Mount mode is the most widely used and recommended mode.

Applications are stored locally in a directory named **tce**. Applications can also be mounted on reboot, which saves RAM.

Unless **tce=xdyz** is specified, Tiny Core will search for all drives until the first "/tce" directory it can find. Apps tool will download applications into this directory. These applications can either be set to "OnBoot" or "OnDemand".

**1.9. Copy mode**

In copy mode, applications are copied to RAM instead of mounted. They can be loaded in bulk (**copy2fs.flg**), selectively loaded (**copy2fs.lst**), or mounted.

In this mode, boot times are longer because copying to RAM takes longer than mounting, but the runtime speed is faster than mount mode.

Apps tool allows the user to choose which applications to copy and which to mount.

Using bulk selection allows the storage device to be unmounted, which can avoid corruption on power loss.

**1.10. Backup/restore & other persistence options**

Tiny Core supports:
- Backup/restore of personal settings
- Persistent "/home" and "/opt" directories

**Backup/restore**

Tiny Core includes **filetool** utility for saving settings and data. "**/opt/.filetool.lst**" contains a list of files and directories to backup on powerdown and restored on reboot. A user can exclude files using "**/opt/.xfiletool.lst**".

By default, "/home" and "/tc" directories are backed up, and caches and temporary directories are excluded.

Filetool creates a backup file "**mydata.tgz**". By default, Tiny Core will look through the root directory on boot to restore data. Boot code **restore=hdXY/DIR** can be specified to choose the backup file directory. Boot code **norestore** will ignore anny backup files.

**Persistent home**

Boot code **home=hdXY** can be used to mount "/mnt/hdXY/home/tc" to "/home/tc". 

Tiny Core can be used with other Linux installations by creating a "/home/tc" directory under another user's home directory.

Users can choose to use either backup or persistent home directory based on the amount of data. 


### Chapter 2. Installing

**2.1. With the official installer**

The official installer is included in the Core Plus edition, but it can also be downloaded separately (**tc-install.tcz**). **tc-install.sh** is a command-line version of the installer.

1. Source and destination
  - Choose path to core.gz (Usually automatically detected)
  - Choose install types:
    - Frugal (For partition and USB sticks)
    - USB-HDD (For whole disk)
    - USB-ZIP (For older computers) 
  - If it's the only Linux system on the computer, choose "Install boot loader" and "Mark Partition Active (bootable)".
2. File system type
  - Choose file system format:
    - No formatting
    - ext2
    - ext3
    - ext4
    - vfat
3. Boot codes
  - Choose boot codes (Can be editted later)
4. Optional parts (Only for Core Plus)
5. Good to go?

**2.2. From Windows via core2usb**

**core2usb** (http://core2usb.sf.net/) can be used to install Tiny Core to USB.

**2.3. Manually**

Tiny Core can be manually installed from any Linux distro.

1. Partitioning & formatting
  - BIOS installations
    - Create partition on target disk formatted in Linux file system (ext4)
  - UEFI installations
    - Create a GPT EFI boot partition and a normal partition that is vfat formatted
2. Files
  - **core.gz** and **vmlinuz** can be downloaded separately (http://repo.tinycorelinux.net/13.x/x86/release/distribution_files/)
  - Create a directory called "tce" on the target partition.
3. Bootloader
  - For BIOS installs, syslinux, lilo, grub 0.x and grub 2 have been tested
  - For UEFI, only grub 2 has been tested
  - Boot code **quiet** can be used to hide kernel's boot output

For grub 0.97:
```
default 0
timeout 10
title Core
root (hd0,0)
kernel /boot/vmlinuz quiet waitusb=5
initrd /boot/core.gz
```

For grub 2:
```
search --no-floppy --fs-uuid --set=root "fdsf-gt434"
menuentry "Core" {
 linux /boot/vmlinuz quiet waitusb=5
 initrd /boot/core.gz
}
```

### Chapter 3. Basic package management via GUI

Apps tool can be used to install packages. The left white area is used to list packages, while the right area shows the information selected from the top 4 tabs: Info, Files, Depends, Size.

Bottom-left drop-down menu defines what to do with the selected extension. The "TCE" path displays the current tce directory. If the tce directory is red if it is on RAM and green if it is on storage. "URI" shows the selected mirror.

Search drop-down menu lets you choose between name, tag, and files. Main menu on the top-left defines the mode of action. Available apps can be browsed by clicking on Apps > Cloud (Remote) > Browse.
