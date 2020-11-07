[toc]

## Introduction

苏振 su

#### 认证

- RHCI-Instructor 讲师

- RHCA-高级

  |  Course   |                       Content                       |
  | :-------: | :-------------------------------------------------: |
  |   CL210   |            红帽 OpenStack 认证系统管理员            |
  | DO407 2.7 | 使用红帽 Ansible 网络自动化，配置和管理网络基础架构 |
  |   DO280   |     红帽平台即服务专业技能证书 OpenShift \| k8s     |
  |   RH318   |            红帽认证虚拟化管理员 (RHCVA)             |
  |   RH236   |             红帽混合云存储专业技能证书              |

- RHCE-中级 系统管理 III

  | Course |      Content       |  Ver  |
  | :----: | :----------------: | :---: |
  | RH294  |    ansible 2.8     | RHCE8 |
  | RH254  | security + service | RHCE7 |

- RHCSA-初级  系统管理I, II

  | Course | Content |  Ver   |        |
  | :----: | :-----: | :----: | :----: |
  | RH134  | system  | RHCSA8 | 4 Days |
  | RH124  |  basic  | RHCSA8 | 5 Days |
#### 环境做准备
|  ID  |                                                      |
| :--: | ---------------------------------------------------- |
| 硬件 | cpu:VT-X、mem:4GB、disk：80GB                        |
| 软件 | OS：x64、APP：VMware (workstation\|fustion)\| player |
| 文件 | folder: rh294/*.vmx                                  |

<div style="background: #dbfaf4; padding: 12px; line-height: 24px; margin-bottom: 24px;">
<dt style="background: #1abc9c; padding: 6px 12px; font-weight: bold; display: block; color: #fff; margin: -12px; margin-bottom: -12px; margin-bottom: 12px;" >Hint - 提示</dt>
解压缩7z/windows、keka/macos
</div>
#### 线下环境

| Machine |        VM        |                       |
| :-----: | :--------------: | --------------------- |
| VMware  |    foundation    |                       |
|   KVM   |    classroom     | dns,dhcp,...          |
|   KVM   |     bastion      | Gateway system        |
|   KVM   |   workstation    | Graphical workstation |
|   KVM   | servera, b, c, d | nic/MAC+IP+HOSTNAME   |

#### 环境配置
- 关闭VMware dhcp：

  - Windows - `编辑`菜单/`虚拟网络编辑器...`/`vmnet1`

  - MacOS -

    ```bash
    function vmnet1_host {
        VFN="/Library/Preferences/VMware Fusion/networking"
        sed -i.bk \
        -e '/VNET_1_DHCP /s/yes/no/' \
        -e '/VNET_1.*NETMASK/s/NETMASK.*/NETMASK 255.255.255.0/' \
        -e '/VNET_1.*SUBNET/s/SUBNET.*/SUBNET 172.25.254.88/' "${VFN}"
    }
    
    # Main area
    vmnet1_host
    ```

- 舒服：`查看`菜单
  - `拉伸客户机`
  - 自定义 / `选项卡`
  
- 关闭`360`或`安全管家`

- MEM：`3072`最小

#### 技巧
| STEP |                | Comment    |
| :--: | -------------- | ---------- |
|  1   | word           | 单词       |
|  2   | <kbd>Tab</kbd> | 列出、补全 |
|  3   | man            | 查帮助     |
|  4   | echo $?        | 0          |

#### 虚拟机控制命令

**[foundation]**

```bash
$ rht-vmctl status all
$ rht-vmctl start classroom
$ rht-vmctl status classroom
$ rht-vmview view classroom
$ rht-vmctl reset classroom
```

```bash
$ rht-vmctl start classroom
$ rht-vmctl start bastion
$ rht-vmctl start servera

- local console
$ rht-vmview view servera

$ nmcli con down 'Wired connection 1'
$ ping classroom
<Ctrl+C>
$ ping -c 4 bastion
$ ping -c 4 servera
- remote console
$ ssh root@servera
```

## 1. [Getting Started with Red Hat Enterprise Linux](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/6)

| vm                               | root        | user              |
| -------------------------------- | ----------- | ----------------- |
| foundation                       | root%Asimov | kiosk%redhat      |
| classroom                        | root%Asimov | instructor%Asimov |
| workstation,bastion,server{a..d} | root%redhat | student%student   |

OS=GNU/Linux

linus + x86/DOS|Minix = Linux /kernel
stallman GNU = tools

redhat 7.. 9 -=> fedora
RHEL 4, 5, 6, **7**, 8
CentOS



## 2. [Accessing the Command Line](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/10)

#### 终端切换

|      | shortcut      | comment    |
| :--: | ------------- | ---------- |
| CLI  | <Ctrl+Alt+Fx> | x in (2,6) |
| GUI  | <Ctrl+Alt+F1> |            |

#### login

|        |                       |                    |
| ------ | --------------------- | ------------------ |
| local  | CLI                   | GUI                |
| remote | ssh root@172.25.250.9 | ssh student@w<Tab> |

#### prompt

[student@workstation ~]$
[root@workstation ~]#
[whoami@hostname pwd]permission

#### logout

\<Ctrl-D> Done

<Ctrl+C> Cancel

内存不够：
CLI=512M, GUI=1024M

#### poweroff, reboot

|      | 关机               | 重启             |
| :--: | ------------------ | ---------------- |
|  1   | init 0             | init 6           |
|  2   | poweroff           | reboot           |
|  3   | systemctl poweroff | systemctl reboot |
|  4   | shutdown -h 0      | shutdown -r 0    |
|  5   | halt -p            | `<Ctrl-Alt-Del>` |

#### syntax

```bash
# cmd [-option] [arg1] [arg2]
```

```bash
# ls
# ls -a
# ls --all
# ls /home
# ls -a /home
```

- 回显式命令

```bash
$ date
$ date +%R
$ date +%y%m%d
```

- 交互式命令

```bash
$ passwd
redhat
P@33w0rd
P@33w0rd
```

#### workspace

|      |                           shortcut                           | version |
| :--: | :----------------------------------------------------------: | :-----: |
|  1   | <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>:arrow_up:</kbd> \| <kbd>:arrow_down:</kbd> | RHEL>=7 |
|  2   | <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>:arrow_left:</kbd> \| <kbd>:arrow_right:</kbd> | RHEL<=6 |



#### 查看文件内容

|      |                                  |                                                 |
| ---- | -------------------------------- | ----------------------------------------------- |
| cat  | cat -n /var/log/messages         |                                                 |
| more | more /var/log/messages           | \<Space>, \<q>                                  |
| less | less /var/log/messages           | \<Space>, \<q>, \<ArrowUp>, \<ArrowDown>, /word |
| head | cat -n /var/log/messages \| head | -n 2                                            |
| tail | cat -n /var/log/messages \| tail | -n 3                                            |
| vim  | vim /var/log/messages            | :q                                              |

#### <kbd>Tab</kbd>

功能：一下补全，两下列出

对象：命令、路径、选项

```bash
# pas<Tab><Tab>
# pass<Tab>
# passwd -<Tab>
# passwd --std<Tab>
# passwd --stdin ki<Tab>
# ls /v<Tab>
# ls /var/l<Tab><Tab>
```

### Continuing a Long Command on Another Line

|      |                                                    |                  |
| ---- | -------------------------------------------------- | ---------------- |
| 1    | <kbd>Alt</kbd>+<kbd>.</kbd>                        | paste"last word" |
| 2    | <kbd>Esc</kbd><kbd>.</kbd>                         |                  |
| 3    | :computer_mouse: left,  double click; middle click | copy,paste       |

文本缓冲区

```bash
# echo haha
# echo haha; echo hehe
# echo wo men ye ke yi, \
shou dong \
huan hang
```

#### history

```bash
# history
# history -a | <Ctrl+D>
# ls -a ~/.bash_history 
# echo $HISTSIZE

