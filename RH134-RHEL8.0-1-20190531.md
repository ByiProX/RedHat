[toc]

## [1. Improving Command-line Productivity](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/6)

#### script

|   OS    |                                 |               |
| :-----: | ------------------------------- | ------------- |
| Windows | *.bat, *.cmd, *.vbs             | *.extand name |
|  Linux  | #!/bin/bash<br>chmod +x file.sh | permission    |

#### \, ', "

```bash
# echo haha
# echo haha # hehe
# echo haha \# hehe
# echo haha \# hehe # heihei
# echo haha \# hehe \# heihei
# echo 'haha # hehe # heihei'
# echo "haha # hehe # heihei"
# echo $SHELL
# echo \$SHELL
# echo '$SHELL'
# echo "$SHELL"
# mkdir "a b"
# ls
# mkdir a b
# ls 
# mkdir 'c d'
# ls

$ echo $HOSTNAME
$ ssh root@172.25.254.250 'touch /tmp/$HOSTNAME'
$ ssh root@172.25.254.250 'ls /tmp'
$ ssh root@172.25.254.250 "touch /home/$HOSTNAME"
$ ssh root@172.25.254.250 'ls -l /home'
```

#### basic

```bash
# cat /etc/shells
# echo $SHELL

# vim first.sh
#!/bin/bash

echo hello world
# chmod +x first.sh
# first.sh || echo no

# echo $PATH
# pwd
# ./first.sh && echo ok
# /root/first.sh && echo ok

# mkdir ~/bin
# mv first.sh bin
# first.sh && echo ok

# bash ~/bin/first.sh 
# . ~/bin/first.sh
# source ~/bin/first.sh
```

```bash
# echo `whoami`
# date
# date +%y%M%D
# date +%y%m%d
# touch `date +%y%m%d`.txt
# ls *.txt

# date +%H%M%S
# touch $(date +%H%M%S).txt
# ls *.txt
```

```bash
# vim os.current
#!/bin/bash

echo -e "User:\t" $(whoami)
echo -e "HOST:\t" `hostname`
echo -e "ipv4:\t" $(ip a s enp1s0 | awk '/inet / {print $2}')
echo -e "Memory:\t" $(free -h | awk '/Mem/ {print $2}') 
echo -e "Disk:\t" $(df -ht xfs | awk '/dev/ {print $4}')

# source os.current
# . os.current
# sh os.current
# bash os.current
```

#### loops

```bash
# echo $SHELL
# which for
# which passwd
# man for || echo no
# man bash
/for.*in.*do
# vim /etc/bashrc 
/for

# echo {1..10}
# echo $(seq 1 10)
# echo $HOSTNAME
# echo $HOSTNAMEI
# echo ${HOSTNAME}I
# echo $HOSTNAME\I

# vim loops.sh
#!/bin/bash

for i in {1..10}; do
  useradd user$i 2>/dev/null
  echo mima${i}P | passwd --stdin user$i
done

# . loops.sh
# id user1
# ssh user1@localhost
mima1P
```
#### exit, if

```bash
# echo $?
# man bash
/if.*then.*elif
/exit
/-e
# vim /etc/bashrc 

# vim tj.sh
#!/bin/bash
if [ -e /etc/fstab ]; then
  echo ten
  exit 10
elif [ -e /folder ]; then
  echo two
  exit 20
else
  echo tree
  exit 30
fi
# chmod +x tj.sh

# ls /etc/fstab 
# ./tj.sh 
# echo $?

# rm -rf /etc/fstab 
# ./tj.sh 
# echo $?

# mkdir /folder
# ./tj.sh 
# echo $?
```

#### test, [  ]

```bash
# [ 0 -eq 0 ]
# echo $?
# [ 0 -eq 1 ]
# echo $?

# test 0 -eq 1
# echo $?
# test 0 -eq 0
# echo $?

# [ 0 -eq 1] || echo no
# [0 -eq 1 ] || echo no
```

#### input

```bash
- $0..$9
- $*,$#,$@
- $?

# ping -c 4 172.25.254.254
# vim p.sh
#!/bin/bash

ping -c $2 172.25.254.$1

echo '$0: ' $0
echo '$1: ' $1
echo '$2: ' $2
echo '$3: ' $3
echo '$#: ' $#
echo '$*: ' $*
echo '$@: ' $@
for i in "$*"; do echo $i; done
for i in "$@"; do echo $i; done
# chmod +x p.sh
# ./p.sh 254 5
PING 172.25.254.254 (172.25.254.254) 56(84) bytes of data.
64 bytes from 172.25.254.254: icmp_seq=1 ttl=64 time=0.455 ms
64 bytes from 172.25.254.254: icmp_seq=2 ttl=64 time=1.20 ms
64 bytes from 172.25.254.254: icmp_seq=3 ttl=64 time=0.653 ms
64 bytes from 172.25.254.254: icmp_seq=4 ttl=64 time=0.448 ms
64 bytes from 172.25.254.254: icmp_seq=5 ttl=64 time=0.933 ms

--- 172.25.254.254 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 121ms
rtt min/avg/max/mdev = 0.448/0.738/1.202/0.291 ms
$0:  ./p.sh
$1:  254
$2:  5
$3:
$#:  2
$*:  254 5
$@:  254 5
254 5
254
5

# ls /etc/vimrc ~/.vimrc
# echo set number >> ~/.vimrc

- read
# vim r.sh
#!/bin/bash

echo -n 'step(1/2) - '
read -p 'Please input your hostname: ' HN
echo -n 'step(2/2) - '
read -p 'Please input your name: ' NM

echo
echo 'Your hostname: ' $HN
echo "Your name: $NM"
# chmod +x r.sh
# ./r.sh
```

#### grep, cut, awk

