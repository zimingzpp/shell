# 第一章、小试牛刀

---

## 1.终端打印

- echo和printf

```sh
#!/bin/bash
#printf.sh
printf "%-5s %-10s %-4s\n" No Name Mark
printf "%-5s %-10s %-4.2s\n" 1 Sam 90
printf "%-5s %-10s %-4.2s\n" 2 Jenny 75
printf "%-5s %-10s %-4.2s\n" 3 kali 84
```

- 彩色输出<br>

重置=0，黑色=30，红色=31，绿色=32，黄色=33，蓝色=34，洋红=35，青色=36，白色=37
打印彩色文本，输入命令:

```sh
echo -e "\e[1;31m This is red text \e[0m"
```

彩色背景，重置=0，黑色=40，红色=41，绿色=42，黄色=43，蓝色=44，洋红=45，青色=36，白色=37

要打印彩色背景，可输入命令:

```sh
echo -e "\e[1;41m This is red background \e[0m"
```

## 2.玩转变量和环境变量

### 预备知识

环境变量可以用env命令查看，对于进程来说，其运行时的环境变量可以使用下面的命令

`cat /proc/$PID/environ`

其中，将PID设置成相关进程的进程ID，假设有一个叫做gedit的应用程序正在运行。我们可以使用pgrep命令获得gedit的进程ID：

`$ pgrep gedit`
`12501`

那么，你就可以通过以下命令获得与该进程相关的环境变量：

```sh
$ cat /proc/12501/environ
GDM_KEYBOARD_LAYOUT=usGNOME_KEYRING_PID=1560USER=slynuxHOME=/home/slynux
```

### 补充内容

1.获得字符串长度

可以用下面的方法获得变量值的长度：

`length=$(#var)`

例如：

```bash
$var=12345678901234567890
$echo $(#var)
```

length就是字符串所包含的字符数

2.检查是否为超级用户



```shell
If [ $UID -ne 0 ]; then
	echo Non root user. Please run as root.
else 
	echo Root user
fi
```

3.修改Bash提示字符串（username@hostname:~$）

当我们打开终端或是运行shell时，会看到类似于user@hostname:/home/$的提示字符串。
不同GNU/Linux发布版中的提示及颜色略有不同。我们可以利用PS1环境变量来定制提示文本。
默认的shell提示文本是在文件~/.bashrc中的某一行设置的。

可以使用如下命令列出设置变量PS1的那一行：

```sh
$ cat ~/.bashrc | grep PS1
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
```

如果要设置一个定制的提示字符串，可以输入：

```sh
slynux@localhost: ~$ PS1="PROMPT>"
PROMPT> Type commands here #提示字符串已经改变
```

我们可以利用类似\e[1;31的特定转义序列来设置彩色的提示字符串（参考1.2节的内容）。
还有一些特殊的字符可以扩展成系统参数。例如：\u可以扩展为用户名，\h可以扩展为主
机名，而\w可以扩展为当前工作目录。

4.获取终端信息

- 获取终端的行数和列数：
```sh
tput cols
tput lines
```
- 打印出当前终端名：`tput longname`
-  将光标移动到坐标(100,100)处：`tput cup 100 100`
-  设置终端背景色：`tputsetb n`，其中，n可以在0到7之间取值
-  设置文本前景色：`tputsetf n`，其中，n可以在0到7之间取值
-  设置文本样式为粗体：`tput bold`
-  设置下划线的起止：
```
tput smul
tput rmul
```
- 删除从当前光标位置到行尾的所有内容：`tputed`
- 在输入密码时，不应该显示输入内容。在下面的例子中，我们将看到如何使用stty来实
现这一要求：
```sh
#!/bin/sh
#Filename: password.sh
echo -e "Enter password: "
stty -echo
read password
stty echo
echo
echo Password read.
```
_选项`-echo`禁止将输出发送到终端，而选项`echo`则允许发送输出_

5.使用Shell进行数学计算

简单运算可以使用let、(())和[]执行基本操作。而进行高级操作时，使用expr和bc

- 使用let如下:
```shell
#!/bin/bash
no1=4
no2=5
let result=no1+no2
echo $result
```

- 其他方法

操作符[]的使用方法和let命令类似：`result=$[ no1 + no2 ]`
