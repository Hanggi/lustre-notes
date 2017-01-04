# lustre-note


### acronym

Acronym | Full Name | 中文
------------ | ------------- | ------------
MDS | Metadata Server | 元数据服务器
OSS | Object Storage Server | 对象存储服务器
OST | Object Storage Target | 存储服务器



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