```bash
# grep root /etc/passwd
# grep root /etc/passwd | cut -f 1,3-5 -d:
# grep ^root /etc/passwd
# grep nologin$ /etc/passwd
# cat -n /etc/selinux/config 
# grep ^$ /etc/selinux/config
# grep -v ^$ /etc/selinux/config
# grep # /etc/selinux/config || echo no
# grep ^# /etc/selinux/config
# grep \# /etc/selinux/config
# grep -v ^# /etc/selinux/config
# grep -nv ^# /etc/selinux/config
# ip a s | grep inet
# ip a s | grep -w inet
# hostname
# hostname | grep -E \[0-9\]
# hostname | grep -e 0 -e 1 -e 2 -e 3
# hostname | grep -o -e 0 -e 1
# ip a s | grep ^4.*br0
# ip a s | grep -A 1 ^4.*br0
# ip a s | grep -B 1 ^4.*br0
# ip a s | grep -C 1 ^4.*br0
# egrep 'root|kiosk' /etc/passwd
# hostname | grep 0
# hostname | grep 0 && echo haha
# hostname | grep 1 || echo hehe
# hostname | grep -q 0 && echo haha
# hostname | grep 0 >/dev/null && echo haha
# cat /etc/selinux/config 
# grep ^SELINUX= /etc/selinux/config 
# grep ^SELINUX= /etc/selinux/config > my.file
# grep ^SELINUX= /etc/selinux/config  | cut -f 2 -d =
# awk -F = '/^SELINUX=/ {print $2}' /etc/selinux/config 
# nmcli connection show 'Bridge br0' | grep -i ipv4.*addre | awk '{print $2}'
```



## [2. Scheduling Future Tasks](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/15)

#### at

```bash
# systemctl status atd
# man at

# at now +2 MINUTE
mkdir /1517
touch /1517/new.file
<Ctrl+D>
# atq
# at -c 1
# ls /var/spool/at<Tab>
# ls /15*
# ls /15* -d

# echo kiosk >> /etc/at.deny 
# su - kiosk -c at now +2 minute

# at now +2 minute <<EOF
touch /tmp/1514.txt
EOF
# atq
# atrm 2
# atq    
```

#### cron

```bash
# systemctl status crond.service 
# ls /etc/crontab* 

file
# vim /etc/crontab 
...
*/2 * * * *     kiosk   date >> /tmp/kiosk.date
# man 5 crontab
# vim /etc/crontab 
# ll /tmp/kiosk*
# cat /tmp/kiosk.date

command
# crontab -h
# man 5 crontab
# su - kiosk -c "crontab -e"
*/1 * * * *       date >> /tmp/kiosk.cmd
# crontab -l -u kiosk
# ll /tmp/kiosk.*
# cat /tmp/kiosk.cmd 

# watch -n 2 cat /tmp/kiosk.cmd

/etc/cron.deny
# echo kiosk >> /etc/cron.deny
# su - kiosk -c 'crontab -e' || echo no
# crontab -e -u kiosk

/etc/anacrontab
# vim /etc/anacrontab
# ls -R /etc/cron*
# vim /etc/cron.d/0hourly 
# vim /etc/cron.daily/logrotate 
# vim /etc/cron.daily/rhsmd 
# vim /etc/cron.hourly/0anacron 
# vim /etc/cron.d/raid-check
```
#### systemd-tmpfiles
```bash
# ls /usr/lib/tmpfiles.d/
# cp /usr/lib/tmpfiles.d/tmp.conf /etc/tmpfiles.d/
# cat /etc/tmpfiles.d/tmp.conf 
# systemd-tmpfiles --clean /etc/tmpfiles.d/tmp.conf

# man tmpfiles.d
# vim /etc/tmpfiles.d/moment.conf
d /run/momentary 0700 root root 30s 
# systemd-tmpfiles --create /etc/tmpfiles.d/moment.conf
# ll /run/momentary -d
# touch /run/momentary/test.file
# sleep 30
# ll /run/momentary
# systemd-tmpfiles --clean /etc/tmpfiles.d/moment.conf 
# ll /run/momentary/
```



## [3. Tuning System Performance](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/26)

#### tuned-adm

```bash
# tuned-adm list 
# tuned-adm active
# tuned-adm profile virtual-host
# tuned-adm active
# tuned-adm recommend 
# tuned-adm profile virtual-guest
# tuned-adm active

# systemctl start cockpit

# yum search tuned
# yum list tuned.noarch
# rpm -ql tuned.noarch | grep service
# systemctl status tuned
```

#### nice, renice

```bash
# ps axo pid,comm,nice
# ps axo pid,comm,nice --sort=-nice
# dd if=/dev/zero of=/dev/null &
# ps efo pid,comm,nice --sort=-nice 
# renice 19 51445
# ps efo pid,comm,nice --sort=-nice 
# nice -n -20 dd if=/dev/zero of=/dev/null &
# ps efo pid,comm,nice --sort=-nice 
# renice -n 10 44787
# ps efo pid,comm,nice --sort=-nice 
# nice -n -9 dd if=/dev/zero of=/dev/null &
# ps efo pid,comm,nice --sort=-nice 
# killall dd
# ps efo pid,comm,nice --sort=-nice
```



## [4. Controlling Access to Files with ACLs](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/33)

#### setfacl, getfacl