# <ArrowUp>/<ArrowDown>

# !cha
# !num
# <Ctrl+R>keyword
```

#### shortcut

```bash
# sYstemctl restart sshd
```

|      | shortcut                                 | word        |
| :--: | ---------------------------------------- | ----------- |
|  1   | <kbd>Ctrl</kbd>+<kbd>A</kbd>             | heAd        |
|  2   | <kbd>Ctrl</kbd>+<kbd>E</kbd>             | End         |
|  5   | <kbd>Ctrl</kbd>+<kbd>:arrow_left:</kbd>  | Word        |
|  6   | <kbd>Ctrl</kbd>+<kbd>:arrow_right:</kbd> | Word        |
|  3   | <kbd>Ctrl</kbd>+<kbd>U</kbd>             | Undo        |
|  4   | <kbd>Ctrl</kbd>+<kbd>K</kbd>             | Kill        |
|  7   | <kbd>Ctrl</kbd>+<kbd>W</kbd>             | Word        |
|  8   | <kbd>Alt</kbd>+<kbd>D</kbd>              | Delete word |



## 3. [Managing Files From the Command Line](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/19)

绝对相对路径

路径是否以根开头

```bash
# cd /
# cd /etc
# cd /var
```

```bash
# cd log
# cd ~
# cd -
# cd .
# cd ..
```

<div style="background: #dbfaf4; padding: 12px; line-height: 24px; margin-bottom: 24px;">
<dt style="background: #1abc9c; padding: 6px 12px; font-weight: bold; display: block; color: #fff; margin: -12px; margin-bottom: -12px; margin-bottom: 12px;" >Hint - 提示</dt>
make directory<br>
move, remove<br>
copy<br>
partition=inode+block
</div>

|      | file  | folder   |
| ---- | ----- | -------- |
| 创建 | touch | mkdir -p |
| 改名 | mv    | mv       |
| 移动 | mv    | mv       |
| 拷贝 | cp    | cp -r    |
| 删除 | rm    | rm -r    |

```bash
# mkdir -p /folder/f1/f2
# ls -d /folder
# touch /folder/file
# ls /folder/file

# cp -r /folder /tmp
# ls -d /tmp/folder
# cp /folder/file /tmp
# ls /tmp/file

# mv /tmp/folder /home
# ls -d /tmp/folder /home/folder
# mv /tmp/file /home
# ls /home/file
# mv /home/folder /home/wjj
# mv /home/file /home/wj

# rm -r /home/wjj
```
<kbd>y</kbd>*n

```bash
# rm /home/wj
```
<kbd>y</kbd>

#### ln

```bash
# dd if=/dev/zero of=/home/bigfile bs=1M count=3
# ls -lh /home/bigfile
# man ls
# du -sh /home
# ln /home/bigfile /home/hard
# ls -lh /home/
# du -sh /home
# ls -lhi /home/
# ln -s /home/bigfile /home/soft
# du -sh /home
# ls -lhi /home/
# ll /etc/grub.cfg
# ll /etc/grub2.cfg
```

#### 通配符

```bash
# ls /folder/
# touch /folder/{a..c}{1..3}.txt
# ls /folder/
# ls /folder/*.txt
# ls /folder/?3*
# ls /folder/a*
# ls /folder/?[23]*
# ls /folder/?[^23]*
# ls /folder/?[!23]*
# ls /folder/{a1,b3}*
# ls /folder/[a-z]*
# ls /folder/[[:alpha:]]*
# ls /folder/[a-zA-Z]*
# ls /folder/[[:lower:]]*
```

## 4. [Getting Help in Red Hat Enterprise Linux](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/32)

#### man

```bash
# man man

# mandb
# man -k passwd

# man passwd
# man 5 passwd
```

#### /usr/share/doc

```bash
# grep -r BOOTPROTO /usr/share/doc/
# vim /usr/share/doc/initscripts/sysconfig.txt
/ifcfg
```

####  [在线帮助](https://access.redhat.com/)

#### info

```bash
# info ls
# info passwd
# info ls
```

#### pinfo

```bash
# pinfo ls
```



## 5. [Creating, Viewing, and Editing Text Files](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/39)

#### >, >> 输出重定向

```bash
# echo haha
# Echo haha

# mkdir /folder
# cd /folder
# ls

# echo haha > 1.txt
# echo haha >> 2.txt
# ls
# cat 1.txt 
# cat 2.txt 

# echo hehe > 1.txt 
# echo hehe >> 2.txt 
# cat 1.txt 
# cat 2.txt 

1=stand, 2=error, &=1+2
# echo haha > new.txt
# cat new.txt 
# Echo haha > new.txt || echo no
# cat new.txt 
# Echo haha 2> new.txt
# cat new.txt 
# echo zheng &> all.txt
# cat all.txt 
# Echo erro &>> all.txt
# cat all.txt

