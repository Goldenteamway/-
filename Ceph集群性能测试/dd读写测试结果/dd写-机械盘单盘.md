# dd写-机械盘单盘

## 单机测试

**1.设备挂载**

在node1下，将物理盘/dev/sdb挂载到/mnt/test。

![](../../pictures/Ceph/dd写-机械盘单盘/1.png)

---

**2.bs=4k的写入速度**

输入

    dd bs=4k count=2500000 if=/dev/zero of=/mnt/test/g1

![](../../pictures/Ceph/dd写-机械盘单盘/2.png)

写10G文件，不加参数来回波动，不准确。

输入

    dd bs=4k count=1000000 if=/dev/zero of=/mnt/test/a1 conv=fsync

![](../../pictures/Ceph/dd写-机械盘单盘/3.png)

写4.1G文件，带conv=fsync参数，平均速度为**112MB/s**

输入

    dd bs=4k count=1000000 if=/dev/zero of=/mnt/test/b1 oflag=sync

![](../../pictures/Ceph/dd写-机械盘单盘/4.png)

写4.1G文件，带oflag=sync参数，平均速度为**4.7MB/s**

输入

    dd bs=4k count=1000000 if=/dev/zero of=/mnt/test/c1 oflag=direct

![](../../pictures/Ceph/dd写-机械盘单盘/5.png)

写4.1G文件，带oflag=direct参数，平均速度为**24.7MB/s**

---

**3.bs=1M的写入速度**

输入

    dd bs=1M count=10000 if=/dev/zero of=/mnt/test/h1

![](../../pictures/Ceph/dd写-机械盘单盘/6.png)

写10G文件，不加参数来回波动，不准确。

输入

    dd bs=1M count=10000 if=/dev/zero of=/mnt/test/d1 conv=fsync

![](../../pictures/Ceph/dd写-机械盘单盘/7.png)

写10G文件，带conv=fsync参数，平均速度为**116.7MB/s**

输入

    dd bs=1M count=10000 if=/dev/zero of=/mnt/test/e1 oflag=sync

![](../../pictures/Ceph/dd写-机械盘单盘/8.png)

写10G文件，带oflag=sync参数，平均速度为**95.0MB/s**

输入

    dd bs=1M count=10000 if=/dev/zero of=/mnt/test/f1 oflag=direct

![](../../pictures/Ceph/dd写-机械盘单盘/9.png)

写10G文件，带oflag=direct参数，平均速度为**125MB/s**