```bash
# mkdir /folder
# touch /folder/file
# ls -ld /folder/file

-m
# man setfacl 
# ls -ld /folder/{.,*}
# man setfacl | grep setfacl.*-m
# setfacl -m u:student:rwx /folder
# ls -ld /folder/{.,*}
# getfacl /folder/
# su - student -c 'touch /folder/s.txt'
# ls -ld /folder/{.,*}

-R
# setfacl -R -m g:bin:rwx /folder
# getfacl /folder/
# ls -ld /folder/{.,*}

-x
# setfacl -x u:student /folder
# getfacl /folder/
# setfacl -x g:bin /folder
# getfacl /folder/
# ls -ld /folder/{.,*}
# setfacl -x m /folder
# ls -ld /folder/{.,*}
# getfacl /folder/
# setfacl -R -m g:bin:rwx /folder
# getfacl /folder/
-b
# setfacl -b /folder/
# getfacl /folder/
# ls -ld /folder/{.,*}

-d
# ls -ld /folder/{.,*}
# setfacl -m u:student:rwx /folder
# touch /folder/0947.txt
# ls -ld /folder/{.,*}
# setfacl -d -m u:student:rwx /folder
# ls -ld /folder/{.,*}
# touch /folder/0948.txt
# ls -ld /folder/{.,*}
# getfacl /folder/0948.txt 
# getfacl /folder/
# setfacl -m d:u:student:rwx /folder
# getfacl /folder

mask
# ls -l /folder/0948.txt 
# getfacl /folder/0948.txt
# /folder/0948.txt
# setfacl -m m::rwx /folder/0948.txt
# getfacl /folder/0948.txt
# /folder/0948.txt
# setfacl -m u:student:rw /folder/0948.txt
# getfacl /folder/0948.txt

backup,restore
# getfacl -R /folder/
# getfacl -R /folder/ > folder.acl
# echo $?
# setfacl -Rb /folder/
# ls -ld /folder/{.,*}
# cd /
# ls -d folder/
# setfacl --restore ~/folder.acl 
# getfacl -R /folder/
# ls -ld /folder/{.,*}
```



## [5. Managing SELinux Security](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/40)

#### permission

