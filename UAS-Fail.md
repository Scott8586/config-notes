## Quirk to handel UAS driver for SSD on Raspberry Pi

After running the Envoy Scraper for the Solar array for a while, I noticed the database had failed.  There were many scary looking failures on the SSD
I had purchased to hold the prometheus managed database.  

```
Apr 23 00:01:04 raspi42 kernel: [3049919.138364] sd 0:0:0:0: rejecting I/O to offline device
Apr 23 00:01:04 raspi42 kernel: [3049919.138380] blk_update_request: I/O error, dev sda, sector 494995720 op 0x0:(READ) flags 0x3000 phys_seg 1 prio class 0
Apr 23 00:01:04 raspi42 kernel: [3049919.138402] EXT4-fs warning (device sda1): htree_dirblock_to_tree:997: inode #15466498: lblock 0: comm grafana-server: error -5 reading directory block
Apr 23 00:01:04 raspi42 dockerd[581]: time="2021-04-23T00:01:04.521477914-07:00" level=info msg="ignoring event" container=cac4ecda1eb0fbb059502a6498d8bcf5cf0ab677c79bcd22938ce4e2549e58fe module=libcontainerd name
space=moby topic=/tasks/delete type="*events.TaskDelete"
Apr 23 00:01:04 raspi42 containerd[565]: time="2021-04-23T00:01:04.522487621-07:00" level=info msg="shim disconnected" id=cac4ecda1eb0fbb059502a6498d8bcf5cf0ab677c79bcd22938ce4e2549e58fe
```

and also problems like the following, causing a SCSI reset, which I have had experience with using "real" SCSI drives as a very bad thing.

```
Apr 24 10:23:11 raspi42 kernel: [45718.824773] sd 0:0:0:0: [sda] tag#5 uas_eh_abort_handler 0 uas-tag 1 inflight: CMD IN 
Apr 24 10:23:11 raspi42 kernel: [45718.824792] sd 0:0:0:0: [sda] tag#5 CDB: opcode=0x28 28 00 00 04 34 00 00 04 00 00
Apr 24 10:23:11 raspi42 kernel: [45718.825170] sd 0:0:0:0: [sda] tag#4 uas_eh_abort_handler 0 uas-tag 2 inflight: CMD IN 
Apr 24 10:23:11 raspi42 kernel: [45718.825184] sd 0:0:0:0: [sda] tag#4 CDB: opcode=0x28 28 00 00 04 30 00 00 04 00 00
Apr 24 10:23:11 raspi42 kernel: [45718.864785] scsi host0: uas_eh_device_reset_handler start
Apr 24 10:23:11 raspi42 kernel: [45719.025882] usb 2-2: reset SuperSpeed Gen 1 USB device number 2 using xhci_hcd
Apr 24 10:23:11 raspi42 kernel: [45719.061082] scsi host0: uas_eh_device_reset_handler success
```

Using smartctl, the drive didn't seem to be in distress, so I looked around and found some notes about problem with the UAS driver:

[If you have a Raspberry Pi 4 and are getting bad speeds transferring data to/from USB3.0 SSDs, read this](https://www.raspberrypi.org/forums/viewtopic.php?t=245931)

So I followed the instruction for adding a quirk to the cmdline.txt file, to downgrade the driver for that specific drive, and it seemed to solve my problems.

```
...usb-storage.quirks=152d:0578:u
```

I'll keep an eye on it for a little while longer...