# find / -user kiosk > true.txt 2> err.txt
# cat true.txt 
# cat err.txt 
# find / -user kiosk 2> /dev/null
# du -sh /dev/null

# echo -e "haha\thehe"
# echo -e "haha\nhehe"
# echo -e "haha\nhehe" > my.txt
# cat my.txt
```

#### <, << 输入重定向

```bash
# wc
ni hao
wo hao
da jia hao
<Ctrl+D>
# cat 
ni hao
wo hao
<Ctrl+D>
# cat > input.txt
ni hao
wo hao
da jia hao
<Ctrl+D>
# cat input.txt 
# wc < input.txt 
# cat > new.txt << EOF
ding yi
zhong zhi fu
EOF
# cat new.txt
```

#### | 管道

```bash
# ls
# ls | wc -l

# echo mima | passwd --stdin kiosk

# cat -n /var/log/messages 
# cat -n /var/log/messages | head -n 8
# cat -n /var/log/messages | head -n 8 | tail -n 1

# cat -n /var/log/messages | head -n 8 | tee save.txt | tail -n 1
# cat save.txt 

# w
# cat -n /var/log/messages | head -n 8 | tee /dev/tty2 | tail -n 1
# <Ctrl+Alt+F2>
```

#### vim 编辑器

三个模式：命令模式（默认）、输入模式、末行模式

```bash
# vim chuangjian.txt
<i>
ni hao
wo hao
<Esc>
:wq!
```

|                                            | cha                                              | word                                                         | line                      |
| ------------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------ | ------------------------- |
| :set                                       | :set number                                      |                                                              |                           |
|                                            | :set nonumber                                    |                                                              |                           |
| Go                                         | <kbd>G</kbd>, n<kbd>G</kbd>                      | <kbd>g</kbd><kbd>g</kbd>, n<kbd>g</kbd><kbd>g</kbd>          | :`line-numbe`             |
| Arrow                                      | <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd> | :arrow_left: :arrow_down: :arrow_up: :arrow_right:           |                           |
| word                                       | <kbd>w</kbd>                                     | <kbd>b</kbd>                                                 |                           |
| delete                                     | <kbd>x</kbd>, <kbd>d</kbd><kbd>l</kbd>           | <kbd>d</kbd><kbd>w</kbd>                                     | <kbd>d</kbd><kbd>d</kbd>  |
| yank                                       | <kbd>v</kbd><kbd>y</kbd>                         | <kbd>y</kbd><kbd>w</kbd>                                     | <kbd>y</kbd><kbd>y</kbd>  |
| paste                                      | <kbd>P</kbd>                                     | <kbd>p</kbd>                                                 |                           |
| muti                                       |                                                  |                                                              | 3<kbd>y</kbd><kbd>y</kbd> |
|                                            | ^                                                | $                                                            |                           |
|                                            | :3,5d                                            |                                                              |                           |
| <strong style="color: red">VISUAL</strong> | <kbd>v</kbd>                                     | <kbd>Ctrl</kbd>+<kbd>v</kbd>, <kbd>j</kbd>\*N, <kbd>I</kbd>, <kbd>Space</kbd>\*2M, <kbd>Esc</kbd> |                           |
| Undo                                       | <kbd>u</kbd>                                     |                                                              |                           |
| Redo                                       | <kbd>Ctrl</kbd>+<kbd>R</kbd>                     |                                                              |                           |
| Replace                                    | <kbd>r</kbd>x                                    | <kbd>R</kbd>                                                 |                           |
| search                                     | <kbd>/</kbd>word, <kbd>?</kbd>word               | <kbd>n</kbd>, <kbd>N</kbd>                                   |                           |
| write                                      | <kbd>:</kbd><kbd>w</kbd><kbd>q</kbd>             | <kbd>:</kbd><kbd>w</kbd> path/file                           | <kbd>:</kbd><kbd>q</kbd>! |
|                                            | :%s/^/# <kbd>Enter</kbd>                         |                                                              |                           |

#### 变量

```bash
# env
# set
# echo $HISTSIZE
# echo $PS1
# PS1="C:\> "
# PS1="[\u@\h \W]\$ "
# PS1="[\u@\h \w]\$ "

# NEW_FOLDER=/fx/fy/fz
# echo $NEW_FOLDER
# mkdir -p $NEW_FOLDER
# ls -d /fx
# ls -R /fx
# NEW_FOLDER="fa fb fc"
# echo $NEW_FOLDER
# mkdir -p $NEW_FOLDER
# ls 
# echo $NEW_FOLDER
# unset NEW_FOLDER
# echo $NEW_FOLDER

export
# export NEW_FOLDER='fa fb fc'
# echo $NEW_FOLDER


# ls /etc/profile /etc/bashrc 
# ls ~/.bash_profile ~/.bashrc 

# psa || echo no
# alias 
# vim ~/.bashrc 
<G>
<o>
alias psa='ps -aux'
# psa || echo no
# <Ctrl+D> 或者 # source ~/.bashrc
# psa && echo OK
```



## 6. [Managing Local Users and Groups](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/48)

#### <strong style="color:red">useradd</strong>, <strong style="color:red">usermod</strong>, userdel

#### id, <strong style="color:red">su</strong>, sudo

#### <strong style="color:red">passwd</strong>, chage

#### <strong style="color:red">groupadd</strong>, groupmod, groupdel

```bash
# id
# id 1000
# id kiosk

su, sudo
# su - kiosk
$ tail /var/log/messages || echo no
$ <Ctrl+D>
# grep wheel /etc/sudoers
# visudo 
# usermod -aG wheel kiosk
# su - kiosk
$ tail /var/log/messages || echo no
$ sudo tail /var/log/messages && echo yes

# grep kiosk /etc/{passwd,shadow}
# chage -l kiosk 

useradd, usermod, userdel
# useradd tom
# grep tom /etc/{passwd,shadow}
# usermod -u 1111 tom
# grep tom /etc/{passwd,shadow}
# usermod -g 1111 tom || echo no

groupadd, groupmod, groupdel
# grep tom /etc/group
# groupadd cat
# grep cat /etc/group
# groupmod -g 1111 cat
# grep cat /etc/group
# groupmod -n mao cat
# grep 1111 /etc/group