|  ID  |            |                         | SELinux                                                      |
| :--: | ---------- | ----------------------- | ------------------------------------------------------------ |
|  1   | Filesystem | chmod, chown, setfacl   | semanage fcontext ...<br>restorecon ...<br>chcon ...<br>touch /.autorelabel |
|  2   | Service    | vim /etc/*.conf         | setsebool -P ...                                             |
|  3   | Firewall   | firewall-cmd ...        | semanage port ...                                            |
|  4   | SELinux    | vim /etc/selinux/config |                                                              |

#### sort

> **samba**
>
> | STEP |            | EXAMPLE                                             |
> | :--: | ---------- | --------------------------------------------------- |
> |  1   | Firewall   | http(/var/log/messages)                             |
> |  2   | Service    | samba(setsebool -P ...)                             |
> |  3   | Filesystem | http(httpd_sys_content_t)<br />samba(samba_share_t) |
> |  4   | SELinux    | vim /etc/selinux/config                             |

#### config

```bash
# vim /etc/selinux/config 
...
SELINUX=permissive
# setenforce 0
# getenforce 

# vim /etc/selinux/config 
...
SELINUX=enforcing
# setenforce 1
# getenforce 
```

#### httpd(httpd_sys_content_t)

```bash
# yum list httpd
# yum -y install httpd
# rpm -qc httpd | cat -n
# egrep '^DocumentRoot|^DirecotryIndex' /etc/httpd/conf/httpd.conf
# echo test > /var/www/html/index.html
# cat /var/www/html/index.html
# rpm -ql httpd | grep service
# systemctl enable --now httpd
# systemctl status httpd
# curl http://localhost

# ls -ldZ /var/www/html/{.,*}
# echo haha > index.html
# ls -ldZ {.,*}
# cp index.html /var/www/html/cp.html
# mv index.html /var/www/html/mv.html
# ls -ldZ /var/www/html/{.,*}
# curl http://localhost/index.html
# curl http://localhost/cp.html
# curl http://localhost/mv.html || echo no
# semanage fcontext -l | grep /var/www
# restorecon -Rv /var/www
# ls -lZ /var/www/html/mv.html 
# curl http://localhost/mv.html

semanage fcontext, restorecon
# echo hehe > /var/www/html/file3.html
# semanage fcontext -a -t default_t /var/www/html/file3.html
# restorecon -Rv /var/www/html/file3.html
# ls -lZ /var/www/html/
# curl http://localhost/file3.html || ecno no
# restorecon -Rv /var/www/ || echo no
# ls -lZ /var/www/html/
# semanage fcontext  -l | grep file3.html
# man semanage fcontext | grep \#
# semanage fcontext -m -t httpd_sys_content_t /var/www/html/file3.html
# restorecon -v /var/www/html/file3.html
# ll -Z /var/www/html/
# curl http://localhost/file3.html && echo ok

chcon
# chcon -t default_t /var/www/html/file3.html
# ls -lZ /var/www/html/file3.html
# semanage fcontext -l | grep file3

/.autorelabel
# touch /.autorelabel
# sync
# reboot
[foundation]$ rht-vmview view servera
# ls -a /.autorelabel
```

#### samba(samba_share_t)

```bash
# yum list samba*
# yum list samba samba-common samba-client
# yum install -y samba samba-client
# rpm -qc samba-common
# vim /etc/samba/smb.conf
...
[gxm]
        path = /common
        write list = student
        valid users = student
# mkdir /common
# chown student /common/
# ls -ldZ /{.,common}
# (echo ss; echo ss) | smbpasswd -a student
# pdbedit -L
# rpm -ql samba | grep service
# systemctl enable --now smb
# systemctl status smb
# smbclient -L //localhost -U student%ss
# smbclient //localhost/gxm -U student%ss
smb: \> ls
smb: \> exit
# yum list selinux*
# yum -y install selinux-policy-doc.noarch
# rpm -ql selinux-policy-doc.noarch
# man -k samba
# man samba_selinux
# man semanage fcontext | grep \#
# semanage fcontext -a -t samba_share_t "/common(/.*)?"
# semanage fcontext -l | grep /common
# ls -ldZ /common/
# restorecon -Rv /common/
# ls -ldZ /common/
# smbclient //localhost/gxm -U student%ss -c ls
```

#### samba(samba_enable_home_dirs)

```bash
# getsebool -a
# getsebool -a | grep ^samba
# smbclient -L localhost -U student%ss
# smbclient //localhost/tom -U student%ss
smb: \> ls
smb: \> exit
# ls -ldZ /home/{.,*}
# man -k samba
# man samba_selinux | grep home
# man setsebool 
# getsebool -a | grep ^samba
# setsebool -P samba_enable_home_dirs on
# getsebool samba_enable_home_dirs
# smbclient //localhost/tom -U student%ss -c ls
```




## [6. Managing Basic Storage](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/51)

> |    0     | mbr  |     dpt     |      |
> | :------: | :--: | :---------: | :--: |
> | 512 Byte | 446  |     64      |  2   |
> |          |      | Primary<=4  |      |
> |          |      |  Extend<=1  |      |
> |          |      | Logical<=14 |      |

#### partition table

|  ID  |              | Count | Size |           |      |
| :--: | ------------ | ----- | ---- | --------- | ---- |
|  1   | mbr \| msdos | 15    | 2TB  | 3P+1E(nL) | 4P   |
|  2   | gpt          | 128   | 8ZB  | P         |      |

#### filesystem type

|  ID  |        |        Windows         |                            Linux                             |      MacOS      |
| :--: | ------ | :--------------------: | :----------------------------------------------------------: | :-------------: |
|  1   | local  | ntfs, fat32, <br>exfat | xfs, ext4 \| swap<br>[exfat](https://centos.pkgs.org/8/rpmfusion-free-updates-x86_64/) | apfs, <br>exfat |
|  2   | remote |       cifs, nfs        |                                                              |                 |

#### STEP

|      | xfs, ext4           | swap              |
| :--: | ------------------- | ----------------- |
|  1   | lsblk               | fdisk -l          |
|  2   | parted /dev/vdb     | fdisk /dev/vdb    |
|  3   | mkfs.xfs  /dev/vdb1 | mkswap /dev/vdb2  |
|  4   | mkdir /mnt/xfs1     | -                 |
|  5   | vim /etc/fstab      |                   |
|  6   | mount -a            | swapon -a         |
|  7   | df -h               | swapon \| free -h |

#### xfs

```bash
# lsblk 
# parted /dev/vdb
> mklabel msdos
> mkpart
> primary
> 1024s
> 2G
> print
> quit
# mkfs.xfs /dev/vdb1
# mkdir /mnt/xfs
# blkid /dev/vdb1
# man fstab
# vim /etc/fstab 
...
<Ctrl+Shift+V> /mnt/xfs xfs defaults 1 2
# mount -a
# df -h /mnt/xfs/

# sync && reboot
# df -h /mnt/xfs/
```

#### ext4

```bash
# lsblk 
# parted /dev/vdc mklabel gpt
# parted /dev/vdc
> mkpart
> hehe
> 1024s
> 3G
> print
> quit
# mkfs.ext4 /dev/vdc1
# mkdir /mnt/ext4
# blkid /dev/vdc1
# vim /etc/fstab 
...
UUID="71f806bb-c96d-4d87-a6f0-6d39b799798c" /mnt/ext4 ext4 defaults 1 2
# mount -a
# df -h /mnt/ext4

# sync && reboot
# df -h /mnt/ext4
```

#### swap - partition

```bash
# lsblk 
# parted /dev/vdb
) mkpart
) p
) 2G
) 3G
) print
) quit
# mkswap /dev/vdb2
# man fstab
# vim /etc/fstab 
...
UUID=a9a75c10-fe32-4038-9756-77992946194c none swap defaults 0 0
# free -h
# swapon -a
# free -h

# sync && reboot
# swapon
```

#### swap - file

```bash
# df -h
# dd if=/dev/zero of=/pagefile.sys bs=512M count=1
# mkswap /pagefile.sys 
# chmod 0600 /pagefile.sys 
# vim /etc/fstab 
...
/pagefile.sys swap swap defaults 0 0 
# swapon -a
# free -h
# swapon -s

# sync && reboot
# swapon
```

#### emergency

```bash
-destroy
# vim /etc/fstab
...
UUID="71f806bb-c96d-4d87-a6f0-6d39b799798c" /mnt/ext4 ext4 default 1 2
# sync && reboot

-rescue
[foundation]$ rht-vmview view servera
...
Give root password for maintenance
(or press Control-D to continue): `redhat`
# vim /etc/fstab
...
UUID="71f806bb-c96d-4d87-a6f0-6d39b799798c" /mnt/ext4 ext4 defaults 1 2
# sync
# <Ctrl+D>
```



## [7. Managing Logical Volumes](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/58)

|  ID  |   Disk    | LVM  |
| :--: | :-------: | :--: |
|  1   |   Slice   |  pv  |
|  2   |   Disk    |  vg  |
|  3   | Partition |  lv  |

#### create

```bash
# lsblk
# parted /dev/vdb -s mklabel msdos mkpart primary 512B 1G
# lsblk
# pvcreate /dev/vdb1 /dev/vdd
# pvs
# vgcreate vg1 /dev/vdb1 /dev/vdd
# vgs
# vgdisplay
# man vgchange
# vgchange -s 16M vg1 || echo no
# vgchange -s 2M vg1
# vgchange -s 4M vg1
# vgremove vg1
# vgcreate -s 16M vg1 /dev/vdb1 /dev/vdd
# vgdisplay
# lvcreate -h
1# lvcreate -l 100 -n lv1 vg1
2# lvcreate -L 1600M -n lv1 vg1
# lvs
# mkfs.vfat /dev/vg1/lv1
# echo $?
# mkdir /mnt/vfat
# blkid /dev/vg1/lv1
# vim /etc/fstab
...
UUID="10E5-D1B1" /mnt/vfat vfat defaults 0 0
# mount -a
# df -h /mnt/vfat/

# sync && reboot
#  df -h /mnt/vfat
```

#### extend

|  ID  |                  | active                |
| :--: | ---------------- | --------------------- |
|  1   | ext2, ext3, ext4 | resize2fs /dev/...    |
|  2   | xfs              | xfs_growfs /mnt/point |
|  3   | swap             | -                     |

```bash
vgexted
# vgs
# lsblk 
# parted /dev/vdb mkpart primary 1G 3G
# lsblk
# pvcreate /dev/vdb2
# vgextend -h
# vgextend vg1 /dev/vdb2
# vgs
# pvs
# lvs

lvcreate
# lvcreate -n xfs -L 500M vg1
# lvcreate -n swap -L 500M vg1
# mkfs.xfs /dev/vg1/xfs
# mkswap /dev/vg1/swap 
# mkdir /mnt/xfs
# mount /dev/vg1/xfs /mnt/xfs
# df -h /mnt/xfs
# swapon /dev/vg1/swap
# swapon

lvextend
# lvextend -h
# lvextend -L 600M /dev/vg1/xfs 
# df -h /mnt/xfs/
# blkid /dev/vg1/xfs
# xfs_growfs 
# xfs_growfs /mnt/xfs/
# echo $?
# df -h /mnt/xfs/
# lvextend -L +100M /dev/vg1/ext4 
# lvs
# df -h /mnt/ext4/
# blkid /dev/vg1/ext4
# resize2fs 
# resize2fs /dev/vg1/ext4
# echo $?
# df -h /mnt/ext4

# swapoff /dev/vg1/swap
# lvextend -L +100M /dev/vg1/swap
# mkswap /dev/vg1/swap
# swapon /dev/vg1/swap
# free -h
```

```bash
lvreduce - ext2, ext3, ext4
# blkid | grep TYPE.*ext
# lsblk 
# df -h /mnt/ext4
# resize2fs -h
# resize2fs /dev/vg1/ext4 500M || echo no
# umount /mnt/ext4
# resize2fs /dev/vg1/ext4 500M || echo fsck
# e2fsck -f /dev/vg1/ext4
# resize2fs /dev/vg1/ext4 500M
# lvs 
# lvreduce -h
# lvreduce -L 500M /dev/vg1/ext4
# lvs 
# mount -a
# df -h /mnt/ext4
```

```bash
vgreduce, pvremove, lvremove, vgremove
# lsblk 
# umount /mnt/xfs
# umount /mnt/ext4
# swapoff /dev/vg1/swap

# vgs
# vgreduce -h
# vgreduce qagroup /dev/vdb2
# vgs
# pvs
# lvs
# pvremove /dev/vdb2
# pvs

# lvs
# lvremove /dev/qagroup/swap
# lvs

# vgs
# vgremove qagroup
# vgs
# pvs

# pvremove -h
# pvremove /dev/vdb1 /dev/vdd
# pvs

# lsblk 
# parted /dev/vdb -s rm 2 rm 1
# lsblk 
```



## [8. Implementing Advanced Storage Features](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/65)

#### stratis(SSD)

```bash
# lsblk
# yum list stra*
# yum install -y stratis*
# rpm -ql stratisd | grep service
# systemctl enable --now stratisd
# systemctl status stratisd

# man -k stratis
# man stratis

# lsblk
# stratis pool create pool1 /dev/vdb /dev/vdc
# stratis pool list
# stratis blockdev list
# stratis filesystem create pool1
# stratis filesystem create pool1 fs1
# stratis filesystem list
# mkdir /mnt/fs1
# mount /stratis/pool1/fs1 /mnt/fs1
# df -h /mnt/fs1
# dd if=/dev/zero of=/my.txt bs=2M count=1024
# cp /my.txt /mnt/fs1/

# df -hT /mnt/fs1
# vim /etc/fstab
...
/stratis/pool1/fs1 /mnt/fs1 xfs _netdev 1 2
# sync && reboot
```

#### vdo (Deduplicate,Compress)

```bash
# yum search vdo
# yum list kmod-kvdo vdo
# man vdo | grep vdo.*create
# vdo create --name=vdo0 --device=/dev/vdc
# ls -l /dev/mapper/vdo0
# man vdostats
# vdostats --human-readable
# mkfs.xfs-K /dev/mapper/vdo0
# udevadm settle
# mkdir /mnt/vdo0
# vim /etc/fstab
...
/dev/mapper/vdo0 /mnt/vdo0/ xfs x-systemd.require=vdo.service 1 2
# mount -a
# vdostats --human-readable
# dd if=/dev/zero of=/my.1 bs=512M count=1
# for i in {1..12}; do
    cp /my.1 /mnt/vdo0/my.${i}
    vdostats --human-readable
    du -sh /mnt/vdo0
  done
```



## [9. Accessing Network-Attached Storage](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/72)

**[foundation]**

```bash
# ls /etc/exports*
# ll /etc/exports
# vim /etc/exports-foundation0
/content    172.25.0.0/255.255.0.0(ro,sync,crossmnt)
# systemctl status nfs-server
# firewall-cmd --list-all
```

**[servera]**

```bash
# showmount -e 172.25.254.250

# grep ver /etc/nfs.conf
# nfsconf --set nfsd vers3 n
# nfsconf --get nfsd vers3
```

#### mount

```bash
# showmount -e 172.25.254.250 
# mkdir /mnt/cmd
# mount 172.25.254.250:/content /mnt/cmd
# ls /mnt/cmd

# umount /mnt/cmd 
# ls /mnt/cmd
```

#### fstab

```bash
# vim /etc/fstab 
...
172.25.254.250:/content /mnt/cmd/ nfs defaults 0 0 
# mount -a
# mount | grep /mnt/cmd
# ls /mnt/cmd/
# umount /mnt/cmd
# ls /mnt/cmd

# systemctl reboot 
# ls /mnt/cmd/
```

#### autofs

**[foundation]**

```bash
/misc
Insert x.iso
# yum list autofs
# yum -y install autofs
# rpm -qc autofs
# grep ^/misc /etc/auto.master
/misc	/etc/auto.misc
# grep -v \# /etc/auto.misc | grep -v ^$
cd -fstype=iso9660,ro,nosuid,nodev :/dev/cdrom
# ll /dev/cdrom
# ls -d /misc || echo no
# systemctl enable --now autofs
# su - kiosk
$ ls /misc/
$ ls /misc/cd
$ findmnt /misc/cd

/net
# showmount -e 172.25.254.250
# grep ^/net /etc/auto.master
# ls /net
# ls /net/172.25.254.250
# ls /net
# ls /net/172.25.254.250/content

/etc/auto.master.d/nfs.autofs,/etc/auto.nfs
# grep autofs /etc/auto.master
# vim /etc/auto.master.d/nfs.autofs
/nfs    /etc/auto.nfs
# cp /etc/auto.misc /etc/auto.nfs
# vim /etc/auto.nfs
content -ro,soft,intr   172.25.254.250:/content
# systemctl restart autofs
# ls /nfs -d
# ls /nfs
# ls /nfs/content

*=&
# man 5 autofs
# vim /etc/auto.nfs 
*       -ro,soft,intr   172.25.254.250:/&
# systemctl restart autofs
# showmount -e 172.25.254.250
# ls /nfs
# ls /nfs/content
# mount | grep /nfs/content
```



## [10. Controlling the Boot Process](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/79)

**[workstation]**

```bash
# systemctl list-dependencies | grep target
# systemctl cat default.target
# systemctl get-default

# systemctl isolate multi-user.target
# systemctl isolate graphical.target
# init 3
# init 5

CONSOLE
# startx
```

#### BOOT

|  ID  |             |                                      |
| :--: | ----------- | ------------------------------------ |
|  1   | BIOS \| EFI | Device (HD, USB, CDROM, iPXE)        |
|  2   | mbr         | grub2                                |
|  3   | linux       | /boot/grub2/grub.cfg<br />**kernel** |
|  4   | initrd      | /boot/grub/grub.cfg<br />**ramfs**   |
|  5   | ...         | driver                               |
|  6   | fstab       |                                      |
|  7   | service     |                                      |
|  8   | target      | login                                |

#### single

**[servera]**

```bash
menu"Send key" / "Ctrl+Alt+Del"
```

**[foundation]**

```bash
# rht-vmview view servera
mouse click
```

**[servera]**

<kbd>ArrowUp</kbd>, <kbd>e</kbd>

```bash
linux...<space>rd.break console=tty0
```

<kbd>Ctrl</kbd>+<kbd>X</kbd>

```bash
# mount | grep sysroot
# mount -o remount,rw /sysroot
# mount | grep sysroot
# chroot /sysroot
# echo mima | passwd --stdin root
# touch /.autorelabel
# sync
```

<kbd>Ctrl</kbd>+<kbd>D</kbd>, <kbd>Ctrl</kbd>+<kbd>D</kbd>

#### grub passwd

```bash
# (echo mima; echo mima) | grub2-mkpasswd-pbkdf2
# rpm -qc grub2-pc
# vim /boot/grub2/grub.cfg
...
set superusers="tom"
password_pbkdf2 tom grub.pbkdf2.sha512.1000...
### BEGIN /etc/grub.d/01_users ###
# sync
```

#### rescue-cdrom

<kbd>Troubleshooting</kbd>
<kbd>Rescue a Red Hat Enterprise Linux system</kbd><kbd>1) Continue</kbd>

<kbd>Enter</kbd>

```bash
# chroot /mnt/sysimage
# ...
# sync
```

<kbd>Ctrl</kbd>+<kbd>D</kbd>, <kbd>Ctrl</kbd>+<kbd>D</kbd>

#### fstab

```bash
Maintaince
Please input root password: `redhat`
# vim /etc/fstab
...
# sync
# systemctl reboot
```

#### ENV

|  ID  |   Single   |   Rescue   |
| :--: | :--------: | :--------: |
|  1   |   passwd   |   passwd   |
|  2   | /etc/fstab | /etc/fstab |
|  3   |     -      |  grub.cfg  |
|  4   |     -      |    dpt     |
|  5   |     -      | grub2/mbr  |

##### LAB4. dpt-msdos

```bash
-destroy
# lsblk
# sfdisk --dump /dev/vda > vda.dump
# cat vda.dump
# parted /dev/vda -s rm 2 rm 1
# scp vda.dump kiosk@172.25.254.250:~

-rescue
# ifconfig eth0 172.25.0.254/16
# scp kiosk@172.25.254.250:~/vda.dump /tmp
# cat /tmp/vda.dump
# sfdisk /dev/vda < /tmp/vda.dump
```



##### LAB5.  grub-mbr

```bash
-destroy
# df -h /boot
# dd if=/dev/zero of=/dev/vda bs=446 count=1

-rescue
# grub2-install /dev/vda
```



## [11. Managing Network Security](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/88)

#### iptables(RHEL<=7)

```bash
# systemctl status iptables || echo no
# systemctl list-unit-files | grep table
# systemctl status nftables
# systemctl status ebtables
# systemctl list-unit-files | grep fire
# systemctl status firewalld
# systemctl start nftables
# systemctl status nftables
# systemctl status firewalld

# systemctl stop nftables
# systemctl mask nftables
# systemctl status nftables
```



#### firewalld(RHEL>=7)

```bash
-config
# vim /etc/firewalld/zones/public.xml

-CLI(*)
[servera]
# firewall-cmd --get-default-zone 
# firewall-cmd --get-active-zones 
# firewall-cmd --get-services 
# firewall-cmd --get-zones 

# firewall-cmd --permanent --add-service=https
# firewall-cmd --list-all && echo no
# firewall-cmd --reload 
# firewall-cmd --list-all && echo ok
# firewall-cmd --permanent --add-port=83/tcp
# firewall-cmd --add-port=83/tcp
# firewall-cmd --list-all
# firewall-cmd --permanent --remove-service=http --remove-service=https 
# firewall-cmd --permanent --remove-port=8909/tcp
# firewall-cmd --reload
# firewall-cmd --list-all

[servera]
# mandb
# man -k firewall
# man firewalld.richlanguage
# man firewalld.richlanguage | grep rule.*service
# firewall-cmd --list-services 
# firewall-cmd --permanent --remove-service=ssh
# firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="172.25.0.0/16" service name="ssh" accept'
# firewall-cmd --reload
# firewall-cmd --list-all
<Ctrl+D>
[foudation]
$ ssh root@servera
#

-GUI
[kiosk@foundation0]$ ssh -X root@workstation
# yum search fire
# yum list firewall-config
# yum install -y firewall-config
# firewall-config
	`permanent`
	`service` select | `port`,add,8089
	`option` / `reload`
	`runtime`

[foundation]->[server:5423]->[classroom:80]
[servera]
# man -k fire
# man firewall-cmd | grep add.*forward
# firewall-cmd --permanent --add-forward-port=port=5423:proto=tcp:toport=80:toaddr=172.25.254.254
# firewall-cmd --permanent --add-masquerade 
# firewall-cmd --reload
# firewall-cmd --list-all
# curl http://servera:5423 || echo no
# curl http://172.25.254.254
[foundation]
# curl http://servera:5423 && echo yes


-WEB
[servera]
# yum search cock
# yum list cockpit-system
# systemctl status cockpit
# systemctl start cockpit
[foundation]
`firefox http://servera:9090` /
	`Networking` / `firewall`
```

#### semanage port

```bash
# yum list httpd
# yum install -y httpd
# systemctl restart httpd
# systemctl status httpd
# grep ^Listen /etc/httpd/conf/httpd.conf 
# sed -i '/^Listen/s/ .*/ 83/' /etc/httpd/conf/httpd.conf
# grep ^Listen /etc/httpd/conf/httpd.conf 
# systemctl restart httpd || echo no
# vim /var/log/messages 
<G> / <Ctrl+B>
# sealert -h
# sealert -a /var/log/audit/audit.log 
# man semanage port 
# man semanage port | grep \#
# semanage port -l
# semanage port -l | grep -w 80
# man semanage port  | grep \#
# semanage port -a -t http_port_t -p tcp 83
# semanage port -lC
# semanage port -l | grep -w 80
# semanage port -l | grep -w 83
# systemctl restart httpd && echo yes
# systemctl status httpd
# semanage port -l | grep -w 83
# man semanage port  | grep \#
# semanage port -d -t http_port_t -p tcp 83
# semanage port -l | grep -w 80
```



## [12. Installing Red Hat Enterprise Linux](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/95)

#### prepare

|  ID  |            workstation             |   servera    |          |
| :--: | :--------------------------------: | :----------: | -------- |
|  1   |                dhcp                |     pxe      | IP       |
|  2   | tftp-server/(syslinux/pxelinux.0)  |     mbr      | boot     |
|  3   |  tftp-server/(iso/linux+initram)   | kernel+ramfs | anaconda |
|  4   | http, ftp, nfs<br>cdrom, hard(iso) |     http     | manual   |
|  5   |             kickstart              |      ks      | auto     |

**[workstation]**

```bash
-dhcp
# ip a s
# yum search dhcp
# yum list dhcp-server
# yum install -y dhcp-server
# rpm -qc dhcp-server
# head /etc/dhcp/dhcpd.conf
# \cp /usr/share/doc/dhcp-server/dhcpd.conf.example /etc/dhcp/dhcpd.conf
# man dhcpd.conf
# vim /etc/dhcp/dhcpd.conf
allow bootp;
allow booting;

