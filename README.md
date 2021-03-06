Purifier
===========
Purifier is a fast transparent stateful firewall powered by DPDK. It was created to solve transport layer DDoS attacks.

Installation
------------
- [Get DPDK 16.07](http://fast.dpdk.org/rel/dpdk-16.07.tar.xz)

- [Install DPDK](http://dpdk.org/doc/quick-start) 

- Reserve huge pages memory
```bash
mkdir -p /mnt/huge
mount -t hugetlbfs nodev /mnt/huge
echo 1024 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages
```
- Load Modules to Enable Userspace IO
```bash
sudo modprobe uio
sudo insmod kmod/igb_uio.ko
```
- Define DPDK environment variable
set path to DPDK 
```bash
export RTE_SDK=/path/to/rte_sdk
```
- set target (In most cases it will be x86_64-native-linuxapp-gcc)
```bash
export RTE_TARGET=x86_64-native-linuxapp-gcc
```
- Compile the application
```bash
cd ../src
make
```
Runing app
----------
- [Bind NIC ports to igb_uio driver] (http://dpdk.org/doc/guides/linux_gsg/build_dpdk.html#binding-and-unbinding-network-ports-to-from-the-kernel-modules)

For example to bind eth1 and eth2 from the current driver and move to use igb_uio
```bash
./tools/dpdk_nic_bind.py --bind=igb_uio eth1
./tools/dpdk_nic_bind.py --bind=igb_uio eth2
```
Run the app
```bash
./build/purifier -c 0x7 -n 4
```
Constraints
-----------

- Tested with ixgbe NIC's

TODO
----

- Add mbuf extension
- Add ip defragmentation
- Add telnet/ssh support
- Rework lookup with SSE/AVX
- Add new white/black lists based on bitmaps 

