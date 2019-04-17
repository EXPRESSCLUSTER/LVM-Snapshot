# EXPRESSCLUSTER X mirror disk backup solution by LVM Snapshot (Linux Data mirror)

## Overview

This guide describes the difficulty of merging a LVM snapshot into a EXPRESSCLUSTER (ECX) mirror disk.


## System configuration
- 2 ECX servers
  - CentOS 7.4.1708
    - Kernel version 3.10.0-693.el7.x86_64
  - EXPRESSCLUSTER X3.3 for Linux
- Data mirror cluster
- Logical Volumes (LV) is used as a cluster partition and a mirror partition


## Test Procedures1
1. Start a failover group on a primary server
2. Shutdown a primary server, and then a failover group is started on secondary server
3. Stop a mirror disk resource
4. Change read/write status of a mirror disk
  ```
  > clproset -w -d <data partition>
  ```
5. Open a mirror disk with no mount
  ```
  > clpmdctrl --active -nomount <mirror disk resource>
  ```
6. Merge a LVM snapshot into a mirror disk
  ```
  > lvconvert --merge <LVM snapshot>
  Can't merge until origin volume is closed.
  Merging of snapshot <LVM snapshot> will occur on next activation of <data partition>
  ```

## Test Procedures2
1. Start a failover group on a primary server
2. Shutdown a primary server, and then a failover group is started on secondary server
3. Stop ECX services
  ```
  > clpcl -t
  ```
4. Merge a LVM snapshot into a mirror disk
  ```
  > lvconvert --merge <LVM snapshot>
  Can't merge until origin volume is closed.
  Merging of snapshot <LVM snapshot> will occur on next activation of <data partition>
  ```


## Restriction

Merging a LVM snapshot into a ECX mirror disk is not started until OS is rebooted once.

The merging is completed after OS reboot and starting a mirror disk resource.


----
2019.04.17	Ogata Yosuke <y-ogata@hg.jp.nec.com>	1st issue