option domain-name "example.com";
option domain-name-servers 172.25.250.254;

default-lease-time 600;
max-lease-time 7200;

log-facility local7;

subnet 172.25.250.0 netmask 255.255.255.0 {
  range 172.25.250.10 172.25.250.20;
  option routers 172.25.250.254;
  next-server 172.25.250.9;
  filename "/pxelinux.0";
}
# systemctl enable --now dhcpd
# firewall-cmd --permanent --add-service=dhcp
# firewall-cmd --reload
-test1, servera-=>ip

-syslinux
# yum search syslinux
# yum list syslinux-tftpboot
# yum install -y syslinux-tftpboot
# rpm -ql syslinux-tftpboot
# mkdir /tftpboot/pxelinux.cfg
# mkdir /content
# mount 172.25.254.250:/content /content
# cd /content/rhel8.0/x86_64/dvd
# ls
# cp isolinux/isolinux.cfg /tftpboot/pxelinux.cfg/default
# vim /tftpboot/pxelinux.cfg/default
...
display `boot.msg`
default `vesamenu.c32`
timeout 60
label linux
  menu label ^Install Red Hat Enterprise Linux 8.0.0
  menu default
  kernel `vmlinuz`
  append initrd=`initrd.img` inst.stage2=`ftp://172.25.250.9/dvd` `inst.ks=ftp://172.25.250.9/ks.cfg` quiet
