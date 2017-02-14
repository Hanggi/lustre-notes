# lustre-note


### acronym

Acronym | Full Name | 中文
------------ | ------------- | ------------
ACL | Access Control List |
AST | Asynchronous System Trap |
DNE | Distributed Namespace Environment |
EA | Extended Attribute |
FID | Lustre File Identifier |
FLD | FID Location Database | FLDB
FMD | Filter Modification Data |
HSM | Hierarchical Storage Management |
iam | Index Access Module |
ldlm | Lustre Distributed Lock Manager |
LOD | Logical Object Device |
LOV | Logical Object Volume | 逻辑对象卷
LMV | Logical Metadata Volume |
lvb | Lustre ValueBlock |
MDC | Metadata Client | 元数据客户端
MDD | Metadata Disk Device? | Metadata Device Driver
MDS | Metadata Server | 元数据服务器
MGS | Management Server | 管理服务器
OBD | Object-based Device |
OFD | OBD Filter Device |
OI | Object Index |
OSC | Object Storage Client | 对象存储客户端
OSD | Object-based Storage Device |
OSD | Object Storage Device |
OSP | Object Storage Proxy |
OSS | Object Storage Server | 对象存储服务器
OST | Object Storage Target | 存储服务器
 | |
SAN | Storage Area Network | 存储域网络
SMB | Server Message Block |
LAP | lov_async_page |
OAP | osc_async_page |


LU_DEVICE_DT: DaTa device (e.g. lod, osp, osd, ofd),
LU_DEVICE_MD: MetaData device (e.g. mdt, mdd),
LU_DEVICE_CL: CLient I/O device (e.g. vvp, lov, lovsub, osc).



Lustreserver Front: MDS/MGS和OSS/OST的集合有时称.
Luster server backends: fsfilt和ldiskfs

From the perspective of MDS, every file are the collection of multiple data objects which striped into one or more OST.

The layout information of one file is defined in the extended attribute(EA) of the index inode.

EA described the mapping relations from the file object ID to its corresponding OST, these informations become striping EA.

## Create Server

#### Mds服务器  
```
# mkfs.lustre --fsname=zds --mdt --mgs /dev/sdb
# mkdir -p /mnt/mds  
# mount -t lustre /dev/sdb /mnt/mds/
# vi /etc/fstab（增加如下内容）  /dev/sdb                /mnt/mds                lustre  defaults        0 0
```

#### Ost1服务器
```
# mkfs.lustre --fsname=zds --ost --mgsnode=mds@tcp0 /dev/sdb
# mkdir -p /mnt/ost1  
# mount -t lustre /dev/sdb /mnt/ost1/
# vi /etc/fstab（增加如下内容）  /dev/sdb                /mnt/ost1                lustre  defaults        0 0
```

#### Ost2服务器
```
# mkfs.lustre --fsname=zds --ost --mgsnode=mds@tcp0 /dev/sdb
# mkdir -p /mnt/ost2  
# mount -t lustre /dev/sdb /mnt/ost2/
# vi /etc/fstab（增加如下内容）  /dev/sdb                /mnt/ost2                lustre  defaults        0 0
```

#### client服务器  
```
# mkdir -p /mnt/zds    
# mount -t lustre mds@tcp0:/zds /mnt/zds/
# vi /etc/rc.local(增加如下内容)  
mount -t lustre mds@tcp0:/zds /mnt/zds/
```

### Check wether the servers working properly

In client node execute
```
$ lfs check servers
```
get:
```
$ lustre-MDT0000-mdc-ffff880232f40800 active.
$ lustre-OST0000-osc-ffff880232f40800 active.
$ lustre-OST0001-osc-ffff880232f40800 active.
```

#### Check the version of lustre

```
$ lctl get_param version
```

#### Check free space

```
$ lfs df
$ lfs df -h
$ lfs df -i
```

## Related Commands

### Server startup ordered

MDS --> OST --> Cient

### lfs

The _lfs_ utility can be used for user configuration routines and monitoring.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.50438206_94597

or

> man lfs

### lfsck

LFSCK is an administrative tool introduced in Lustre software release 2.3 for checking and repair of the attributes specific to a mounted Lustre file system.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.lfsckadmin

or

> man lfsck

### e2fsck

When an OSS, MDS, or MGS server crash occurs, it is not necessary to run e2fsck on the file system.

Before repair, please umount the filesystem first.

> http://doc.lustre.org/lustre_manual.xhtml#troubleshootingrecovery

or

> man e2fsck


## System Configuration Manual

### mkfs.lustre

The _mkfs.lustre_ utility formats a disk for a Lustre service.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.50438219_75432

or

> man mkfs.lustre

### tunefs.lustre

The tunefs.lustre utility modifies configuration information on a Lustre target disk.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.50438219_39574

or

> man tunefs.lustre

### lctl

The lctl utility is used for root control and configuration. With lctl you can directly control Lustre via an ioctl interface, allowing various configuration, maintenance and debugging features to be accessed.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.50438219_38274

or

man lctl

### mount.lustre

The mount.lustre utility starts a Lustre client or target service.

> http://doc.lustre.org/lustre_manual.xhtml#dbdoclet.50438219_12635

or

> man mount.lustre