# usermod -g 1111 tom
# grep tom /etc/{passwd,shadow}
# usermod -c xiaomao tom
# grep tom /etc/{passwd,shadow}
# usermod -d /home/mao tom
# grep tom /etc/{passwd,shadow}
# ls /home/
# mv /home/tom /home/mao
# ls /home/ -l
# cat /etc/shells 
# usermod -s /bin/sh tom
# grep tom /etc/{passwd,shadow}
# echo mima | passwd --stdin tom
# grep tom /etc/{passwd,shadow}
# usermod -l mao tom
# id mao
# usermod -L mao
# grep mao /etc/{passwd,shadow}
# usermod -U mao
# grep mao /etc/{passwd,shadow}
# userdel mao
# id mao
# ls -l /home/mao

# useradd jerry
# ls /home/ -l
# userdel -r jerry
# id jerry
# ls /home/ -l

# groupdel tom

chage
# chage -l kiosk
# chage -d 0 kiosk
# ssh kiosk@localhost
mima
mima
P@33w0rd
P@33w0rd
# chage -l kiosk
# chage -m 9 -M 30 -W 14 kiosk
# chage -l kiosk
# grep kiosk /etc/shadow

# usermod -s /bin/false kiosk
```

|  ID  | FILE        |
| :--: | ----------- |
|  1   | /etc/passwd |
|  2   | /etc/shadow |
|  3   | /etc/group  |



## 7. [Controlling Access to Files](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/61)

#### permission

|      |         |   file   | folder |
| :--: | ------- | :------: | :----: |
|  r   | Read    |   cat    |   ls   |
|  w   | Write   |   vim    | touch  |
|  x   | execute | ./script | **cd** |

#### ls -l

```bash
# ls -lh /etc/fstab 
-rw-r--r--. 1 root root 1.1K Aug  3 22:33 /etc/fstab
```

| rwx  |      |                         |  7   |
| :--: | :--: | ----------------------- | :--: |
| r--  | 100  | 1*2^2^+0\*2^1^+0\*2^0^= |  4   |
| -w-  | 010  | 0*2^2^+1\*2^1^+0\*2^0^= |  2   |
| --x  | 001  | 0*2^2^+0\*2^1^+1\*2^0^= |  1   |

#### <strong style="color:red">chmod</strong>

who which what

| u    | User  |      |
| ---- | ----- | ---- |
| g    | Group |      |
| o    | Other |      |
| a    | All   |      |

\+ , -, =

```bash
# mkdir /folder
# touch /folder/file

char
# ls -ld /folder/{.,*}
# chmod g+w,o=- /folder
# ls -ld /folder/{.,*}
# chmod go=rx /folder/
# ls -ld /folder/{.,*}
# chmod a=- /folder/
# ls -ld /folder/{.,*}
# chmod o+r /folder/
# ls -ld /folder/{.,*}

number
# chmod 755 /folder/
# ls -ld /folder/{.,*}
# chmod 700 /folder/
# ls -ld /folder/{.,*}

-R
# ls -ld /folder/{.,*}
# chmod -R 755 /folder/
# ls -ld /folder/{.,*}
```

#### <strong style="color:red">chown</strong>

```bash
# id root
# id 1
# id 2

# chown bin:daemon /folder/
# ls -ld /folder/{.,*}
# chown kiosk /folder/
# ls -ld /folder/{.,*}
# chown :root /folder/{.,*}
# ls -ld /folder/{.,*}
# chown -R bin /folder
# ls -ld /folder/{.,*}
```

#### suid=4, sgid=2, stick=1

rwxrwxrwx	777

uuugggooo

rw**s**rw**s**rw**t**	**7**777

```bash
suid
- kiosk root /etc/shadow
# whereis passwd
# ll /usr/bin/passwd /etc/shadow

- root kiosk /tmp/root.mo
# ll /usr/bin/touch
# touch /tmp/root.txt
# ll /tmp/root.txt
# cp /usr/bin/touch /usr/bin/mo
# chown kiosk /usr/bin/mo
1# chmod u+s /usr/bin/mo
2# chmod 4755 /usr/bin/mo
# ll /usr/bin/mo
# mo /tmp/root.mo
# ll /tmp/root.mo

sgid
# touch /folder/root.txt
# ll /folder/{.,root.txt}
# ll -d /folder/{.,root.txt}
# chown :kiosk /folder/
# ll -d /folder/{.,root.txt}
1# chmod 2755 /folder/
2# chmod g+s /folder
# ll -d /folder/{.,root.txt}
# touch /folder/root.new
# ll -d /folder/{.,root.*}


stick
# su - kiosk -c 'touch /tmp/k{1..3}.txt'
# ll /tmp/k*.txt
# rm -rf /tmp/k1.txt 
# su - kiosk -c 'rm -rf /tmp/k2.txt'
# ll /tmp/k*.txt
# su - bin -s /bin/bash -c 'rm -rf /tmp/k3.txt' || echo no
# ll /tmp/k*.txt
```

#### umask

```bash
folder= 777 - umask
file= 666 - umask

# umask
# mkdir wjj; touch wj; ls -ld wj*
# su - kiosk
$ umask
$ mkdir wjj; touch wj; ls -ld wj*
```



## 8. [Monitoring and Managing Linux Processes](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/70)

#### gnome-system-monitor

#### ps

```bash
# ps -aux
# ps -aux | head -n 2
# pstree
# pstree -p
```

#### top

```bash
# top
```

<kbd>h</kbd>, <kbd>q</kbd>

<kbd>f</kbd>, <kbd>ArrowDown</kbd>

<kbd>Space</kbd>, <kbd>s</kbd>

<kbd>k</kbd>, <kbd>Enter</kbd>, <kbd>9</kbd>

<kbd>q</kbd>

#### kill

```bash
# kill -l
# kill -9 1234
```

#### jobs, bg, fg

```bash
# jobs
# dd if=/dev/zero of=/dev/null bs=1K
<Ctrl-Z>
# jobs
# bg
# dd if=/dev/zero of=/dev/null bs=2K &
# jobs
# fg 2
<Ctrl-C>
# kill %1
```

####  uptime

```bash
# top
# uptime
# w
```



## 9. [Controlling Services and Daemons](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/81)

#### systemd

```bash
# ps -axu | head -n 2
```

#### systemctl

```bash
kernel
# sysctl -a | grep -i ipv4.*forward

# systemctl help
# systemctl help list-units
# systemctl -t help
# systemctl list-units 
# systemctl list-units -t service
# systemctl list-unit-files | wc -l
# systemctl list-units | wc -l