...
# cp isolinux/{boot.msg,vesamenu.c32} /tftpboot/
# cp images/pxeboot/{vmlinuz,initrd.img} /tftpboot/
-tftp-server
# yum search tftp
# yum install -y tftp-server
# rpm -ql tftp-server
# vim /usr/lib/systemd/system/tftp.service
...
ExecStart=/usr/sbin/in.tftpd -s /tftpboot
# systemctl daemon-reload
# systemctl enable --now tftp
# firewall-cmd --permanent --add-service=tftp
# firewall-cmd --reload
-test2, servera-=>ip,pxelinux0,default,install

-ftp
# yum search ftp
# yum install -y vsftpd
# rpm -qc vsftpd
# man -k ftp
# man vsftpd.conf
# vim /etc/vsftpd/vsftpd.conf 
...
anonymous_enable=YES
anon_root=/var/ftp
# systemctl enable --now vsftpd
# systemctl stop firewalld
# systemctl disable firewalld
# mount /content/rhel8.0/x86_64/isos/rhel-8.0-x86_64-dvd.iso /var/ftp/dvd
```

**[servera]**

PXE

#### kickstart

##### file

**[workstation]**

```bash
# ls -l /root/anaconda-ks.cfg
# cp anaconda-ks.cfg ks.cfg
```

```bash
# cp anaconda-ks.cfg ks.cfg
# vim ks.cfg
#version=RHEL8
ignoredisk --only-use=vda
bootloader --append="console=ttyS0 console=ttyS0,115200n8 no_timer_check net.ifnames=0  crashkernel=auto" --location=mbr --timeout=1 --boot-drive=vda
zerombr
clearpart --all --initlabel
reboot
text
url --url="ftp://172.25.250.9/dvd"
keyboard --vckeymap=us --xlayouts=''
lang en_US.UTF-8
network  --bootproto=dhcp --device=link --activate
rootpw --iscrypted nope
auth --enableshadow --passalgo=sha512
selinux --enforcing
firstboot --disable
services --disabled="kdump,rhsmcertd" --enabled="sshd,NetworkManager,chronyd"
timezone America/New_York --isUtc
part / --fstype="xfs" --ondisk=vda --size=8000

