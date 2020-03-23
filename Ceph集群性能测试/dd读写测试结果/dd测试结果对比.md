# dd测试结果对比

| 类型→     测试方式↓ | **机械盘单盘** | **机械盘集群** | **机械盘开源ceph** | **ssd单盘** | **ssd集群 (做日志盘)** | **ssd开源ceph (做日志盘)** | **集群双网卡** |
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
