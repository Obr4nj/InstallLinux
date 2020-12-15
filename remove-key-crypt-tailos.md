## Using UI

The first, I follow the instructions on [your's website](https://tails.boum.org/doc/first_steps/persistence/use/index.en.html) but this load unlock very low then I restart and go inside OS

When I inside OS I can find the USB but this locked

![beofore-passphrase](https://github.com/Obr4nj/InstallLinux/blob/main/beofore-passphrase.jpg)

I try unlock with passphrase in `Disks`

![enter-passphrase](https://github.com/Obr4nj/InstallLinux/blob/main/enter-passphrase.png)

and try unlock in the `File`

![after-enter-passphrase](https://github.com/Obr4nj/InstallLinux/blob/main/after-enter-passphrase.png)

But I got the same result, I don't see usb anywhere

![after-passphrase](https://github.com/Obr4nj/InstallLinux/blob/main/after-passphrase.jpg)

After that, I try using `Delete persistent volume`

![using-delete-persistent](https://github.com/Obr4nj/InstallLinux/blob/main/using-delete-presistent.png)

and I get the same when unlock when start OS, this load non-stop

![presistent-loading](https://github.com/Obr4nj/InstallLinux/blob/main/presistent-loading.png)

## Fix using terminal
I search on internet, I check some command-line:

```console
root@amnesia:~# lsblk -a
NAME                                          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
loop0                                           7:0    0     1G  1 loop  /lib/live/mount/rootfs/filesystem.squashfs
loop1                                           7:1    0         0 loop  
loop2                                           7:2    0         0 loop  
loop3                                           7:3    0         0 loop  
loop4                                           7:4    0         0 loop  
loop5                                           7:5    0         0 loop  
loop6                                           7:6    0         0 loop  
loop7                                           7:7    0         0 loop  
sda                                             8:0    1  28.9G  1 disk  
├─sda1                                          8:1    1     8G  1 part  /lib/live/mount/medium
└─sda2                                          8:2    1  20.9G  1 part  
  └─luks-b8dd5dce-ab62-4823-a504-85567ecb636d 254:0    0  20.9G  1 crypt 
nvme0n1                                       259:0    0 232.9G  0 disk  
├─nvme0n1p1                                   259:2    0   200M  0 part  
├─nvme0n1p2                                   259:3    0 222.7G  0 part  
└─nvme0n1p3                                   259:4    0 619.9M  0 part  
nvme1n1                                       259:1    0 238.5G  0 disk  
├─nvme1n1p1                                   259:5    0    16M  0 part  
└─nvme1n1p2                                   259:6    0 238.5G  0 part  
root@amnesia:~# cryptsetup luksDump /dev/sda2
LUKS header information for /dev/sda2

Version:       	1
Cipher name:   	aes
Cipher mode:   	xts-plain64
Hash spec:     	sha256
Payload offset:	4096
MK bits:       	512
MK digest:     	4a b5 82 a0 2f 81 e4 a1 86 b0 9c 6e 3f 95 d2 d1 38 5b da ba 
MK salt:       	69 f8 6b 52 e3 cb 05 9b 49 da b5 ae 46 83 fc 03 
               	be 9e 51 89 83 c7 26 4e cc 86 9c 5e 31 77 8d 08 
MK iterations: 	135966
UUID:          	b8dd5dce-ab62-4823-a504-85567ecb636d

Key Slot 0: ENABLED
	Iterations:         	2182260
	Salt:               	9a ca 00 54 53 19 9d 0a 23 0e f0 36 1c 42 d0 9e 
	                      	31 c0 bf fa 1f c5 6d c3 3e e5 a3 b2 14 0e 2b 9d 
	Key material offset:	8
	AF stripes:            	4000
Key Slot 1: DISABLED
Key Slot 2: DISABLED
Key Slot 3: DISABLED
Key Slot 4: DISABLED
Key Slot 5: DISABLED
Key Slot 6: DISABLED
Key Slot 7: DISABLED
root@amnesia:~# cryptsetup luksRemoveKey /dev/sda2
Enter passphrase to be deleted: 

WARNING!
========
This is the last keyslot. Device will become unusable after purging this key.

Are you sure? (Type uppercase yes): YES
Device wipe error, offset 4096.
Cannot wipe device /dev/sda2.
root@amnesia:~# mount /dev/sda2
mount: /dev/sda2: can't find in /etc/fstab.
root@amnesia:~# fsck -l
fsck from util-linux 2.33.1
root@amnesia:~# ntfsfix /dev/sda2
Mounting volume... Error opening '/dev/sda2' read-write
NTFS signature is missing.
FAILED
Attempting to correct errors... Error opening '/dev/sda2' read-write
NTFS signature is missing.
FAILED
Failed to startup volume: Invalid argument
Error opening '/dev/sda2': Read-only file system
Volume is corrupt. You should run chkdsk.
```

But I can't find `/etc/fstab` and `/etc/crypttab`, I don't know why my usb-stick lost this.
