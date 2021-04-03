# BSD_Migration_Notes
---


## Getting Hardware Information
---

### Hard Drives
**Searching through 'dmesg.boot'**

1. IDE Hard disk names starts with ad – /dev/ad0 first IDE hard disk, /dev/ad1 second hard disk and so on
2. SATA/SSD (ATA Direct Access device driver) disk names starts with ad – /dev/ada, /dev/sdb and so on.
3. SCSI Hard disk names starts with da – /dev/da*
4. IDE CDROM/RW/DVD names starts with acd – /dev/acd*
5. SCSI CDROM/RW/DVD names starts with cd – /dev/cd*

```
> egrep 'da[0-9]|cd[0-]' /var/run/dmesg.boot
ada0 at ahcich0 bus 0 scbus0 target 0 lun 0
ada0: <ST500DM002-1BD142 HP73> ATA8-ACS SATA 3.x device
ada0: Serial Number Z3TPBRLL
ada0: 600.000MB/s transfers (SATA 3.x, UDMA5, PIO 8192bytes)
ada0: Command Queueing enabled
ada0: 476940MB (976773168 512 byte sectors)
ada0: quirks=0x1<4K>
cd0 at ahcich2 bus 0 scbus1 target 0 lun 0
cd0: <hp DVD-RAM GHA3N RH06> Removable CD-ROM SCSI device
cd0: Serial Number 328CD009357
cd0: 150.000MB/s transfers (SATA 1.x, UDMA5, ATAPI 12bytes, PIO 8192bytes)
cd0: Attempt to query device size failed: NOT READY, Medium not present - tray closed
da0 at umass-sim0 bus 0 scbus2 target 0 lun 0
da0: <Kingston DataTraveler 2.0 1.00> Removable Direct Access SPC-2 SCSI device
da0: Serial Number 50E549C2020CFE31796FEFB1
da0: 40.000MB/s transfers
da0: 7388MB (15131636 512 byte sectors)
da0: quirks=0x3<NO_SYNC_CACHE,NO_6_BYTE>
```


## Partitioning Scheme
---

```
# Device                Mountpoint      FStype  Options         		Dump    Pass#	(Size)
/dev/gpt/ssdboot	                boot                    		         	512k
/dev/gpt/ssdrootfs      /               ufs     rw              		1       1	2g
/dev/gpt/ssdvarfs       /var            ufs     rw              		2       2	2g
tmpfs                   /tmp            tmpfs   rw,mode=01777   		0       0
/dev/gpt/ssdusrfs       /usr            ufs     rw              		2       2
/dev/gpt/hdhomefs	/home		ufs	rw				2	2
md99            	none            swap    sw,file=/usr/swap/swap,late     0       0	4g
```


## References
---
1. [Using a Solid State Drive with FreeBSD](http://www.wonkity.com/~wblock/docs/html/ssd.html)
2. [FreeBSD Find Out All Installed Hard Disk Size Information](https://www.cyberciti.biz/faq/freebsd-hard-disk-information/)
