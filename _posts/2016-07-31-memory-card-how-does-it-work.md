---
layout: post
title: Memory card - How does it work?
date: 2016-07-31 16:18:14.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '25336725751'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

My notebook is a bit slow. I only have 4 gb of ram. So I want to upgrade it a bit.
First thing first. What do I have now?

```
sudo dmidecode --type memory
```

This gives us all the info we need about the current memory in out computer.

```
# dmidecode 3.0
Getting SMBIOS data from sysfs.
SMBIOS 2.8 present.

Handle 0x005B, DMI type 16, 23 bytes
Physical Memory Array
Location: System Board Or Motherboard
Use: System Memory
Error Correction Type: None
Maximum Capacity: 16 GB
Error Information Handle: Not Provided
Number Of Devices: 2

Handle 0x0060, DMI type 17, 34 bytes
Memory Device
Array Handle: 0x005B
Error Information Handle: Not Provided
Total Width: 64 bits
Data Width: 64 bits
Size: 4096 MB
Form Factor: SODIMM
Set: None
Locator: DIMM A
Bank Locator: Not Specified
Type: DDR3
Type Detail: Synchronous
Speed: 1600 MHz
Manufacturer: Micron
Serial Number: CFC29B80
Asset Tag: 9876543210
Part Number: 8KTF31561HK-1G6E1
Rank: 1
Configured Clock Speed: 1600 MHz

Handle 0x0065, DMI type 17, 34 bytes
Memory Device
Array Handle: 0x005B
Error Information Handle: Not Provided
Total Width: Unknown
Data Width: Unknown
Size: No Module Installed
Form Factor: DIMM
Set: None
Locator: DIMM B
Bank Locator: Not Specified
Type: Unknown
Type Detail: None
Speed: Unknown
Manufacturer: Not Specified
Serial Number: Not Specified
Asset Tag: Not Specified
Part Number: Not Specified
Rank: Unknown
Configured Clock Speed: Unknown
```


In the first section we can see that the maximum-capacity of this computer is 16gb of memory and that the there is space for two devices. In other words, it has two slots for memory. So I guess I can add two 8gb of memory without problem.
Then it lists the current memories I have installed. As you can see I only have one memory installed, on the first slot. And it appears to be a 4gb memory. What is important to note here it the type. Although it says that the type is DDR3 it might no be the case. To make sure it needs DDR3 and not DDR3L! A DDR3 might not work if it is DDR3L the machine requires.