- status
RHEL>=7# systemctl status sshd
RHEL<=6# service sshd status

- is-enabled, is-active
# systemctl is-enabled sshd
# systemctl is-active sshd

- start, stop
# systemctl is-active sshd
# systemctl stop sshd
# systemctl is-active sshd
# systemctl start sshd
# systemctl is-active sshd

- enable, disable
# systemctl is-enabled sshd
# systemctl disable sshd
# systemctl is-enabled sshd
# systemctl enable sshd
# systemctl is-enabled sshd

- restart(stop && start), reload
# systemctl status sshd
# systemctl restart sshd
# systemctl status sshd
# systemctl reload sshd
# systemctl status sshd

- list-dependencies
# systemctl list-dependencies 
# systemctl list-dependencies | grep target

- mask, unmask
# systemctl mask atd
# systemctl stop atd
# systemctl start atd || echo no
# systemctl status atd
# systemctl unmask atd
# systemctl start atd
# systemctl status atd
```

|         | RHEL<=6               | RHEL>=7                  |
| ------- | --------------------- | ------------------------ |
| start   | service DAEMON start  | systemctl start DAEMON   |
| stop    | service DAEMON stop   | systemctl stop DAEMON    |
| status  | service DAEMON status | systemctl status DAEMON  |
| enable  | chkconfig DAEMON on   | systemctl enable DAEMON  |
| disable | chkconfig DAEMON off  | systemctl disable DAEMON |



## 10. [Configuring and Securing SSH](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/88)

#### ssh

```bash
[foundation]
# rht-vmctl start classroom 
# echo -e "172.25.254.254\tc" >> /etc/hosts
# ping -c 4 c
# rm -rf .ssh/config 

# whoami
1# ssh c
2# ssh instructor@c
3# ssh instructor@c 'mkdir ~/newfolder'
# ssh instructor@c 'ls -l'

# ll /etc/ssh/ssh_host_*
```

#### ssh-keygen, ssh-copy-id

```bash
# rm -rf .ssh/
# ssh-keygen 
<Enter>*3
# ls .ssh/
# ssh-copy-id instructor@c
# ssh 'instructor@c'
 
# cat -n .ssh/id_rsa.pub 
# ssh instructor@c 'cat -n .ssh/authorized_keys'

-i
# ssh-keygen 
/root/id_new
<Enter>*2
# ls /root/id*
# ssh-copy-id -i ~/id_new.pub root@c
# ssh -i ~/id_new 'root@c'
```

#### sshd_config

**[classroom]**

```bash
# yum search ssh
# rpm -qc openssh-server
# vim /etc/ssh/sshd_config
...
PermitRootLogin no
OPERATION: /Root, w, C, no, <Esc>, <Z><Z>
# rpm -ql openssh-server | grep service
# systemctl restart sshd
# systemctl enable sshd
```

**[foudation]**

```bash
# ssh root@c
Asimov
# ssh instructor@c
$ su -
Asimov
# 
```

**[classroom]**

```bash
# vim /etc/ssh/sshd_config 
...
PasswordAuthentication no
OPERATION:
 /Pass, 2n, w, C, no, <Esc>, :x
# systemctl restart sshd
```

**[foundation]**

```bash
$ ssh instructor@c && echo ok
# ssh instructor@c || echo no
```



## 11. [Analyzing and Storing Logs](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/97)

#### log

```bash
# ls -l /var/log

rsyslog
# vim /etc/rsyslog.conf

logger
# man logger
# tail -f -n 1 /var/log/secure
<Ctrl+Shift+T>
# logger -p authpriv.info suibian
<Alt+1>
```

#### messages

```bash
# vim /etc/ssh/sshd_config
...
PermitRootLogin no:wq
# systemctl restart sshd
# systemctl status sshd
# vim /var/log/messages
<G>
# vim /etc/ssh/sshd_config
...
PermitRootLogin no
# systemctl restart sshd
# systemctl status sshd
```

**[workstation]**

```bash
# yum list httpd
# yum -y install httpd
# systemctl restart httpd
# systemctl status httpd
# rpm -qc httpd
# vim /etc/httpd/conf/httpd.conf 
...
Listen 83
# systemctl restart httpd || echo no
# systemctl status httpd
# vim /var/log/messages 
<G>
TIP: semanage port -a -t PORT_TYPE -p tcp 83
# man semanage port | grep semanage.*port
# semanage port -l
# semanage port -l | grep http | grep 80
# semanage port -a -t http_port_t -p tcp 83
# systemctl restart httpd && echo ok
# systemctl status httpd
```

#### logrotate

```bash
# vim /etc/cron.daily/logrotate
# vim /etc/logrotate.conf
# vim /etc/logrotate.d/syslog
```

#### systemd-journald

```bash
# systemctl status systemd-journald.service
# ls /run/log/
# man journalctl
# journalctl 
# journalctl -n 5
# journalctl -f
# journalctl -p err
# journalctl --since today
# journalctl --since "-1 hour" 
# journalctl --since "2019-12-22 08:30:00" \
--until "2019-12-22 11:00:00"
# journalctl _SYSTEMD_UNIT=sshd.service
```

```bash
# systemctl status systemd-journald.service 
# rpm -qf /usr/lib/systemd/system/systemd-journald.service 
# rpm -qc systemd
# man journald.conf
/storage
# vim /etc/systemd/journald.conf
...
Storage=persistent
# mkdir /var/log/journal
# ls -ld /run/log/journal
# chown root:systemd-journal /var/log/journal
# chmod 2755 /var/log/journal
# ls -ld /var/log/journal
# systemctl restart systemd-journald.service 
# systemctl status systemd-journald.service 
# ls /var/log/journal/
```

#### chrony

```bash
# timedatectl 
# timedatectl list-timezones 
# timedatectl list-timezones | grep -i to
# timedatectl set-timezone Asia/Tokyo
# timedatectl 

# systemctl list-units | grep -i ntp
# systemctl status chronyd.service
# rpm -qf /usr/lib/systemd/system/chronyd.service
# rpm -qc chrony-3.3-3.el8.x86_64
# vim /etc/chrony.conf
...
server 172.25.254.254 iburst
# ping -c 4 172.25.254.254
# ping -c 4 classroom

