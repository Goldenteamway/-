# dd测试结果对比分析

## 测试结果

| 部署类型→     测试方式↓ | **机械盘单盘** | **机械盘集群** | **机械盘开源ceph** | **ssd单盘** | **ssd集群 (做日志盘)** | **ssd开源ceph (做日志盘)** | **集群双网卡** |
| :------: | :------: | :------: | :------: | :------: | :------: |  :------: | :------: |
| **写bs=4k 无参数** | (10G) 不准确 | (10G) RBD不准确 CEPHFS:26.3MB/s | (10G)RBD不准确 CEPHFS:111.5MB/s | (10G) 不准确 | (10G) RBD不准确 CEPHFS:62.5MB/s | (10G) RBD不准确 CEPHFS:987MB/s | (10G)RBD不准确 CEPHFS:88MB/s |
| **写bs=4k conv=fsync**  | (4.1G) 112MB/s | (4.1G) RBD:34.0MB/s CEPHFS:27MB/s | (4.1G)RBD:73.6MB/s CEPHFS:73.3MB/s | (4.1G) 88.7MB/s | (4.1G) RBD:82.6MB/s CEPHFS:62.3MB/s | (4.1G) RBD:83.3MB/s CEPHFS:70.4MB/s | (4.1G)RBD:88.3MB/s CEPHFS:89.4MB/s |
| **写bs=4k oflag=sync** | (4.1G) 4.7MB/s | (41M) RBD:637KB/s CEPHFS:1.1MB/s | (41M)RBD:620KB/s CEPHFS:1.5MB/s | (1G) 10.2MB/s | (41M) RBD:662KB/s CEPHFS:1.1MB/s | (41M) RBD:677KB/s CEPHFS:1.6MB/s | (41M)RBD:660KB/s CEPHFS:1.1MB/s |
| **写bs=4k oflag=direct** | (4.1G) 24.7MB/s | (41M) RBD:1.6MB/s CEPHFS:1.1MB/s | (41M)RBD:1.6MB/s CEPHFS:1.6MB/s | (1G) 30MB/s | (41M) RBD:1.6MB/s CEPHFS:1.1MB/s | (41M) RBD:1.6MB/s CEPHFS:1.6MB/s | (41M)RBD:1.6MB/s CEPHFS:1.1MB/s |
| **写bs=1M 无参数** | (10G) 不准确 | (10G) RBD不准确 CEPHFS:26.3MB/s | (10G)RBD不准确 CEPHFS:111MB/s | (10G) 不准确 | (10G) RBD不准确 CEPHFS:63.2MB/s | (10G) RBD不准确 CEPHFS:1.3GB/s | (10G)RBD不准确 CEPHFS:89.2MB/s |
| **写bs=1M conv=fsync**  | (10G) 116.7MB/s | (1G) RBD:34.7MB/s CEPHFS:25.7MB/s | (1G)RBD:38.9MB/s CEPHFS:77.8MB/s | (10G) 94MB/s | (1G) RBD:86.3MB/s CEPHFS:61.8MB/s | (1G) RBD:24.8MB/s CEPHFS:82.5MB/s | (1G)RBD:93.4MB/s CEPHFS:87.5MB/s |
| **写bs=1M oflag=sync** | (10G) 95.0MB/s | (1G) RBD:25.2MB/s CEPHFS:21MB/s | (1G)RBD:25.9MB/s CEPHFS:25.5MB/s | (10G) 95.2MB/s | (1G) RBD:30.5MB/s CEPHFS:22.1MB/s | (1G) RBD:29.5MB/s CEPHFS:26.3MB/s | (1G)RBD:30.2MB/s CEPHFS:22.0MB/s |
| **写bs=1M oflag=direct** | (10G) 125MB/s | (1G) RBD:29.5MB/s CEPHFS:21.4MB/s | (1G)RBD:31.9MB/s CEPHFS:25.9MB/s | (10G) 95.7MB/s | (1G) RBD:36.1MB/s CEPHFS:22.5MB/s | (1G) RBD:34MB/s CEPHFS:26.9MB/s | (1G)RBD:35.9MB/s CEPHFS:22.4MB/s |
| **读bs=4k** | (4.1G) 136MB/s | (4.1G) RBD:117MB/s CEPHFS:105MB/s | 空置 | (4.1G) 142MB/s | (4.1G) RBD:115.3MB/s CEPHFS:105MB/s | 空置 | (4.1G)RBD:117MB/s CEPHFS:105MB/s |
| **读bs=1M** | (4.1G) 136MB/s | (4.1G) RBD:117MB/s CEPHFS:105MB/s | 空置 | (4.1G) 142MB/s | (4.1G) RBD:117MB/s CEPHFS:106MB/s | 空置 | (4.1G)RBD:117MB/s CEPHFS:105MB/s |

## 参数介绍

dd命令是很强大的工具，我们可以用它来测试磁盘的读写性能，以下将介绍它的一些相关参数。


### 两个设备， /dev/null和/dev/zero

这两个设备在我们日常Linux的使用中经常会遇到，可以把它两简单的理解为“**黑洞**”和“**白洞**”。

/dev/null相当于一个垃圾回收站，我们可以把文件移动、拷贝到该设备中，意味着这些数据被删除。例如我们用到的命令：
    
    dd bs=4k  if=/mnt/test1/a1 of=/dev/null

这里可以理解为从a1的文件中读数据，将读到的数据给/dev/null设备，相当于将a1里的内容拷贝到/dev/null中。

/dev/zero设备可以从中获取到源源不断的东西，例如我们用到的命令：

    dd bs=4k count=2500000 if=/dev/zero of=/mnt/test1/a1

我们不断的从/dev/zero中读数据，将这些内容写入到a1文件中，具体的方式是，每次读4k，一共读2500000次。

### dd命令的conv=fsync,oflag=sync/direct

对于写数据来说，不加任何的方式，直接进行写会出现数据不准确的情况，例如上例：

    dd bs=4k count=2500000 if=/dev/zero of=/mnt/test1/a1

根据实验的结果可以看出，每次写入的速度差距都非常大。这是因为命令结束的时候数据还没有真正写到磁盘上去，对于磁盘的写，我们一般是先写到了缓存就返回了。

文件 → 缓存 → 磁盘，在这一过程中，我们可以设置不同的参数来调节配置，这里属于非direct 模式，把数据写入系统缓存，然后就认为io成功，并由操作系统决定缓存中的数据什么时候被写入磁盘。首先对比conv=fsync,oflag=sync这两个参数。

**conv=fsync**是在dd命令结束前同步数据和元数据信息，也就是说假如我们写100次，这100次它会都先写到缓存中，最后一次性的写入磁盘。

**oflag=sync**功能也是同步数据和元数据信息，但不同的是它每写一次，都要先写入缓存，再写入磁盘。假如我们写100次，它需要写入100次缓存，100次磁盘。

因此，如果是一次写，它们之间的差别不大，但如果是多次写，oflag=sync要比conv=fsync慢很多，从实验中也可以看出来。

如果要规避掉文件系统cache，直接读写，不使用buffer cache，文件 → 磁盘的形式可以用
**oflag=direct** ，direct 模式就是把写入请求直接封装成io指令发到磁盘。