%post --erroronfail
echo redhat | passwd --stdin root
useradd tom
%end

%packages
@core
@base
NetworkManager
dnf-utils
-plymouth
%end
# cp ks.cfg /var/ftp/
# chmod o+r /var/ftp/ks.cfg
# ll /var/ftp/ks.cfg 
```

```bash
inst.ks=http://172.25.250.9/ks.cfg
inst.ks=ftp://172.25.250.9/ks.cfg
inst.ks=nfs:172.25.250.9:/var/ftp/ks.cfg
inst.ks=cdrom:/ks.cfg
inst.ks=hd:/ks.cfg
```

##### web

> https://access.redhat.com/labs/

#### KVM

**[foundation]**

```bash
# systemctl start cockpit
# yum search cock
# yum install -y cockpit-machines
```

`firefox http://172.25.254.250:9090`


## [13. Comprehensive Review](http://foundation0.ilt.example.com/slides/RH134-RHEL8.0-en-1-20190531/#/104)



## A. Appendix

#### setcourse

**[kiosk@foundation0 ~]**

```bash
$ nmcli connection down Wired\ connection\ 1 
$ rht-clearcourse 0
$ rht-setcourse rh134
```

#### vimrc

```bash
$ ls /etc/vimrc ~/.vimrc

$ vim ~/.vimrc
set number ts=2 sw=2 et
```



