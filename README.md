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
- check https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/installing/docker_dir.html

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

## Installing ICP-CE
- pull image
```
$ docker pull ibmcom/icp-inception:3.2.0
```
- Make directory
```
$ sudo mkdir /opt/ibm-cloud-private-3.2.0
$ cd /opt/ibm-cloud-private-3.2.0
```
- Extract info
```
$    sudo docker run -e LICENSE=accept \
   -v "$(pwd)":/data ibmcom/icp-inception:3.2.0 cp -r cluster /data
```
- Add the IP address of each node in the cluster to the /<installation_directory>/cluster/hosts file

wget https://ak-dsw-mul.dhe.ibm.com/sdfdl/v2/fulfill/CC1W1EN/Xa.2/Xb.htcOMovxHCAgZGS1maQdxPEqyz_ap_DF/Xc.CC1W1EN/ibm-cloud-private-x86_64-3.2.0.tar.gz/Xd./Xf.lPr.A6VR/Xg.10386025/Xi./XY.knac/XZ.Wl5YIOIeZrTyS9-JE51zOJopGo4/ibm-cloud-private-x86_64-3.2.0.tar.gz#anchor

wget https://ak-dsw-mul.dhe.ibm.com/sdfdl/v2/fulfill/CC1W6EN/Xa.2/Xb.htcOMovxHCAgZGS1maQdxPEqyz9Fmrjn/Xc.CC1W6EN/icp-docker-18.06.2_x86_64.bin/Xd./Xf.lPr.A6VR/Xg.10386025/Xi./XY.knac/XZ.dm3RP1RFPUCHUrkwaL6_efz4r7w/icp-docker-18.06.2_x86_64.bin#anchor