# timedatectl set-time 11:33 || echo no
# timedatectl set-ntp false 
# timedatectl 
# timedatectl set-time 11:33 && echo ok
# timedatectl 
# timedatectl set-ntp true
# timedatectl 
# sleep 15
# timedatectl 
```



## 12. [Managing Networking](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/110)

|      |                      |                                                           |
| ---- | -------------------- | --------------------------------------------------------- |
| 1    | IP/(NETMASK\|PREFIX) | 172.25.0.9/255.255.0.0 \| 172.25.0.9/16                   |
| 2    | GATEWAY              | 172.25.x.x                                                |
| 3    | DNS                  | 正向解析 # host servera，<br />反向解析 # host 172.25.0.9 |

网段：IP与掩码二进制与运算 

|          |                |           |
| -------- | -------------- | --------- |
| 网络地址 | 172.25.0.0     | 主机位全0 |
| 广播地址 | 172.25.255.255 | 主机位全1 |

ip4,ip6

```bash
# ip address show enp1s0
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:00:fa:0a brd ff:ff:ff:ff:ff:ff
    inet 172.25.250.10/24 brd 172.25.250.255 scope global noprefixroute enp1s0
       valid_lft forever preferred_lft forever
    inet6 fe80::e6c5:468e:edb6:9b52/64 ...
```



|              |  ipv4  |   ipv6   |   mac    |
| :----------: | :----: | :------: | :------: |
| 二进制（位） |   32   |   128    |    48    |
|  符号（分）  |   .    |    :     |    :     |
|     进制     | 十进制 | 十六进制 | 十六进制 |
|      组      |   4    |    8     |    6     |

#### ping,ping6

```bash
# ping -c4 172.25.250.10

RHEL<=7
# ping6 2001::1
x# ping6 fe80::e6c5:468e:edb6:9b52 || echo no
1# ping6 fe80::e6c5:468e:edb6:9b52%enp1s0
2# ping6 fe80::e6c5:468e:edb6:9b52 -I enp1s0

RHEL8
# ping6 2001::1
# ping6 fe80::e6c5:468e:edb6:9b52
```

#### gateway

```bash
1# nmcli connection show 'Wired connection 1' | grep ipv4.ga
2# ip route
3# netstat -nr
```

#### dns

```bash
# cat /etc/resolv.conf
```

#### hostname

```bash
RHEL<6
# hostname
RHEL>=7
# hostnamectl
```

#### nmcli

```bash
# nmcli dev status 
# nmcli connection show

# ls /etc/sysconfig/network-scripts/ifcfg-*

-dhcp
# nmcli connection modify 'Wired connection 1' \
connection.autoconnect yes \
ipv4.method auto
# systemctl restart NetworkManager
# ip a s
-static
# nmcli con mod 'Wired connection 1' \
connection.autoconnect yes \
ipv4.method manual \
ipv4.addresses 172.25.250.11/24 \
ipv4.gateway 172.25.250.254 \
ipv4.dns 8.8.8.8
# nmcli connection up 'Wired connection 1' 
# ip a s

# mandb
# man -k nmcli
# man nmcli | grep -A 2 nmcli.*mod

+ipv4.addresses,-ipv4.dns
# nmcli con mod 'Wired connection 1' +ipv4.addresses 172.25.250.10/24
# ifdown 'Wired_connection_1' && ifup 'Wired_connection_1'
# ip a s
# nmcli con mod 'Wired connection 1' -ipv4.addresses 172.25.250.10/24
# ifdown 'Wired_connection_1' && ifup 'Wired_connection_1'
# ip a s
```

#### nmtui

```bash
# nmtui
# nmcli connection up 'Wired connection 1'
```

#### nm-connection-editor

**[foundation|workstation]**

```bash
$ nm-connection-editor
```

#### ifcfg-*

```bash
# vim /usr/share/doc/initscripts/sysconfig.txt 
/ifcfg-, <Ctrl+F>/<Ctrl+B>
# vim /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1
...
PREFIX=16
# nmcli connection up 'Wired connection 1' 
# ip a s
```

#### tracepath

```bash
# tracepath 172.25.0.250
# tracepath 172.25.250.250
```

#### service

```bash
# netstat -antup
# systemctl status sshd

# netstat -antup | grep 22
# ss -antup | grep 22

# grep ssh.*22 /etc/services
```

#### hostnamectl

```bash
RHEL>=7
# hostnamectl set-hostname sa.example.com
# hostname
# cat /etc/hostname

RHEL<7
# hostname sa.example.com
# vim /etc/sysconfig/network
```

#### resolv.conf

```bash
# cat /etc/resolv.conf 
# grep -i dns /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1 
# nmcli con mod 'Wired connection 1' ipv4.dns 8.8.8.8

# grep -i dns /etc/sysconfig/network-scripts/ifcfg-Wired_connection_1 
# cat /etc/resolv.conf && echo no
# nmcli connection up 'Wired connection 1' 
# cat /etc/resolv.conf && echo change
```



## 13. [Archiving and Transferring Files](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/123)

- Partition=inode/个数+block/容量

#### tar

```bash
-c create
-t list
-x extract
# dd if=/dev/zero of=./file1.txt bs=1M count=1
# dd if=/dev/zero of=./file2.txt bs=1M count=2
# ls -lh file*
# du -sh /boot

# man tar
# tar -cf file.tar file[12].txt /boot
# echo $?

# tar -tf file.tar 
# du -sh /boot
# ls -lh file[12]*
# ls -lh file*

# rm -rf file[12]*
# ls -lh file*
# tar -xf file.tar 
# ls file*
# ls -ld boot/
# du -sh boot/
# tar -xf file.tar -C /tmp
# ls -ldh /tmp/{file*,boot}

-z gzip
-j bzip2
-J xz
# man tar
# tar -czf file.tar.gz file[12]*
# tar -cjf file.tar.bz2 file[12]*
# tar -cJf file.tar.zx file[12]*
# ls -lh file*
# file file.tar.zx 
# tar -tf file.tar.zx
# tar -xf file.tar.zx -C /home
```

#### scp | [winscp](https://winscp.net/eng/docs/interfaces)

```bash
local to remote
# rht-vmctl start classroom 
# ping -c 4 classroom.example.com
# scp file.tar.gz instructor@classroom.example.com:/tmp
# ssh instructor@classroom.example.com "ls -l /tmp/file*"

remote to remote
# scp instructor@classroom.example.com:/tmp/file.tar.gz kiosk@172.25.254.250:~
# ls -l ~kiosk/file.tar.gz 

