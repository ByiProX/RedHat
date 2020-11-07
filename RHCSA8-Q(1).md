## 重要配置信息

在考试期间，除了您就坐位置的台式机之外，还将使用多个虚拟系统。您不具有台式机系统的根访问权，但具有对虚拟系统的完全根访问权。

#### 系统信息

|             系统              |     IP 地址      |
| :---------------------------: | :--------------: |
| `mars.domain250.example.com`  | `172.25.250.100` |
| `venus.domain250.example.com` | `172.25.250.200` |

您使用的系统属于 DNS 域 `domain250.example.com`。该域中的所有系统都位于 `172.25.250.0/255.255.255.0` 子网中，该子网中的所有系统都位于 `domain250.example.com` 中。

针对这些系统列出的 IP 地址是应该分配给系统的地址。您可能需要为一个或两个系统配置网络，以便能够通过上述地址访问您的地址。

#### 帐户信息

mars 的根密码已经设置为 `flectrag` 。

除非另有指定，否则这将是用于访问其他系统和服务的密码。此外，除非另有指定，否则应将该密码用于您创建的什么问题帐户或者需要设置密码的任意服务。

#### 其他信息

您可以通过 SSH 或控制台访问考试系统（参见下文所述）。请注意，SSH 访问权可能取决于您解答其他考试项目的情况。

如果您需要在系统上安装其他软件，可以使用位于以下地址的存储库：

- `http://content/rhel8.0/x86_64/dvd/BaseOS`

- `http://content/rhel8.0/x86_64/dvd/AppStream`

#### 重要评测信息

您的系统会在重新引导后进行评测，因此务必确保您实施的的所有配置和服务在重新引导后仍然保留。服务必须在没有人工干预的情况下启动。

同样，本次考试使用的所有虚拟实例都必须 能够重新引导至适当的多用户目标，而无需任何人工辅助。<strong style="color: red">在无法引导或无法进行无人干预引导的系统上完成的所有操作都将为零分。</strong>



<h2>考试要求</h2>
在您的系统上执行以下所有步骤。
[toc]

## 在 mars.domain250.example.com 上执行以下任务。

#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置网络设置

> **将 mars 配置为具有以下网络配置：**
>
> - [ ] 主机名：`mars.domain250.example.com`
> - [ ] IP 地址：`172.25.250.100`
> - [ ] 子网掩码：`255.255.255.0`
> - [ ] 网关：`172.25.250.254`



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置您的系统以使用默认存储库

> **配置您 的系统以使用默认存储库**
>
> - [ ] YUM 存储库已可以从 `http://content/rhel8.0/x86_64/dvd/BaseOS` 和 `http://content/rhel8.0/x86_64/dvd/AppStream` 使用配置您的系统，以将这些位置用作默认存储库。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		调试 SELinux

> **调试 SELinux**
>
> 非标准端口 `82` 上运行的 Web 服务器在提供内容时遇到问题。根据需要调试并解决问题，使其满足以下条件：
>
> - [x] 系统上的 Web 服务器能够提供 `/var/www/html` 中所有现有的 HTML 文件（注：不要删除或以其他方式改动现有的文件内容）
> - [x] Web 服务器在端口 `82` 上提供此内容
> - [x] Web 服务器在系统启动时`自动启动`



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		创建用户帐户

> **创建用户帐户**
>
> 创建下列用户、组和组成员资格：
>
> - [x] 名为 `sysmgrs` 的组
> - [x] 用户 `natasha` ，作为次要组从属于 `sysmgrs`
> - [x] 用户 `harry` ，作为次要组还从属于 `sysmgrs`
> - [x] 用户 `sarah` ，无权访问系统上的`交互式 shell` 且不是 `sysmgrs` 的成员
> - [x] `natasha` 、 `harry` 和 `sarah` 的密码应当都是 `flectrag`



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置 cron 作业

> **配置 cron 作业**
>
> 配置 `cron` 作业，该作业`每隔 2 分钟`运行并执行以下命令：
>
> - [ ] `logger "EX200 in progress"`，以用户 `natasha` 身份运行



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		创建协作目录

> **创建具有以下特征的协作目录 `/home/managers` ：**
>
> - [ ] `/home/managers` 的组用权是 `sysmgrs`
> - [ ] 目录应当可被 `sysmgrs` 的成员读取、写入和访问，但任何其他用户不具这些权限。（当然，root 用户有权访问系统上的所有文件和目录）
> - [ ] `/home/managers` 中创建的文件自动将组所有权设置到 `sysmgrs` 组



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置 NTP

> **配置 NTP**
>
> 配置您的系统，使其成为 `materials.example.com` 的 NTP 客户端。（注：`materials.example.com` 是 `classroom.example.com` 的 DNS 别名）



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置 autofs

