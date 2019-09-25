# This is a guide to installl ICP-CE Community edition 

## Pre-req
- supported docker version 18.06.2
- install yum utils
```
$ yum install yum-utils
$ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum makecache fast
$ sudo yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.107-3.el7.noarch.rpm
```
- verify 
```
$ cat /etc/yum.repos.d/docker-ce.repo
```
- verify again
```
$ yum repolist
```
- install docker-ce
```
$ yum install docker-ce -y
```
- install it in each node
-  check installation status
```
$ sudo systemctl start docker
$ sudo systemctl status docker
```
## Prepare each node for installation

- check ports are open
```
$  ss -tnlp | awk '{print $4}'| egrep -w "<port_numbers>"
```

- Configure the /etc/hosts file on each node in your cluster.
- On each cluster node, you must configure either a default gateway or a route to the service_cluster_ip_range.
For example, if you want to configure a route to the default IPv4 service_cluster_ip_range, run the following command:
```
$  ip route add 10.0.0.0/24 dev eth0
```


## Disk partitionaning 

## List logical disks and partitions
`sudo fdisk -l`

## Partition the disk
`sudo fdisk /dev/sdb`

* Press `n` to create a partition
* Press `p` or `l` to create primary or logical partitions
* Press `w` to write your changes or `q` to quit

## Format the partition

* `sudo mkfs -t ext4 /dev/sdb1`
* `sudo mkfs -t ext4 -N 2000000 /dev/sdb1` - This will manually set the number of inodes to 2,000,000

## Mount disk
* `mount` - Shows what is mounted
* `mkdir /mnt/mydrive`
* `mount -t ext4 /dev/sdb1 /mnt/mydrive`

## Get disk's UUID
`ls -al /dev/disk/by-uuid/`  
or  
`blkid`

## Mount at boot

Add the following line to your `/etc/fstab` file adjusting the UUID to your device's id and the directory to where you want to mount:

`UUID=811d3de0-ca6b-4b61-9445-af2e306d9999	/mnt/mydrive	ext4	defaults 0 0`

`mount -a` - remounts filesystems from `/etc/fstab`