NVMe - Homelab

#####  Comparison of Homelab System's NVMe devices and write performance
#####
#####            NVMe manufacturers provided write speed represents RAW data transfer rate to storage media 
#####            Use dd to write a 10G file direct to device using oflag=direct (bypass system page cache)
#####            The measured dd data transfer rates to a EXT4 filesystem will of necessity include the EXT4 metadata file management overhead in the measured time
#####            Setting the dd blocksize bs=4M improves result ( aligned blocksizes ??) setting to bs=8M loses the benefit
#####            Writing to raw sectors might give an improved result, but how to avoid overwriting filesytem content.  ???   TBD
#####



##### Opi5P   -  Xunlong Orange Pi 5 Plus  -  Linux 7.2.14-200.fc44.aarch64  -  Fedora Linux 44 (Forty Four)
#####            4xCortex-A76 4xCortex-A55    8xthread     Homelab aarch64 testbed

root@opi5p:~# lspci -vvv -d ::0108 | grep -E 'LnkCap:|LnkSta:'
		LnkCap:	Port #1, Speed 16GT/s, Width x4, ASPM L1, Exit Latency L1 unlimited
		LnkSta:	Speed 8GT/s (downgraded), Width x4
root@opi5p:~# lspci | grep Crucial
0000:01:00.0 Non-Volatile memory controller: Micron/Crucial Technology P310 M.2 2230/2280 NVMe PCIe SSD (DRAM-less) (rev 01)
##### Crucial P310 500GB M.2 2280 NVMe SSD PCIe 4.0x4 write rated 3,500 MB/sec
root@opi5p:~# dd if=/dev/zero of=test_write_perf bs=4M count=2560 oflag=direct status=progress
9181331456 bytes (9.2 GB, 8.6 GiB) copied, 5 s, 1.8 GB/s
2560+0 records in
2560+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 5.84531 s, 1.8 GB/s
##### bs=4M has no effect on time (board chipset constraint ??)



##### merri   -  B450M DS3H  -  Linux 7.2.14-200.fc44.x86_64  -  Fedora Linux 44 (Server Edition) 
#####            AMD Ryzen 5 5600G with Radeon Graphics   12xthread     Homelab NAS

root@merri:~# lspci -vvv -d ::0108 | grep -E 'LnkCap:|LnkSta:'
		LnkCap:	Port #0, Speed 8GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 8GT/s, Width x4
root@merri:~# lspci | grep Samsung
07:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983
##### a newer Samsung SSD 970 EVO Plus 500GB PCIe 3.0x4 device write rated 3,200 MB/ses
root@merri:~# dd if=/dev/zero of=test_write_perf bs=4M count=2560 oflag=direct status=progress
9445572608 bytes (9.4 GB, 8.8 GiB) copied, 3 s, 3.1 GB/s
2560+0 records in
2560+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 3.40928 s, 3.1 GB/s



##### pipin   -  B450M DS3H  -  Linux 7.2.14-300.fc45.x86_64  -  Fedora Linux 45 (Server Edition)
#####            AMD Ryzen 7 3800X 8-Core Processor    16xthread     Homelab x86_64 testbed

root@pipin:~# lspci -vvv -d ::0108 | grep -E 'LnkCap:|LnkSta:'
		LnkCap:	Port #0, Speed 8GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 8GT/s, Width x4
root@pipin:~# lspci | grep Samsung
01:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM961/PM961/SM963
##### an old Samsung SSD 960 EVO 500GB PCIe 3.0x4 device write rated 1,800 MB/sec
root@pipin:~# dd if=/dev/zero of=test_write_perf bs=4M count=2560 oflag=direct status=progress
9437184000 bytes (9.4 GB, 8.8 GiB) copied, 5 s, 1.9 GB/s
2560+0 records in
2560+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 5.69565 s, 1.9 GB/s



##### frodo  -  B550 GAMING X V2  -  Linux 7.2.14-200.fc44.x86_64  -   Fedora Linux 44 (Workstation Edition)
#####           AMD Ryzen 9 5900X 12-Core Processor   24xthread       Homelab Workstation

root@frodo:~# lspci -vvv -d ::0108 | grep -E 'LnkCap:|LnkSta:'
		LnkCap:	Port #0, Speed 16GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 16GT/s, Width x4
		LnkCap:	Port #0, Speed 16GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 8GT/s (downgraded), Width x4
root@frodo:~# lspci | grep Samsung
01:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller PM9A1/PM9A3/980PRO
0a:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller PM9A1/PM9A3/980PRO
##### tested 1st dev a Samsung SSD 980 PRO 500GB PCIe 4.0x4 device write rated 5,000 MB/sec (OS drive)
root@frodo:~# dd if=/dev/zero of=test_write_perf bs=4M count=2560 oflag=direct status=progress
8942256128 bytes (8.9 GB, 8.3 GiB) copied, 2 s, 4.5 GB/s
2560+0 records in
2560+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 2.40173 s, 4.5 GB/s



##### rippr   -  PRIME TRX40-PRO  -  Linux 7.2.14-200.fc44.x86_64  -    Fedora Linux 44 (Server Edition)  
#####            AMD Ryzen Threadripper 3960X 24-Core Processor    48xthread        Homelab    VM Host Server

root@rippr:~# lspci -vvv -d ::0108 | grep -E 'LnkCap:|LnkSta:'
		LnkCap:	Port #0, Speed 16GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 16GT/s, Width x4
		LnkCap:	Port #0, Speed 8GT/s, Width x4, ASPM L1, Exit Latency L1 <64us
		LnkSta:	Speed 8GT/s, Width x4
root@rippr:~# lspci | grep Samsung
47:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller PM9A1/PM9A3/980PRO
48:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983
#####  tested 2nd dev a 970EVO Plus PCIe 3.0x4 device write rated 3,200 MB/sec  (OS drive)
root@rippr:~# dd if=/dev/zero of=test_write_perf bs=4M count=2560 oflag=direct status=progress
8854175744 bytes (8.9 GB, 8.2 GiB) copied, 3 s, 3.0 GB/s
2560+0 records in
2560+0 records out
10737418240 bytes (11 GB, 10 GiB) copied, 3.63503 s, 3.0 GB/s
##### No direct to CPU socket on TRX-40 Pro uses I/O chipset






