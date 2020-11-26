# Kali USB with persistence memory

Download Kali Linux iso

Format Usb to FAT32

Open Disk Utility - Locate and select usb

Make 2 Partitions or more as per your need

1st Partition - Name Kali \( Format FAT32 Size 4GB \)

2nd Partition - Name persistence \( Format FAT32 - it fills up quickly when you update and upgrade Kali so i usually have 8GB or more\)

Once done

Install Unetbootin for Mac \( its Free \) google it

Open

Choose Diskimage

Click ... \( three dots - open - locate kali iso \)

Select Kali iso \( if already downloaded \)

Now

Make sure Type is Selected as Usb Drive

Select Partition to install Kali on Usb \( Kali partition for me \)

Click ok

Once finished click exit.

Restart Mac and hold option key on start-up

Select EFI Boot

Select Live System Persistence

Open Gparted

Select Device \( Usb \)

U will see all usb Partitions and Select persistence

Right click On persistence and select Unmount

Again Right click on persistence and select delete

Apply pending opration

Once finish it will automatically named

Unallocated

Again Select and Right click and Select New

Create as Primary Partition

File System - ext4

Label - persistence

Apply Pending Operations

Once done

Exit gparted

Now open terminal

type following

fdisk -l \( To see disks/Volumes \)

mkdir -p /mnt/myusb

enter

mount \(persistence partion name\) /mnt/myusb/

for me it was - mount /dev/sdb3 /mnt/myusb/

enter

echo "/ union" &gt; /mnt/myusb/persistence.conf

umount disk \( For me umount /dev/sdb3 \)

enter

open persistence partition from Files and open file persistence.conf

if it contains text / union \(you should be good\).

reboot to kali live persistence again

After reboot it should be working.