remote to local
# scp instructor@classroom.example.com:/tmp/file.tar.gz /home
# ls -l /home/
```

#### sftp

|  ID  |       |                |
| :--: | :---: | -------------- |
|  1   |  ftp  | client         |
|  2   | sftp  | ssh SubService |
|  3   | vsftp | service        |

```bash
# sftp instructor@classroom.example.com 
sftp> pwd
Remote working directory: /home/instructor
sftp> !pwd
/root
sftp> cd /tmp
sftp> lcd /home/kiosk
sftp> pwd
Remote working directory: /tmp
sftp> !pwd
/home/kiosk
sftp> !ls
ClassPrep.txt		      ...
sftp> put ClassPrep.txt
Uploading ClassPrep.txt to /tmp/ClassPrep.txt
ClassPrep.txt    100%   31KB   9.4MB/s   00:00    
sftp> ls
ClassPrep.txt     NIC1       ...
sftp> get NIC1
Fetching /tmp/NIC1 to NIC1
/tmp/NIC1    100%    7     1.6KB/s   00:00    
sftp> !ls N*
NIC1
sftp> exit
```

#### rsync

```bash
# cp -a /boot /tmp

# ls -ld home || echo no
# rsync -av root@classroom.example.com:/home .
# ls -ld home/
# ls -l home/

# rsync -av root@classroom.example.com:/home .

- append
# ssh root@classroom.example.com 'touch /home/new.file'
# rsync -av root@classroom.example.com:/home .
# ls home
# cat home/new.file

- modify
# ssh root@classroom.example.com 'echo haha > /home/new.file'
# rsync -av root@classroom.example.com:/home .
# cat home/new.file

- delete
# ssh root@classroom.example.com 'rm -rf /home/new.file'
# rsync -av root@classroom.example.com:/home .
# ls home/
# cat home/new.file 
```



## 14. [Installing and Updating Software Packages](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/132)

#### rpm

```bash
-q query all
# rpm -qa
# rpm -qa | grep cifs-utils
# rpm -qa | grep samba-
# find /content/rhel8.0/x86_64/dvd/ -name samba*rpm
# rpm -qip /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-4.9.1-8.el8.x86_64.rpm
# rpm -qlp /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-4.9.1-8.el8.x86_64.rpm
# rpm -qcp /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-4.9.1-8.el8.x86_64.rpm
# rpm -qdp /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-4.9.1-8.el8.x86_64.rpm

- i install, verbose, hash
# rpm -ivh /content/rhel8.0/x86_64/dvd/BaseOS/Packages/samba-4.9.1-8.el8.x86_64.rpm || echo no
# rpm -qa | grep autofs
# rpm -qi autofs

-e erase
# rpm -e autofs
# rpm -q autofs
# find /content/rhel8.0/x86_64/dvd/ -name autofs*rpm
# rpm -ivh /content/rhel8.0/x86_64/dvd/BaseOS/Packages/autofs-5.1.4-29.el8.x86_64.rpm
# rpm -q autofs

config
# rpm -qc autofs
# rpm -qc samba-common
from
# rpm -qf /etc/samba/smb.conf
# rpm -qd autofs
list
# rpm -ql autofs
# rpm -q httpd && rpm -q --scripts httpd
# rpm -q httpd || rpm -qp --scripts /content/rhel8.0/x86_64/dvd/AppStream/Packages/httpd-2.4.37-10.module+el8+2764+7127e69e.x86_64.rpm
# rpm -q httpd || rpm -qpi /content/rhel8.0/x86_64/dvd/AppStream/Packages/httpd-2.4.37-10.module+el8+2764+7127e69e.x86_64.rpm

# rpm -e autofs
# rpm -ivh http://foundation0.ilt.example.com/rhel8.0/x86_64/dvd/BaseOS/Packages/autofs-5.1.4-29.el8.x86_64.rpm
# rpm -q autofs
 
# rpm -e autofs
# wget http://foundation0.ilt.example.com/rhel8.0/x86_64/dvd/BaseOS/Packages/autofs-5.1.4-29.el8.x86_64.rpm
# rpm -ivh autofs-5.1.4-29.el8.x86_64.rpm
```

#### yum | dnf

```bash
# yum list httpd
# yum list http*

*# yum search ntp

# yum info chrony.x86_64

# yum provides *smb.conf

# yum list autofs
# yum erase autofs
# yum list autofs
# yum -y erase autofs

# yum install autofs
# yum install -y autofs
# yum list autofs
# yum history
# yum clean all

# yum grouplist
# yum groupinstall 'Virtualization Host'
```

```bash
# ls /etc/yum.repos.d/
# vim /etc/yum.conf 

# yum repolist
# rm -rf /etc/yum.repos.d/*.repo
# yum repolist

# yum provides yum-config-manager
# rpm -ivh http://training.ilt.example.com/rhel8.0/x86_64/dvd/BaseOS/Packages/dnf-utils-4.0.2.2-3.el8.noarch.rpm

# yum-config-manager -h
# yum-config-manager --disable rht-ext
# yum repolist
# cat -n /etc/yum.repos.d/rht-ucf.repo 
# yum-config-manager --enable rht-ext
# yum repolist
# cat -n /etc/yum.repos.d/rht-ucf.repo 
# cat -n /etc/yum.repos.d/rhel-dvd.repo 
# yum repolist

# yum-config-manager -h
# yum-config-manager --add-repo file:///content/ucf/
# ls /etc/yum.repos.d/
# yum-config-manager --add-repo http://foundation0.ilt.example.com/rhel8.0/x86_64/dvd/BaseOS/
# yum-config-manager --add-repo http://foundation0.ilt.example.com/rhel8.0/x86_64/dvd/AppStream/
# ls /etc/yum.repos.d/

GPG-KEY1
# yum repolist
# yum -y install autofs || echo no
# rpm --import /content/rhel8.0/x86_64/dvd/RPM-GPG-KEY-redhat-release 
# yum -y install autofs
# rpm -qa | grep pub
# rpm -qa | grep pub
# rpm -e gpg-pubkey-fd431d51-4ae0493b gpg-pubkey-d4082792-5b32db75 gpg-pubkey-530679ee-4f6b7813 
# rpm -qa | grep pub
# yum -y erase autofs
# yum -y install autofs || echo no

GPG-KEY2
# vim /etc/yum.repos.d/foundation0.ilt.example.com_rhel8.0_x86_64_dvd_BaseOS_.repo 
...
gpgcheck=0
# yum -y install autofs
# yum -y erase autofs

