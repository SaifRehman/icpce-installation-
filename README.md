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

## Docker Push transformation-advisor-db:1.9.9

```
wget https://dl.boxcloud.com/d/1/b1!pPk2EPtdYd40XEDsQJ_u4A6WwZSA4GsQkyRUGVoHEXG8xku0_rEpMDJZhhNRVfYgQfOevzPH3MGmzsrUq4J86ihRnFgEJ9ycG4OxL5yYxBbCNELE1ZSIQD1oeuzYU4UZjzLNYxs-JSA6nfodS9kOLmSc8jaj_gi4a35Uojl8nZkqcUbvz59-L5Q5VQbYSfYSiVG1m8H4FGIQe80mvzeiLeCSO-jbGRjNlP1-MY4nxegAxTtQAUgVEzF_2sRhNX83jN7IjGsIIF8x_Nr7JdjGH_u61i9HQP_YOQCSAHCNRJono9m3KwpqqsE5UvBTW9tlSC1TxPBzZcV0CW_dS6kJSpeYyHmSFUpBnM69hEBkn4EyAio_q02RnEm2loX2XC_QqtARR-5N9zNEn5aFtY3Ynrln6QYttqk1lx6eVf6fu5AL_dOlF6KDWmaN_yTLtSGhyQzlxc15OiLqf7MLqDJWWHwlj1Q-JJ4y-Ape8imsnhgNI0zBo7xDb_bP6x7rhZffZ3hvR3cmFkxyZRrkzzXVn8MkQwy9gY9ixng7bsDYoI2ugMvODMwYbQuWkRrLH8rSgFprn1_9LlKo-G_SyNTfpisz5lVQBzGTlyjkDEdeTVh_dZWCP0QqslhHNLU7ClA_4CAP7es-0BuRlqH_2HB3JBG5NXTwnryxHXW92ODkyst9JkfGiMfkuiGWhXxCoZ8134VjdbyXibcZxDQmoYfZAsnbf3XS_hJbFwXfPxUsckFpmbXdNO7HbuWHnZJ5SeU6du0CJJAPIUxuBq5XFd3G9_IYsLjHEKJsoAKv7kHmPX8dfH2oI8ItcUbtUYmNLjyTXWhNUeyCKYN2WZtiG-LDVbYqCYZOL3ahZdFJVuRcsrRVujJ7VLre0NGHQdk8pknuGQQAg6OEDsdq5NYHqFEgmAXjOqUg1R3yso2XqCZXgFHSUIiMVRP8r3f8C6nEGijgJje6C_e58G4Y_Fgldvziw4m4JEAhIrCOTAkueXX8iLC_MLwYQAMgf3hqCEWQKpYx1c3SDdV6BfJ8E6Q1soIUNK17CSttI1FrRCEgpvlECR_9lk5GisYbqO8FHJJW_Fs0FMhhkhbBhP_MgYkw2I2ArCkzMFXJMgepXh8lBr8t_pSdfSu47APu1V4hrndVdsAV1WmNW_LpXiDAIIK11Okk8EgjUhO1j2D0O7W4EneJfMIgcBNZtscqECHpz5J215jX8XOAikXoLnRlPa_g9CF8Sod6VOOqGFQU4p3DNPeQhIRn5nrjg0qv0vx6WfWMOG_jS_aB2AqABEok4LQnIO5NKR-Zi5ioHc60QhG0vdt_rhE9_dpv6vHd6bsSrk3AeCfuj0Ncc1dYs_7VY6fwX2bJJ0dIjjaMp-ZiFjkKnxIwJlBMhGxO9KlXh1OF9LqI-qg./download
docker load < ta.tar.gz
docker tag ibmcom/transformation-advisor-db:1.9.9 mycluster.icp:8500/transformation-advisor-db:1.9.9
docker push mycluster.icp:8500/transformation-advisor-db:1.9.9
```

## ibmcom/transformation-advisor-server