#### rpm2cpio

```bash
# ls /etc/samba/smb.conf
# rm -rf /etc/samba/smb.conf
# yum -y install samba-common || echo no
# ls /etc/samba/smb.conf
# yum -y reinstall samba-common
# ls /etc/samba/smb.conf

# rm -rf /etc/samba/smb.conf
# rpm -qf /etc/samba/smb.conf
# yum provides *smb.conf
# find /content/ -name samba-common-4.9.1-8.el8.noarch*
# man rpm2cpio | grep rpm2cpio.*cpio
# rpm2cpio /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-common-4.9.1-8.el8.noarch.rpm | cpio -dium
# ls -ld etc
# ls etc/samba/smb.conf
# vim etc/samba/smb.conf

# yum -y reinstall samba-common
# ls /etc/samba/smb.conf
# diff /etc/samba/smb.conf etc/samba/smb.conf
# vim /etc/samba/smb.conf
# diff /etc/samba/smb.conf etc/samba/smb.conf
# vim /etc/samba/smb.conf
# diff /etc/samba/smb.conf etc/samba/smb.conf
```

#### mount iso

```bash
# find / -name *.iso
# mkdir /mnt/iso
# mount /content/rhel8.0/x86_64/isos/rhel-8.0-x86_64-dvd.iso /mnt/iso
# ls /mnt/iso
# umount /mnt/iso
# mount -o loop /content/rhel8.0/x86_64/isos/rhel-8.0-x86_64-dvd.iso /mnt/iso
# ls /mnt/iso
```

#### sed

```bash
# man sed
# cat -n /etc/selinux/config 
# grep ^SELINUX= /etc/selinux/config
# sed '/^SELINUX=/itopline' /etc/selinux/config
# cat -n /etc/selinux/config 
# man sed
# sed -i '/^SELINUX=/itopline' /etc/selinux/config
# cat /etc/selinux/config
# sed -i '/^SELINUX=/abottline' /etc/selinux/config
# cat /etc/selinux/config
# sed -i '/^SELINUX=/s/=.*/=permissive/' /etc/selinux/config
# cat /etc/selinux/config
# sed -i '/line/d' /etc/selinux/config
# cat /etc/selinux/config
# cat -n /etc/selinux/config
# sed -i '7s/^/#/' /etc/selinux/config
# cat -n /etc/selinux/config
# sed -i '7s/#//' /etc/selinux/config
# cat -n /etc/selinux/config
# sed -i '7s|=.*|=enforcing|' /etc/selinux/config
# cat -n /etc/selinux/config

# sed -i.bk '7s|=.*|=enforcing|' /etc/selinux/config
# ls /etc/selinux/config*
/etc/selinux/config  /etc/selinux/config.bk
# diff /etc/selinux/config.bk /etc/selinux/config
7c7
< SELINUX=permissive
---
> SELINUX=enforcing
```

