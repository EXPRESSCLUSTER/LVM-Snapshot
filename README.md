# LVM snapshot on ECX MD resource

An idea to be validated

1. Make MD resource (NMP1)

   | Resouce name	| md1
   |:--			|:--
   | Cluster partition	| /dev/sdb1
   | Data partition	| /dev/sdb2
   | File system	| none

2. Make PV, VG, LV with NMP1

   ```
   clpmdstat -m md1
   pvreate /dev/NMP1
   vgcreate FOO /dev/NMP1
   lvcreate --name BAR --size 500GB FOO	# example for the MD resource of 500 GB
   ```

3. Format and mount the LV on node-A

   ```
   mkfs.ext4 /dev/FOO/BAR
   mkdir /md
   mount /dev/FOO/BAR /md
   ```

4. Take a snapshot

   ```
   lvcreate -s -n HOGE /dev/FOO/BAR
   ```

5. Takeover the FOG to node-B by clpgrp, shutdown or surprise power off 

6. Marge the snapshot on node-B

   ```
   umount /md
   lvconvert --merge /dev/FOO/HOGE
   mount /dev/FOO/BAR /md
   ```
---
2021.06.21 Miyamoto Kazuyuki