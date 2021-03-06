#   Linux 磁盘分区、挂载

## 1 分区基础知识

### 1.1 分区方式

```
mbr分区:
1.最多支持四个主分区 
2.系统只能安装在主分区上
3.扩展分区要占一个主分区 扩展分区继续可以划分多个分区
4.mbr最大只支持2TB,但拥有最好的兼容性
```

```
gpt分区:
1.支持无限多个主分区(但操作系统会限制,比如windows下最多支持128个主分区)
2.最大支持18EB的容量(1EB=1024PB)(1PB=1024T)
3.windows7 64位后支持gpt模式
```



### 1.2 windows下的磁盘分区

![0](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122852827-900640102.png)



## 2Linux分区

### 2.1 原理介绍

1.Linux 来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构 , Linux 中每个分区都是用来组成整个文件系统的一部分

2.Linux 采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录， 且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122654975-499315572.png)

### 2.2 硬盘说明

现在主要的硬盘都是scsi硬盘

linux中对scsi硬盘的标识为"sdx~"

如:

sda1:表示该磁盘的分区1

sda2:表示该磁盘的分区2

![ nh](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122855934-957064183.png)



## 3 挂载的经典案例

需求是给我们的 Linux 系统增加一个新的硬盘，并且挂载到/home/newdisk

```
步骤:
1.虚拟机添加硬盘
	安装后需要重启，才能显示
2.分区
	fdisk /dev/sdb
	m	选择帮助
	n	增加分区
	p	划主分区
	1	主分区1
	1	默认
	w	写入分区信息并退出

	fdisk /dev/sdb

3.格式化
	mkfs -t ext4 /dev/sdb1
	
	格式化完成后可以使用

4.挂载
	mount /dev/sdb1 /home/newdisk
	
	临时挂载，重启后消失

5.解绑
	umount /home/newdisk/
	或者
	umount /dev/sdb1


6.设置自动挂载（设置永久挂载）
	vim /etc/fstab
	(内容如下)
	mount -a   立即生效
	
	
	
	"""
	uuid  直接写地址
	0 代表不检测
	1 代表检测
	
	"""
```

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200110161936243-282182386.png)
![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200110161939071-712754494.png)
![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200110161941076-1951010147.png)
![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200110161943898-735119730.png)
![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200110161946201-2122995071.png)

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122917330-143078854.png)







## 4 磁盘情况查询

### 4.1 查询系统整体磁盘使用情况

```
df -lh
```

![](https://img2018.cnblogs.com/blog/1739658/202001/1739658-20200109122920888-1065995189.png)



### 4.2 查询指定目录的磁盘占用情况

```
du 	[选项]		/目录
	-h	带计量单位（以人类可读）
	-s	指令目录占用磁盘大小
	-a	含文件
	-c  列出明细,并显示汇总值
	--max-depth=1	子目录深度->向下查询
```

```
例子:
查询  /opt 目录的磁盘占用情况，深度为 1

du -ach --max-depth=1 /opt
```



### 4.3 常见案例

```
1.统计/home 文件夹下文件的个数
	ls -l /home | grep '^-' | wc -l
2.统计/home 文件夹下目录的个数
	ls -l /home | grep '^d' | wc -l
3.统计/home 文件夹下文件的个数，包括子文件夹里的
	ls -lR /home | grep '^-' | wc -l
4.统计文件夹下目录的个数，包括子文件夹里的
	ls -lR /home | grep '^d' | wc -l
```