GPG-KEY3
# yum -y install autofs || echo no
# vim /etc/yum.repos.d/foundation0.ilt.example.com_rhel8.0_x86_64_dvd_BaseOS_.repo 
...
gpgkey=http://foundation0.ilt.example.com/rhel8.0/x86_64/dvd/RPM-GPG-KEY-redhat-release
# yum -y install autofs
```



## 15. [Accessing Linux File Systems](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/145)

#### Disk

|                |                      |        |
| :------------- | -------------------- | ------ |
| /dev/sd{a..zz} | SATA, SAS, SCSI, USB | 热插拔 |
| /dev/hd[a..z]  | IDE                  |        |
| /dev/vd{a..zz} | VirtIO               |        |
| /dev/nvme0n1   | nvme                 |        |
#### filesystem type

|         |              |            |        |
| ------- | ------------ | ---------- | ------ |
| Windows | fat16, fat32 | ntfs       | exfat* |
| Linux   | xfs          | ext3, ext4 | exfat  |
| MacOS   | hfs          |            | exfat* |

```bash
# df -hT
# df -ht xfs
```



#### mount, umount

```bash
# lsblk 
# blkid  /dev/nvme0n1p4

# mkdir /mnt/dev
# mount /dev/nvme0n1p4 /mnt/dev
# ls /mnt/dev
# mkdir /mnt/uuid
# mount UUID="a8d55acd-6425-4273-8e30-4c340587ba0b" /mnt/uuid
# ls /mnt/uuid
# df -h /mnt/dev/
# df -h /mnt/uuid/
# df -h

# umount /mnt/dev 
# mount | grep /mnt/dev
# ls /mnt/dev
# umount /dev/nvme0n1p4
# mount | grep nvme0n1p4
```

#### locate, find

```bash
# ls file*
# locate file.tar || echo no
# updatedb &
# jobs
# locate file.tar && echo ok
 
# find / -name file.tar
# find / -name file.tar 2>/dev/null
# man find
# find / -user kiosk
# man find | grep find.*exec
# find /home/ -user kiosk -exec cp -a {} /mnt/dev \;
# ls -l /mnt/dev/
```

#### ln

```bash
# ls -l file2.txt 
# du -sh .

# ln file2.txt hard
# ls -lh file2.txt hard
# du -sh .
# ls -lih file2.txt hard
# man find | grep inum
# find / -inum 540914070

# ln -s file2.txt soft
# ls -lih file2.txt soft 
```

#### dd, df, du

```bash
# dd if=cd.iso of=/dev/sdb

# df -h

# du -sh /boot
```



## 16. [Analyzing Servers and Getting Support](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/154)

#### cockpit

```bash
# yum search cock
# yum list cockpit
# systemctl list-unit-files | grep cock
# systemctl status cockpit.service
# systemctl start cockpit.service
# systemctl enable cockpit.service
# echo $?
# ss -antup | grep cock
# firewall-cmd --list-all
```

<kbd>Alt</kbd>+<kbd>F2</kbd> / `firefox http://127.0.0.1:9090`

root%Asimov



## 17. [Comprehensive Review](http://foundation0.ilt.example.com/slides/RH124-RHEL8.0-en-1-20190531/#/162)

## Appendix

#### [Typora](https://typora.io/) Markdown编辑器


#### SHELL快捷键

|      |                                                           |                                              |
| ---- | --------------------------------------------------------- | -------------------------------------------- |
| 1    | <kbd>Esc</kbd><kbd>.</kbd> \| <kbd>Alt</kbd>+<kbd>.</kbd> | 自动粘贴上一条命令最后一个词，到光标所在位置 |
|      |                                                           |                                              |
|      |                                                           |                                              |

#### &&, ||

```bash
# echo dui && echo zhixing
# Echo cuo || echo zhixing
```

#### virsh

**[foundation]**

```bash
# virsh list
# virsh console servera
root%redhat
...
# <Ctrl+D>
<Ctrl+]>
```

#### share

```
\\10.10.16.21\Share
```

#### Terminal

|      |                                               |      |
| ---- | --------------------------------------------- | ---- |
|      | <kbd>Ctrl</kbd>+<kbd>+</kbd>                  |      |
|      | <kbd>Ctrl</kbd>+<kbd>-</kbd>                  |      |
|      | <kbd>Win</kbd>+<kbd>:arrow_up:</kbd>          |      |
|      | <kbd>Win</kbd>+<kbd>:arrow_left:</kbd>        |      |
|      | <kbd>Win</kbd>+<kbd>:arrow_right:</kbd>       |      |
|      | <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>T</kbd> |      |
|      | <kbd>Alt</kbd>+<kbd>1</kbd>                   |      |
|      |                                               |      |
|      |                                               |      |

#### SERVICE

| step | CMD                          | DEMO                  |
| :--: | ---------------------------- | --------------------- |
|  1   | yum search KEYWORD           | ssh                   |
|  2   | rpm -qc PKG                  | openssh-server.x86_64 |
|  3   | vim CFG                      | /etc/ssh/sshd_config  |
|  4   | rpm -ql PKG \| grep service  | openssh-server.x86_64 |
|  5   | systemctl restart DAEMON     | sshd                  |
|  6   | systemctl enable DAEMON      |                       |
|  7   | firewall-cmd --permanent ... |                       |
|  8   | security                     |                       |

#### Simulation

| STEP |                                                              |                    |
| :--: | ------------------------------------------------------------ | ------------------ |
|  1   | VMware/ <kbd>恢复快照...</kbd> -=> `INIT`                 | 恢复快照           |
|  2   | 启动虚拟机 |                |
|  3   | 插入`ex*.iso`，记得复选<kbd>连接 CD/DVD </kbd> |                |
|  4   | `ssh root@localhost` | 切换到root身份               |
|  5   | `yum install -y /run/media/kiosk/<Tab>/<Tab>` | 安装，注意提示         |
|  6   |  <kbd>Ctrl</kbd>+<kbd>D</kbd>  |  退出root，返回kiosk     |
|  7   | `exam-setup`                                                 | 设置，绿色黄色回显       |
|  8   | <kbd>Win</kbd> / <kbd>View Exam</kbd>                        |  模拟考试题（网页） |
|  9   |  `exam-grade`                                                 | 判分               |