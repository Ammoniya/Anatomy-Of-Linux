# Linux
![t-shirt-tuxedo-linux-t-shirt-png-clip-art](https://user-images.githubusercontent.com/20130001/85932004-1c006c00-b8e6-11ea-99df-a54d4bae740f.png)
### Daytoday Shell Digest
#### file hierarchy
![standard-unix-filesystem-hierarchy](https://user-images.githubusercontent.com/20130001/61242287-f3c63e80-a762-11e9-8c45-8446392fbe99.png)
#### create user
```
sudo adduser <user name>
sudo passwd <user name>

sudo groupadd <user group name>

sudo usermod -aG <user group> newuser [Ex-sudo]
```
#### file navigation
```
cd
ls , ls -a , ls -A, ls -l, ls -a -l, ls -A -l ,ls -a -l --color
mkdir , rmdir
rm , rm -rf
cp , mv (move and rename)

history
history -c
history -d <line number>
```
#### file manipulation
```
view - more, less, head, tail, cat
edit - vim, nano, gedit
create empty - touch
>>,>,|
&& - left after right
&  - left run on background
```

#### services
```
service --status-all
	+ running
	- not running
	? can not be determined (WTF services)
service <service name> start/stop/restart/reload/status
```
```
systemctl list-unit-files
	static
	enabled
	disabled

systemctl start/stop/restart/reload/status <service name>
```
#### explore services and process running on ports
```
*view services and ports
netstat -a		Everyting 
netstat -ntl  		TCP only
netstat -nul  		UDP only
netstat -tulpn 		Active listening and Active listening both TCP/UDP
netstat -plnt   	Only Listening


sudo lsof -i :<port number>     more details in NAME
sudo lsof -n -i :<port number>	less details in NAME
sudo lsof -t -i :<port number>	view only por numbers
```
#### process
```
top 
htop ( sudo apt-get install htop)
	* M – sort by memory
	* P – sort by CPU
	* T – sort by time

ps
ps -A
ps -le			(Ex - sudo ps -le | grep apache2)
	UID		- user identification number
	PID		- process identification number
	PPID	- parent process identification number
	PRI		- priority  of the process 
			PR is the process's actual priority that use by Linux kernel
					range - (0 , 139) 
					(0 to 99 for real time and 100 to 139 for users)
	NI		- nice value of the process
			In Linux we can set guidelines for the CPU to follow 
			when it is looking at all the tasks it has to do.
			These guidelines are called niceness or nice value
					range - (-20 to 19)
					* The lower the number the more priority that task gets
					* niceness value is high number like 19 the task will be 
					set to the lowest priority 
         			        * -20 is highest, 0 default and +19 is lowest
                                        * PR = 20 + NI
                                        * default nice in /etc/security/limits.conf
	TTY		- terminal type
ps -p pid1,pid2,pid3....
ps -C <program or service or script name>
ps -G <group name>

*change nice value
nice -n <nice value> <process name> [Ex : nice -n 10 chrom]
renice <nice value> -p <pid>        [Ex : renice 10 -p $(pgrep apache2)]

*view 5 scheduling policies in a LINUX environment (IDK ,modify and apply those policies)
chart -m

pstree
pstree -a

pgrep <program or service or script name> (pgrep apache | wc -l)

kill    <pid>
pkill   <process name>
killall <process name>

cTrl + D (throw to background)
```
#### file system
```
* hard link
ln <source> <link>
rm <hard link>

* soft link
ln -s <source> <link>
rm <soft link>

stat <dir>    - inode info of dir
stat -f <dir> - inode info of file system

#disk file system
df -h     - human readable
df -m     - in MB
df -i     - inode info
df -T     - file system type
df -t/-x <file type>    - include/exclude specific file system type
df -hT <directory>      - directory info

ls -i
stat
find <> -inum 
find <> -inum <delete/?>

du -a -h <folder>  //disk usage free space

sudo fdisk -l - HDD info
sudo badblocks -v /dev/sda*
```
#### :stars: working with magic numbers in Linux (file signature)
* A magic number is a sequence of bytes that is used in all files of a certain format, usually at a given position (often at the beginning). Since all files in that particular format have that particular byte sequence in that particular position, and most files in other formats don't have it, the magic number is a way to recognize what format a file is in
* These bytes can be used by the system to “differentiate between and recognize different files” without a file extension
* Most files have the signatures in the bytes at the beginning of the file, but some file systems may even have the file signature at   offsets other than the beginning. For example, file system **ext2/ext3** have bytes **0x53** and **0xEF** at the **1080th** and **1081st** position
*  A reiserfs filesystem always has **ReIsErFs** starting at position **65588** *(or ReIsEr2Fs, etc., in more recent versions)*
* Magic numbers/File signatures are typically not visible to the user, but can be seen by using a hex editor or by using the ```xxd``` command as mentioned below. These bytes are essential for a file to be opened.
* Changing/corrupting these bytes will render the file useless as most tools will not access these files due to potential damaging
* Some files however, do not have magic numbers, such as plain text files, but can be identified by checking the character set **(ASCII in the case of text files)**
- can be explore it using **file** command as follows
- ``` file -b <filename> ```
* Find all the list of all signatures [view](https://en.wikipedia.org/wiki/List_of_file_signatures) 
```
file <file name>
file -b <bile name>

* decisions are based on a database typically found in 
	/etc/magic or 
	/usr/share/misc/magic
	
file -c </dev/sda*>
file -c <file name>

```
#### searching things
```
whereis - binary, source code, man page
which   - binary
locate  - 
find    -
```
#### utilities 
```
grep <reg expression> (sudo ps -le | grep apache2)
grep -i <reg expression> ~ force grep to ignore word case in regX

sort <file name>  (Ex sort foo.txt > sortfoo.txt)
sort -r <file name>
sort -n <fiile name> 
sort -n -r <file name>

wc -l   - lines
wc -w   - words
wc -m   - characters

dpkg -i <pkg>
tar -xvzf <*.tar.gz>

apt-get install/update/upgrade/remove/list/search <pkg from repo>
add-apt-repository ppa:<pkg>/ppa
```
#### permissions and security
```
whoami 

uname
uname -a

chmod , umask (etc/bashrc or etc/profile)

sudo chown $USER:[$USERGROUP] <file>
sudo chown -R $USER:[$USERGROUP] <dir>

adduser <user name>
adduser -d <custom home path> <user name>
adduser -M <user name> # create user without home directory
adduser -u <uid> <user name>
adduser -u <uid> -g <gid> <user name>
adduser -G <group/s> <username>

w - who logged in 
w <user name>

```
#### config
```
/etc/environment - environment variable
~/.profile - set path etc
~/. bashrc - alias etc
source active ~/.bashrc or ~/.profile
```

### env
```
printenv - view environment variables
```
* USER – your current username.
* SHELL – the path to the current command shell (for example, /bin/bash).
* PWD – the current working directory.
* HOSTNAME – the hostname of the computer.
* HOME – your home directory.
* MAIL – the location of the user's mail spool.

#### pci-bus information 
```
lspci - default
lspci -v  - info
lspci -vv - more info
lspci -t - tree output
```
#### network troubleshooting
note :- *ethX* interpret the network interface<br>
view network interface naming conventions 
Two character prefixes based on the type of interface<br>
-   en — Ethernet
-   ib — InfiniBand
-   sl — serial line IP (slip)
-   wl — wlan
-   ww — wwan
```
lspci -v/-vv | grep -i <ethernet/wireless> - find hardware

sudo tail /var/log/messages
sudo more /sys/class/net/ethX/carrier 		- [0/1 - up/down]
sudo more /sys/class/net/ethX/operstate		- []
/etc/init.d/networking restart

netstat -rn

ip a					- view all interfaces 
ip addr show				- view all interfaces 

ip [a/addr] [show/list] ethX 	- show only the interface
 
ip addr add <ip/cidr> dev ethX	- add ip
ip addr del <ip/cidr> dev ethX	- del ip

ip link set ethX up 		- start 
ip link set ethX down		- stop

ip route show					- view route 
ip route add <ip/cidr> via <ip/cidr> dev eth0 	- add static ip route to ip
ip route del <ip/cidr> 				- remove static ip
ip route add default via <ip/cidr>	 	- add default gateway


ifconfig				- view info
ifconfig -a				- details output 
ifconfig ethX				- view 

ifconfig ethX <ip>			- add ip to del use [ip addr del <ip/cidr>
ifconfig ethX netmask <mask not CIDR>   - add subnet mask

ipconfig ethX up 			- up connection
ipconfig ethX down 			- down connection

ifconfig mtu <ammount>			- change MTU

*iwconfig ~ [Manage Wireless LAN in Linux]

ethtool ethX		- check eth stat
ethtool -i ethX		- check eth stat
ethtool -S ethX 	- check eth stat
ethtool -t <devices> 	- check eth test

##IP Aliasing – Assigning multiple IP addresses to the same network
interface##

route
route -n
route add default gw <ip>

/etc/network/interfaces

iface <inter face> inet static
Address <ip>
netmask <mask ip>
broadcast <ip>
gateway <ip>

** change Name server **
/etc/ resolv.conf  

# nameserver <ip>

ping <ip>
ping -I <inter face> <ip>
ping -i <int> <ip>
ping -c <int> <ip>
```


#### Linux Kernel Health
![linux_observability_tools](https://user-images.githubusercontent.com/20130001/82738189-83f5ec80-9d53-11ea-8e92-990e0c8af6cd.png)

#### Detecting Virtualization
```
dmidecode -s system-product-name
dmidecode -t system|grep 'Manufacturer\|Product'
ls -1 /dev/disk/by-id/
systemd-detect-virt
virt-what
lshw -class system
/proc/cpuinfo 
```