```
wget https://dl.boxcloud.com/d/1/b1!y1n93FAfOShy2e1s1ZB4-U7zibIEldc-sfIJwUDn6eCrUbXx9FVGyBAP6Sw2nDmm5YX1B9iT0Xv9LMvim_g2_w85L5pZyzhikoDvP2nQ_GEAypNhdWjuh1PTI4e4J2aLJo0UtZpVsM26YO-fyqQK6046bCC_cd3Ons7EbtACGL3os6TByFP5VLMgRkxxwgfUqMvSHzMuXUwSAZ4c28KWH_K1pGnz3TKBwFuqppSYxj0oL76RPj3epKJFol6Rp3b7lyP6owpgy0PHUGabUN6cHm4bAsqG9c6hgMDcMMjyVrgmYJN9APU0Cf5HIuValL94fNHO8k0V2zrroKhleYSDAfBGdrrilt1QwO8dDj_1Uz6eCqM5f2eVSA_wOTFUTi9Zle9pAJT_azyaczBLLlPkXQHgWYyUw72X1aidMx3h0NH4l7G_5tDUJYgTCHq_bhhf5pJPnRn3MhCr7vbP_Yz5ZBRiKDs42h1U0WfZYVoFoh21_RnCNsKAqseXI7I-1SFHv6shBdk1Wqbvf3Z8NQ4HvdrQvD6LgF_bcp758aienr0e-IgkKR0fQujifzAc7KOaFitl89MQmTblY_6HywnQV2ZKZ37iTvMFHCrjEHoDUTN5vzNFMy8WvbMbBE9upYhRw4apjaBG2-mp6T94ENfXuTvViaNI3c3YaEeQw8ZHvndNEIePmztnJbYbHuhukxkKXjVhrWGb193St4-6lLUwyyS5Yqp0-htXY7rECoJIv5N2YK2Za5uE7Fqtz7LOXzplxUn-oJuum74mBKVWvQ8Z5xB6GmybfTU2TS1o8IF1f4zjahjojppLMc4voB3o_mkut_SLi-P9JxAAtqpbNoSIUP3fi5a1_0O6zhLyQXenocE4S5HBmITsaQUs8HSf2seV40gAdU-E9BHOgo0_hcVlM7q-R8f_JuiU0okjS3ITMiNbyfxblNiFHhQg7hTK3z_Mk1xZ80ZyxHQu4tLzPtQ_sAk9wfnbdd1NAmx0M5R50iQmJP0Xzy6Ub-UWva9-nqFxS8RjIDAxXB8LuqscgAct7Vyly45mjjJR1T5Nu3Kl2tPICX8VuKDZpFtpLz6lJL06w5OW6N3FdhfKfigC5od6toNq54GvZVztVj6GN-d6vDqS9ylDhMh4ywBEazWVKCPA-lcuflX3tJh6oZqHwTGziCoQOINZX8eu5kMzAVolnchUUQd7FoT00yrjjDZY8Qlri1TBgc3Q-BZu5WuCDsXFMMSkZL17rjMQnUvZqBk4WQb-bFzRfnH_5eERrtw8ZuszDYdnnZX12dkbw67XmGbZRs__EoPv_UjRjfPDZlysCyWFY7S9sj19r-hP8YnamlDTHjBw90mOCsya1Fuy5KcaCaVEcJSp2DuzfpdmDPS7YK9P3I0vvY6fhAlOFW2fp2tO6cniVOL8Z88qyQ../download
docker load < ta-server.tar.gz
docker tag ibmcom/transformation-advisor-server:1.9.9 mycluster.icp:8500/transformation-advisor-server:1.9.9
docker push mycluster.icp:8500/transformation-advisor-server:1.9.9
```

## Create secret

```
kubectl -n default create secret generic transformation-advisor-secret --from-literal=db_username='YWRtaW4=' --from-literal=secret='YWRtaW4='
```