> **配置 autofs**
>
> 配置 `autofs` ，以按照如下所述自动挂载远程用户的主目录：
>
> - [ ] `materials.example.com` ( `172.25.254.254` ) NFS 导出 `/rhome` 到您的系统。此文件系统包含为用户 `remoteuser1` 预配置的主目录
> - [ ] `remoteuser1` 的主目录是 `materials.example.com:/rhome/remoteuser1`
> - [ ] `remoteuser1` 的主目录应自动挂载到本地 `/rhome` 下的 `/rhome/remoteuser1`
> - [ ] 主目录必须可供其用户`写入`
> - [ ] `remoteuser1` 的密码是 `flectrag`



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置 /var/tmp/fstab 权限

> **配置 /var/tmp/fstab 权限**
>
> 将文件 `/etc/fstab` 复制到 `/var/tmp/fstab` 。配置 /var/tmp/fstab 的权限以满足如下条件：
>
> - [x] 文件 `/var/tmp/fstab` 自 `root` 用户所有
> - [x] 文件 `/var/tmp/fstab` 属于组 `root`
> - [ ] 文件 `/var/tmp/fstab` 应不能被任何人执行
> - [x] 用户 `natasha` 能够读取和写入 `/var/tmp/fstab`
> - [x] 用户 `harry` 无法写入或读取 `/var/tmp/fstab`
> - [ ] 所有其他用户（当前或未来）能够读取 `/var/tmp/fstab`



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置用户帐户

> **配置用户帐号**
>
> 配置用户 `manalo` ，其用户 ID 为 `3533`。此用户的密码应当为 `flectrag`。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		查找文件

> **查找文件**
>
> 查找当 `jacques` 所有的所有文件并将其副本放入 `/root/findfiles` 目录



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		查找字符串

> **查找字符串**
>
> 查找文件 `/usr/share/xml/iso-codes/iso_639_3.xml` 中包含字符串 `ng` 的所有行。将所有这些行的副本按原始顺序放在文件 `/root/list` 中。 `/root/list` 不得包含空行，且所有行必须是 `/usr/share/xml/iso-codes/iso_639_3.xml` 中原始行的确切副本。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		创建存档

> **创建存档**
>
> 创建一个名为 `/root/backup.tar.gz` 的 tar 存档，其应包含 `/usr/local` 的 tar 存档，其应包含 `/usr/local` 的内容。该 tar 存档必须使用 `gzip` 进行压缩。





---

## 在 venus.domain250.example.com 上执行以下任务。

#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		设置 root 密码

> **设置 root 密码**
>
> 将 venus 的 root 密码设置为 `flectrag` 。您需要获得系统访问权限才能进行此操作。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置您的系统以使用默认存储库

> **配置您 的系统以使用默认存储库**
>
> - [ ] YUM 存储库已可以从 `http://content/rhel8.0/x86_64/dvd/BaseOS` 和 `http://content/rhel8.0/x86_64/dvd/AppStream` 使用配置您的系统，以将这些位置用作默认存储库。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		调整逻辑卷大小

> **设置逻辑卷大小**
>
> 将逻辑卷 `vo` 及其文件系统的大小调整到 `230` MiB。确保文件系统内容保持不变。注：分区大小很少与请求的大小完全相同，因此可以接受范围为 `217` MiB 到 `243` MiB 的大小。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		添加交换分区

> **添加交换分区**
>
> 向您的系统添加一个额外的交换分区 `756MiB` 。交换分区应在系统`启动时自动挂载`。不要删除或以任何方式改动系统上的任何现有交换分区。



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		创建逻辑卷

> **创建逻辑卷**
>
> 根据如下要求，创建新的逻辑卷：
>
> - [x] 逻辑卷取名为 `qa` ，属于 `qagroup` 卷组，大小为 `60` 个扩展块
> - [x] `qagroup` 卷组中逻辑卷的扩展块大小应当为 `16 MiB`
> - [ ] 使用 `ext3` 文件系统格式化新逻辑卷。该逻辑卷应在系统启动时自动挂载到 `/mnt/qa` 下



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		创建 VDO 卷

> **创建 VDO 卷**
>
> 根据如下要求，创建新的 `VDO` 卷：
>
> - [ ] 使用未分区的磁盘
> - [ ] 该卷的名称为 `vdough`
> - [ ] 该卷的逻辑大小为 `50G`
> - [ ] 该卷使用 `xfs` 文件系统格式化
> - [ ] 该卷（在系统启动时）挂载到 `/vbread` 下



#### ○ <font style="font-size:80%">复查</font> ○ <font style="font-size:80%">完成</font>		配置系统调优

> **配置系统调优**
>
> 为您的系统选择建议的 `tuned` 配置集并将它设为默认设置。